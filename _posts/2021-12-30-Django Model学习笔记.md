---
layout: post
title:  "Django Model 学习笔记"
date:   2021-12-30 13:43:00 +0800
categories: [工作]
---

## Models: Define your data, essential fields, and behaviors of your data.
Each model maps to a single database table.

---

### Quick example

```
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```

This example model defines a Person. first_name and last_name are fields of the model.  

```
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
```

---

### Using models

Edit your settings file and change the INSTALLED_APPS.

```
INSTALLED_APPS = [
    #...
    'myapp',
    #...
]
```

When you add new apps to INSTALLED_APPS, be sure to run manage.py migrate, optionally making migrations for them first with manage.py makemigrations.

---

## Fields

Fields: class attributes == database fields  
In-built APIs: clean, save, or delete......  
name conflict

```
from django.db import models

class Musician(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    instrument = models.CharField(max_length=100)

class Album(models.Model):
    artist = models.ForeignKey(Musician, on_delete=models.CASCADE)
    name = models.CharField(max_length=100)
    release_date = models.DateField()
    num_stars = models.IntegerField()
```

---

### Field types:

AutoField  
BinaryField - max_length  
BooleanField  
CharField - max_length    
DateField  
DecimalField - max_length   
EmailField - max_length = 254  
IntegerField  
TextField  
...  
And another type of fields.  

---

### Field options:

null  
blank  
default  
help_text  
unique  
choices  

```
from django.db import models

class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
```

```
from django.db import models

class Runner(models.Model):
    MedalType = models.TextChoices('MedalType', 'GOLD SILVER BRONZE')
    name = models.CharField(max_length=60)
    medal = models.CharField(blank=True, choices=MedalType.choices, max_length=10)
```

primary_key

```
from django.db import models

class Fruit(models.Model):
    name = models.CharField(max_length=100, primary_key=True)
```

```
>>> fruit = Fruit.objects.create(name='Apple')
>>> fruit.name = 'Pear'
>>> fruit.save()
>>> Fruit.objects.values_list('name', flat=True)
<QuerySet ['Apple', 'Pear']>
```

---

### Verbose field names

```
first_name = models.CharField("person's first name", max_length=30)
```

Except for ForeignKey, ManyToManyField and OneToOneField.   

```
verbose_name=
```

---

## Many-to-one relationships: ForeignKey

```
from django.db import models

class Manufacturer(models.Model):
    # ...
    pass

class Car(models.Model):
    manufacturer = models.ForeignKey(Manufacturer, on_delete=models.CASCADE)
    # ...
```

### on_delete:

```
class Artist(models.Model):
    name = models.CharField(max_length=10)

class Album(models.Model):
    artist = models.ForeignKey(Artist, on_delete=models.CASCADE)

class Song(models.Model):
    artist = models.ForeignKey(Artist, on_delete=models.CASCADE)
    album = models.ForeignKey(Album, on_delete=models.RESTRICT)
```

```
>>> artist_one = Artist.objects.create(name='artist one')
>>> artist_two = Artist.objects.create(name='artist two')
>>> album_one = Album.objects.create(artist=artist_one)
>>> album_two = Album.objects.create(artist=artist_two)
>>> song_one = Song.objects.create(artist=artist_one, album=album_one)
>>> song_two = Song.objects.create(artist=artist_one, album=album_two)
>>> album_one.delete()
# Raises RestrictedError.
>>> artist_two.delete()
# Raises RestrictedError.
>>> artist_one.delete()
(4, {'Song': 2, 'Album': 1, 'Artist': 1})
```

---

## Many-to-many relationships: ManyToManyField

```
from django.db import models

class Topping(models.Model):
    # ...
    pass

class Pizza(models.Model):
    # ...
    toppings = models.ManyToManyField(Topping)
```

### through

```
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=128)

    def __str__(self):
        return self.name

class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(Person, through='Membership')

    def __str__(self):
        return self.name

class Membership(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    date_joined = models.DateField()
    invite_reason = models.CharField(max_length=64)
```

### through_fields

```
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=50)

class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(
        Person,
        through='Membership',
        through_fields=('group', 'person'),
    )

class Membership(models.Model):
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    inviter = models.ForeignKey(
        Person,
        on_delete=models.CASCADE,
        related_name="membership_invites",
    )
    invite_reason = models.CharField(max_length=64)
```

---

## One-to-one relationships: OneToOneField

```
from django.db import models

class Place(models.Model):
    name = models.CharField(max_length=50)
    address = models.CharField(max_length=80)

    def __str__(self):
        return "%s the place" % self.name

class Restaurant(models.Model):
    place = models.OneToOneField(
        Place,
        on_delete=models.CASCADE,
        primary_key=True,
    )
    serves_hot_dogs = models.BooleanField(default=False)
    serves_pizza = models.BooleanField(default=False)

    def __str__(self):
        return "%s the restaurant" % self.place.name

class Waiter(models.Model):
    restaurant = models.ForeignKey(Restaurant, on_delete=models.CASCADE)
    name = models.CharField(max_length=50)

    def __str__(self):
        return "%s the waiter at %s" % (self.name, self.restaurant)
```

```
>>> p1 = Place(name='Demon Dogs', address='944 W. Fullerton')
>>> p1.save()
>>> p2 = Place(name='Ace Hardware', address='1013 N. Ashland')
>>> p2.save()
>>> r = Restaurant(place=p1, serves_hot_dogs=True, serves_pizza=False)
>>> r.save()
>>> r.place
<Place: Demon Dogs the place>
>>> p1.restaurant
<Restaurant: Demon Dogs the restaurant>
>>> from django.core.exceptions import ObjectDoesNotExist
>>> try:
>>>     p2.restaurant
>>> except ObjectDoesNotExist:
>>>     print("There is no restaurant here.")
There is no restaurant here.
>>> hasattr(p2, 'restaurant')
False
```

---

### Import models from another app

```
from django.db import models
from geography.models import ZipCode

class Restaurant(models.Model):
    # ...
    zip_code = models.ForeignKey(
        ZipCode,
        on_delete=models.SET_NULL,
        blank=True,
        null=True,
    )
```

---

### Field name restrictions:
1. Cannot be a Python reserved word
2. Cannot contain more than one underscore in a row
3. Cannot end with an underscore

SQL reserved words, such as join, where or select, are allowed as model field names

---

## Meta options

Model metadata is “anything that’s not a field

```
from django.db import models

class Ox(models.Model):
    horn_length = models.IntegerField()

    class Meta:
        ordering = ["horn_length"]
        verbose_name_plural = "oxen"
```

db_table: name of the database table  
app_label: declare which app it belongs to  

---

## Model inheritance

- abstract base model inheritance
- multi-table inheritance
- proxy inheritance

--- 

### abstract base model inheritance

It does not generate a database table or have a manager, and cannot be instantiated or saved directly.  
Fields inherited from abstract base classes can be overridden with another field or value, or be removed with None.  
Base class Table is not created in This inheritance.  

```
from django.db import models

# Create your models here.

class ContactInfo(models.Model):
	name=models.CharField(max_length=20)
	email=models.EmailField(max_length=20)
	address=models.TextField(max_length=20)

    class Meta:
        abstract=True

class Customer(ContactInfo):
	phone=models.IntegerField(max_length=15)

class Staff(ContactInfo):
	position=models.CharField(max_length=10)
```

---

### multi-table inheritance

```
from django.db import models

# Create your models here.

class Place(models.Model):
	name=models.CharField(max_length=20)
	address=models.TextField(max_length=20)

	def __str__(self):
		return self.name


class Restaurants(Place):
	serves_pizza=models.BooleanField(default=False)
	serves_pasta=models.BooleanField(default=False)

	def __str__(self):
		return self.serves_pasta
```

Base class table is also created in this inheritance.  
it will create a one to one field model relationship for Restaurant table from Place table.  

---

### proxy inheritance

```
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)

class MyPerson(Person):
    class Meta:
        proxy = True

    def do_something(self):
        # ...
        pass
```

This style is used, if you only want to modify the Python level behaviour of the model, without changing the model’s fields.  
You Inherit from base class and you can add your own properties except fields.  
base class should not be abstract class.  
we can not use multiple inheritance in proxy models.  
The main use of this is to overwrite the main functionalities of existing model.  
it always query on original model with overridden methods.  