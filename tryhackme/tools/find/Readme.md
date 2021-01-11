
#Lastly, time-related searches will be covered. These are more complex but may prove useful. The flag consists of a word and a prefix. 
#The words are min and time, for minutes and days, respectively. The prefixes are a, m, and c, and are used to specify when a file was 
#last accessed, modified, or had its status changed. As for the numerical values, the same rules of the -size flag apply, except there
# is no suffix. To put it all together: in order to specify that a file was last accessed more than 30 minutes ago, the option -amin +30 
#is used. To specify that it was modified less than 7 days ago, the option -mtime -7 is used. (Note: when you want to specify that a file 
#was modified within the last 24 hours, the option -mtime 0 is used.)

<find all files owned by the user "kittycat"/>

find / -type f -user kittycat 

<Find all files that are exactly 150 bytes in size />
find / -type f -size 150c



<find all files in the /home directory (recursive) with size less than 2 KiB’s and extension ".txt">
find /home -type f -size -2k -name "*.txt"

<find all files that are exactly readable and writeable by the owner, and readable by everyone else (use octal format)>
find / -type f -perm 644

<Find all files with write permission for the group "others", regardless of any other permissions, with extension ".sh" (use symbolic format)>
find / -type f -perm -o=w -name "*.sh"

<Find all files in the /usr/bin directory (recursive) that are owned by root and have at least the SUID permission (use symbolic format)>
find /usr/bin -type f -user root -perm -u=s

<Find all files that were not accessed in the last 10 days with extension ".png">
find / -type f -atime +10 -name "*png"

<Find all files in the /usr/bin directory (recursive) that have been modified within the last 2 hours>
find /usr/bin -type f -mmin -120 


#we can suppress the output of any possible errors to make the output more readable. This is 
#done by appending 
2> /dev/null 
#to our command. This way, we won’t see any results we’re not allowed to access.




