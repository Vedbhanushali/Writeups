
//Given the username "admin", the password "password", and the ip "10.10.10.10", how would you run 
ipconfig on that machine
kali>smbmap -u admin -p password -H 10.10.10.10 -x "ipconfig"


//smbclient can also be used as smbmap


