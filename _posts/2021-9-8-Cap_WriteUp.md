---
layout: post
title: HackTheBox - Cap 
---
#linux #htb  
[https://app.hackthebox.eu/machines/Cap](https://app.hackthebox.eu/machines/Cap)

Running nmap on the vulnerable machine
![nmap]({{ site.baseurl }}/images/Cap/Nmap.png)
3 ports open - 21, 22, 80

--- 

Trying to login to the Ftp server, No Anonymous login :(
![failftp]({{ site.baseurl }}/images/Cap/ftp_fail.png)

---
Looking at the website, it seems to be some kind of security dashboard
![web]({{ site.baseurl }}/images/Cap/website.png)

The data directory had some packet captures, along with some stats regarding the same
![data]({{ site.baseurl }}/images/Cap/3.png)
Here, we can download the pcap files. But /data/9 didnt seem to contain useful information
Tinkering with the number, and downloading the /data/0 file
![wire]({{ site.baseurl }}/images/Cap/wireshark.png)

Viewing it in wireshark, looking for logins or any other useful info
With the filter "ftp", we get usename and password
![ftp]({{ site.baseurl }}/images/Cap/login_info.png)

---
Using this login information to get access to the ftp server, 
![login]({{ site.baseurl }}/images/Cap/ftp.png)

Using the "get" command, pull both the linpeas.sh (used later for priv esc) as well as the user.txt to the attacker machine.  
User flag is   
![user]({{ site.baseurl }}/images/Cap/user.png)

---
Using scp (ssh copy), we can send the linpeas.sh file to the machine.  
Logging in using the same credentials through ssh.

Running linpeas.sh (it is an enumeration script)
Found  a weakness : 
![py]({{ site.baseurl }}/images/Cap/python3.png)

Which means, we need to leverage python3 to get root.  
One of the ways, is to execute /bin/sh with the setUID as 0 (root)

Python script :    
![pe]({{ site.baseurl }}/images/Cap/privesc.png)

Running the script, we end up getting root, and the flag :D
![root]({{ site.baseurl }}/images/Cap/root.png)  

[Here is a cat spitting facts about the universe](https://youtu.be/pRtxaJdZPAo)

