
#this is normal dirbuster
gobuster dir -u http://10.10.156.24/secret/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 5 -q


#this is for searching file with extension is dir
gobuster dir -u http://10.10.156.24/secret/ -x .txt,.php,.html,.md -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 5 -q

