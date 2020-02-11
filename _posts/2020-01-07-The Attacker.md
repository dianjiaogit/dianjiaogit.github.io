---
layout: post
title:  "The Attacker"
date:   2020-01-07 22:53:01 +0800
categories: NetworkSecurity
---

# Principle of Humanity:  
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

# Principle of Access:  
There is always someone with legitimate access and a means to use it.  

* Four basics types of connectivity:  
	- inbound access  
	- outbound access  
	- both  
	- neither


## Inbound Access:  
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

## Outbound Access:  
Someone from inside the network can connect to outside but the network is not accessible from the outside.  
So attackers need an inside user to connect to the outside. e.g. e-mail.  

## Email Attacks  
Three basic approach: attachments, attacking the e-mail program, and malicious links.

### Attachments  
First Generation Attachment: Executable program, install something in the background to grant access to the attacker. Simple, direct and effective.  
Second Generation Attachment: Documents that contained the ability to run code. Send a copy to other e-mail addresses in the address book. Because of the "macro" feature of Microsoft Office.  
Third Generation Attachment: Programs that do not seem like programs. Esoteric extensions e.g. .chm or .hta. E-mail filter updated to catch them.  
Current Generation Attachment: Third-party application-specific documents, e.g. .doc, .xls, .pdf, .zip... use vulnerabilities of the application to execute the code.  

### Attacking the e-mail program  
Ideal Attack. Send specially crafted e-mail, corrupt the e-mail program, execute attacker-supplied code. Only need the user to preview or view the e-mail. Does not need the user to open the attachment.  

### Malicious links  
Links bring the user to specifically designed websites to leverage vulnerabilities in web browsers or plug-ins.  

## Website Hijack Attacks  
Commandeer legitimate sites that the attacker knows or hopes a target user will visit. aka "Watering hole" attacks. The Attacker waits for the target to come to them. Same result as e-mail link-based attack except it does not need e-mail to direct the user.

1. The Attacker takes over and replaces the content of a popular website with malicious code.  
2. Use a cross-site-scripting attack (XSS). The Attacker uploads code that the website then displays to other users. These sites allow users to post information or comments.  
3. Avoid commandeering the website itself, hijack the domain name. Hack the registrars, so the IP address from the DNS server will be different.  
4. Use ad network instead of commandeering a website. As advertisements can be placed by anyone, including the attacker. Put malicious code into the ads.  
5. Pornography, illegal software, or pirated movies that are tainted and posted for download. Hope users of the target network will open them.  

## Other Attacks  
Malicious thumb drives  
Attack other network that the laptops connect to  
Wireless access  
Attack from smartphones  
Social engineering  
Physical access attack  
...  

## Circumventing Outbound Restrictions  
How to restrict access:  
1. Software, such as parental control software, Kaspersky and McAffee  
Attacker need user to "allow" or attack the software directly.
2. Software or hardware on the network, such as firewall or proxy sercer.  
Must gain access to the network device itself (but difficult). Or Establish a command and control channel into the network (but need circumventing the device first). Or Establish outbound connections via allowed network protocols (Easier).  

Attacker's challenge: lecerage initial execution to circumvent host-based and network-based restrictions and establish command and control.  

## Bidirectional Access  
Some types of connections are allowed in and others allowed out. Each direction may have its own set of access controls, restictions and monitoring.  

The attacker follows the path of least resistance, mixes and matches approaches for the greatest effect.  

## No Outside Access  
A network with no outside connectivity is commonly called an isolated network or an "air-gapped" network. This network is physically separated from the Internet.  

The most secure network configuration possible to protect against outsider threats and the most inconvenient for sharing information or administering.  

Attacker must breach physical security or trick, cajole, bribe, or blakmail users into doing it for them.  

Air gap did not prevent the Stuxnet worm.  

Attacker must find a way to be, to corrupt, or to compromise an insider in order to reduce the problem of physical access to one of insider threat security.  

# Principle of Economy  
Ambitions will always exceed available resources. All ambitions are tempered by the constraints of reality.  

* Time  
  Targeting Capabilities (gathering and analyse)  
  Exploitation Expertise
  Networking Expertise  
  Software Development Expertise  
  Operational Expertise  
  Operational Analysis Expertise  
  Technical Resources  

