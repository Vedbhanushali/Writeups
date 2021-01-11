first we registered in Nessus website
and a activation code is sent to our website

now we will depakage the downloaded file
$sudo dpkg -i Nessus-8.12.1-debian6_amd64.deb 

now we will start nessus service first and then install it
//to start
$/bin/systemctl start nessusd.service 

//to check the status of service
$/bin/systemctl status nessusd.service

then in browser we moved to 
$https://kali:8834/

where nessus website is hosted so we registered our selves
and some downloads and configuration took place


#rest is easy to do

 
