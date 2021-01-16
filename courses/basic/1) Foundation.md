whoami      //detail of user
hostname    //name of host on that system like unbuntu, windows, kali, debian
cat .bash_history       //to display history of commands

----------------Terminal shortcuts -----------------
ctrl + c - kill command
ctrl + z - stop (suspend command)
fg - foreground job number      // to bring program foreground type fg [number]
clear - clears screen
exit - exit terminal
history - see commands run in past
ctrl + r - search command history
ctrl + a - move cursor to beginning
ctrl + e - move cursor to end
ctrl + k - kill or cut from where cursor is
ctrl + y - paste
ctrl + l - clear

------------------Basic commands------------------------
passwd - change new password
pwd - print working directory
cd - change directory
mkdir - make / create directory
touch - create file
ls - list directory content     //mostly used tag (ls -al)
cat - display files
less - show files
head - show first 10 lines
tail - show last 10 lines
cp - copy files
mv - move or rename files
rm and rmdir - remove file and or directory

-----------------Installing software------------------
apt search package_name (apt-cache)    //search for package
apt show package_name (apt-cache)      //information about package
apt install -y package_name                //install package without asking of yes no dialouge
apt remove                              //remove package but leaves configs
apt purge                               //remove package and configs to

apt update      //resynchronize update
apt upgrade     //upgrade all installed packages
apt dist-upgrade    //same as upgrade and also upgrade dependiences

dpkg -l     //find version of installed application
dpkg -i package.deb     //install package
dpkg -r package.deb     //remove package

---------------------archiving tool tar------------------
tar c - Create archive
tar r - append to archive
tar t - list contents of archive
tar x - Extract
tar v - verbose
tar f file - file to use

---------------- Compression and decompression-----------
#create tar from files
tar cvf archive.tar file1 file2 dir1 dir2

#list contents of archive
tar tvf archive.tar

#adding to file or dir to archive
tar rvf archive.tar file3
tar rcf archive.tar dir3

#Extract file or directory
tar xvf archive.tar file3
tar xvf archive.tar dir3/subdir3

#Extract archive
tar xvf archive.tar

#Extract in dirrent directory
tar xvf archive.tar -C /tmp/extracted

#Compress
tar zcvf commpressd.tar.gz file1 file2
tar jcvf commpressed.tar.bz2 file2 file2

list
tar ztvf compressed.tar.gz
tar jtvf compressed.tar.bz2

Extract file or directory
tar zxvf compressed.tar.gz file1
tar jxvf compressed.tar.bz2 file1

----------------Wildcards-------------------------------------
* - match zero or all characters
? - Matches exactly one character
[] - Matches any of the characters enclosed in the brackets
\ - Escape character

tree .      //gives a good graphical representation

file file_name - Determine format of file
apropos keyword - commands whose one line description contains the keyword      //this will normally give us the description of the command

which command_name/program_name      //gives the location of the binary file
locate command_name     //to locate all files with command_name associated
man program_name        //to have manually of the command
Press
G - to go the beginning of the manual
shift + G - to go the end of the manual
/ - to search for a word (Press N to go to next word)(press shift+N to go to previous word)
q - to quit


