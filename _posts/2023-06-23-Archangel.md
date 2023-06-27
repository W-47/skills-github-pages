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
And we get a hit for **robots.txt**. which when we visit the page looks something like this.
![](https://i.ibb.co/vv2SyZj/robots.png)

And we can actually see another directory **/test.php**. visit this page and we can view another page with a button.
![](https://i.ibb.co/VYM0RyP/control.png)
We can then see that the button redirects us to **?view=/var/www/html/development_testing/mrrobot.php**.

From here we can try to do a path traversal filter. After hours of trial and error I found this filter and it worked.
**php://filter/convert.base64-encode/resource=**

