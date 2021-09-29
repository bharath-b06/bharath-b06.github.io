---
layout: post
title: HackTheBox - Knife 
---
#linux #htb  
[https://app.hackthebox.eu/machines/Knife](https://app.hackthebox.eu/machines/Knife)

Running nmap on the vulnerable machine
![nmap]({{ site.baseurl }}/images/Knife/nmap.png)
2 ports open - 22, 80


---
Looking at the website, it seems to be some kind of a hospital website
![web]({{ site.baseurl }}/images/Knife/website.png)


Running gobuster on the website, but no useful directories :(
![gob]({{ site.baseurl }}/images/Knife/gobust_fail.png)

Running wappalyzer to get more information about the website

![wapp]({{ site.baseurl }}/images/Knife/wappa.png)

We get the php version running on the website
Looking for exploits for php 8.1.0

Exploit db has a script for the exact php version, to gain rce
[https://www.exploit-db.com/exploits/49933](https://www.exploit-db.com/exploits/49933)
![edb]({{ site.baseurl }}/images/Knife/exploitdb.png)

---

Running the python exploit from exploitdb, 

![revs]({{ site.baseurl }}/images/Knife/revshell.png)

We get an unstable shell, therefore to make it stable  
Tried running a few reverse shell commands
nc, python, bash from this link
[https://gist.github.com/sckalath/67a59eb4955f1f9aedde](https://gist.github.com/sckalath/67a59eb4955f1f9aedde)

Running 
![rev]({{ site.baseurl }}/images/Knife/upgradeshell.png)
ended up working, with a listener running on the attacker machine

Grabbing the user flag :D
![user]({{ site.baseurl }}/images/Knife/user.png)

---

### Privilege escalation
Running `sudo -l` gives an idea of what the user is capable of, and if its able to get increase privs

![su]({{ site.baseurl }}/images/Knife/sudol.png)

There exists a "knife" executable, it is written in ruby  
Looking for options to run knife using the --help flag:
![ke]({{ site.baseurl }}/images/Knife/knifeexec.png)

To get root, in ruby we can run bash commands inside the system() function
![root]({{ site.baseurl }}/images/Knife/root.png)

Grab the root flag x

[Here is a chonk cat](https://youtu.be/dJy7ByTpbyk)  


