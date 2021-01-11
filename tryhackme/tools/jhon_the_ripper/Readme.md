//to dercypt shadow password
kali>john /etc/shadow

//to specify wordlist to above code
kali>john /etc/shadow --wordlist=/usr/share/wordlists/rockyou.txt

//t0 decrypt md5 hash

kali>john --format=raw-MD5 /home/kali/Documents/tryhackme/tools/jhon_the_ripper/hash_file.txt

