#we can also run this via the recon tool at 
https://dnsdumpster.com/ 

#Sublist3r is a fantastic python script that allows us to perform quick and easy recon against our target, discovering various subdomains associated with the websites/domains in scope.  
#Inshort it is used for gathering information of subdomain and dns of the gived domain
#install-
git clone https://github.com/aboul3la/Sublist3r.git

#satisfy requirement
pip3 install -r requirements.txt

#running it on nbc.com which as an american news company

python3 sublist3r.py -d nbc.com -o sub-output-nbc.txt -t 30 
