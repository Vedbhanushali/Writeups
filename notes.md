#to search in system of the file without getting any error message
find / -name .overpass 2>/dev/null		#here replace the file name with .overpass 
-----------------------------------Introductory to network-------------------------------------
ping IP
//or we can enter domain name also
ping www.google.com

traceroute IP
//or we can enter domain name also
traceroute www.google.com

#to get information like owner and dns stuff of the domain or IP 
whois bbc.co.uk

#to search whether application is already installed in the system type
type -a program_name 
#like
type -a nmap

#to get information of TTL (Time to live of the domain) than use dig
dig facebook.com


#For privliege escalation to find all SUID on system
find / -user root -perm -4000 -exec ls -ldb {} \;


#to listen on netcat
nc -lvnp 9001		//9001 is listening port

#to turn on privilege mode on bash if possible
bash -p


