---
layout: post
title: Alice in wonderland
categories: Privesc
---
Hello guys and welcome to yet another writeup. This is ye another easy box on tryhackme and is accessible [here](https://tryhackme.com/room/archangel)
We shall tackle some awesome topics which include:
  1. LFI.
  2. Priviledge exploitation.
  3. Web exploitation.
With that said let us get right to it

# NMAP scan.
As usual we are gonna start of by scanning open ports on our machine.
![](https://i.ibb.co/ZVjYSZ9/nmap.png)

We are able to see that port 80 is open and is hosting a web application.
we are able to view a home page which does not have alot on it but we can see a type of hostname

![](https://i.ibb.co/YLJjDwK/wavefire.png)

We can actually use this and and it to our **/etc/hosts** file, then try and go to the page.
![](https://i.ibb.co/TWTBYN0/mafia.png)

# LFI.
Okay guys it is about to get really messy in here. Stay frosty.

So first of all we are going to FUZZ for hidden directories.
![](https://i.ibb.co/kQRKnYg/wfuzz.png)
And we get a hit for **robots.txt**
![](https://i.ibb.co/vv2SyZj/robots.png)
  
