---
layout: page
permalink: /blog/
title: 博客
---

<div id="archives">
{% for category in site.categories %}
  <div class="archive-group">
    {% capture category_name %}{{ category | first }}{% endcapture %}
    <div id="#{{ category_name | slugize }}"></div>
    <p></p>
    {% if category_name == "CNBlog" %}
      {% for post in site.categories[category_name] %}
        <article class="archive-item">
        <h4><a href="{{ site.baseurl }}{{ post.url }}"> {{post.date | date: '%Y.%m.%d'}}   {{post.title}}</a> </h4>
        </article>
      {% endfor %}
    {% endif %}
  </div>
{% endfor %}
</div>
