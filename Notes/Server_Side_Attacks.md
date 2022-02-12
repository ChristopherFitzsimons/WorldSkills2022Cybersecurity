## **Server-Side Attack Notes**

* * *

*Following are the notes for the Server Side Attack.*

# Nginx Reverse Proxy & AJP
If coming across an open AJP proxy port (8009 TCP), you can use the Nginx with ajp_module to access the **"hidden"**  Tomcat manager. The way to do this is by compiling the Nginx source code and then adding the ajp_module.


1.Download the Nginx source code

2.Download the required module (In this case <font color="lightgreen">ajp_module</font>)

3.Compile Nginx source code with the <font color="lightgreen">ajp_module</font>

4.Create a configuration file pointing to the AJP Port

**Downloading Nginx Source Code**

```console
wget https://nginx.org/download/nginx-1.21.3.tar.gz
tar -xzvf nginx-1.21.3.tar.gz
```

**Compile Nginx source code with the ajp module**

```console
git clone https://github.com/dvershinin/nginx_ajp_module.git
```

<font color=lightgreen>Clone the nginx_ajp_module to the same place as where the nginx folder is located at.</font>

```console
cd nginx-1.21.3
```

<font color=lightgreen>Travel to the nginx folder and run each commands inside of it.</font>

```console
sudo apt install libpcre3-dev
```

<font color=lightgreen>Neded for building.</font>

```console
./configure --add-module=`pwd`/../nginx_ajp_module --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules
```

<font color=lightgreen>Adding the nginx_ajp_module.</font>

```console
make
sudo make install
```

<font color=lightgreen>Compiling and installing.</font>

```console
nginx -V
````

<font color=lightgreen>If everything was compiled correctly it should give you an output</font>

**Pointing to the AJP Port**

We first need to comment out the entire <font color="lightgreen">server</font> block and then append the following lines inside the <font color="lightgreen">http</font> block in <font color="lightgreen">/etc/nginx/conf/nginx.conf</font>

```php
	upstream tomcats {
		server <TARGET_SERVER>:8009;
		keepalive 10;
		}
	server {
		listen 80;
		location / {
			ajp_keep_conn on;
			ajp_pass tomcats;
		}
	}
```

To check if Nginx is working first start it then try to go to the website on localhost/127.0.0.1

```console
sudo nginx
```
