---
layout: post
title:  "The Attacker"
date:   2020-01-07 22:53:01 +0800
categories: NetworkSecurity
---

## **Principle of Humanity**:  
Computer Network Exploitation(CNE) is grounded in human nature.

## * **Life Cycle of an Operation**:
	1. Starting
	2. Targeting
	3. Initial Access
	4. Persistence, Access Expansion, Exfiltration.
	5. Detection

Stage 1: Targeting contains identification of the target network, the attack strategies and tactics necessary to exploit that network.

Stage 2: Initial Access contains of penetrating any defensive security and gaining the ability to run commands or other software on one of the target's computers or network devices.

Stage 3 : Persistence turns initial access into reoccurring access.

Stage 4: Expansion increasing access to a target network.

Stage 5: Exfiltration is the retrieval of wanted data from the target network.

Stage 6: Detection occurs when an operation is exposed to the target.

-------------------------------------------------------  

## **Principle of Access**:  
There is always someone with legitimate access and a means to use it.  

* Four basics types of connectivity:  
	- inbound access  
	- outbound access  
	- both  
	- neither


### Inbound Access:  
Someone from outside the network can connect into the network. e.g. public web, or restricted.  
Access control: "know", "have" and "virtual location".

Attacking method:  
1. Impersonating legitimate access  
"know": Guess or steal password  
"have": Steal hardware tokens or cell phones. Use malware.  
"virtual location": Compromise the network first.  
2. Gaining illegitimate access  
Exploit an exposed software service. e.g. Morris Worm  
Escalating privileges.  

### Outbound Access:  
Someone from inside the network can connect to outside but the network is not accessible from the outside.  
So attackers need an inside user to connect to the outside. e.g. e-mail.  

### Email Attacks  
Three basic approach: attachments, attacking the e-mail program, and malicious links.

### Attachments  
First Generation Attachment: Executable program, install something in the background to grant access to the attacker. Simple, direct and effective.  
Second Generation Attachment: Documents that contained the ability to run code. Send a copy to other e-mail addresses in the address book. Because of the "macro" feature of Microsoft Office.  
Third Generation Attachment: Programs that do not seem like programs. Esoteric extensions e.g. .chm or .hta. E-mail filter updated to catch them.  
Current Generation Attachment: Third-party application-specific documents, e.g. .doc, .xls, .pdf, .zip... use vulnerabilities of the application to execute the code.  

### Attacking the e-mail program  
Ideal Attack. Send specially crafted e-mail, corrupt the e-mail program, execute attacker-supplied code. Only need the user to preview or view the e-mail. Does not need the user to open the attachment. 