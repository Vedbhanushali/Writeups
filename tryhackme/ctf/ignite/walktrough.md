kali>nmap -sC -sV $IP
//after doing this we find out that it is running CMs S with version 1.4
so find its exploit on db
--CVE-2018-16762

and after downloading it we need to modify just ip address of the target
after that

turn on burp in foxyproxy
start burp suite
and run the script

after running the script forward the request through burpsuite and we will get the result

