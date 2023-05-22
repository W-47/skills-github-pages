---
layout: post
title: Alice in wonderland 
---
Hello and welcome to the tryhack me writeup [Alice on wonderland writeup](https://tryhackme.com/room/wonderland)

# NMAP SCAN

![nmap.png](/assets/nmap.png)

After a quick nmap scan we can see that the port 22 and port 80 are open.

# WEB ENUMARATION

From the previous nmap scan we can see that a web server is hosted on port 80 and we can use the IP to see it 

This is what we see 

![](/assets/web1.png)

Here we can use gobuster in an effort to find any directories that may be hidden.

![](/assets/go1.png)

From the results of the first gobuster we see a hidden directory **/r**. When we go to the directory this is what we see

![](/assets/web2.png)

Frome the title we see that we are told to keep going. So we use gobuster on the directory again discovering more directories which make up the word **rabbit** (Follow the white rabbit)
![](/assets/web7.png)

We can view the page source on this.

![](/assets/creds.png)

Aah yes we get a username and maybe a password and we shall try to ssh to this user using the creds.

