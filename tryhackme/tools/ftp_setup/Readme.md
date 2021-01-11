#if vsftpd not install then install it

sudo apt install vsftpd

#after install we will run ftp server

service vsftpd start

# to check the status of the service

service vsftpd status

#configure ftp credentials

nano /etc/vsftpd.conf

#and enable anonymous_enable=YES

#to stop the ftp service

service vsftpd stop
service vsftpd status


