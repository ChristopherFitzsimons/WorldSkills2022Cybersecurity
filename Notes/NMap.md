---
layout: default
---

<[Back to Main Page](../index.html)

# Network Enumaration with NMap
Created by Christopher Fitzsimons

## TLDR
Nmap is a largly known tool used by Defensive and Offensive professionals. This tool allows the user to scan networks and fine servers with open services and ports. This is vital information as from an Offensive perspective they can use this information to target their attack against services and servers which look vulnerable. From a Defensive perspective this tool can be used to evaluate your network protection and arise issues or vulnerbilities on servers.

## Usefull Commands
### Search for up hosts
```bash
nmap -sn 10.129.150.229
```
### Search for port and service Version
```bash
nmap -sV -p 80 10.129.150.229
```
### Fast Search for common ports
```bash
nmap -F 10.129.150.229
```
## Scan All Ports
```bash
nmap -sV -p- 10.129.150.229
```
### Install Vulnscan for nmap
```bash
git clone https://github.com/scipag/vulscan scipag_vulscan
ln -s `pwd`/scipag_vulscan /usr/share/nmap/scripts/vulscan
nmap -sV --script=vulscan/vulscan.nse <target>
// or
// Basic Vuln catigory
nmap --sV --script vuln <target>
```
### nmap-vulners
```bash
cd /usr/share/nmap/scripts/
git clone https://github.com/vulnersCom/nmap-vulners.git
nmap --script nmap-vulners/ -sV <target>
```
## Uses common scripts with Scan
```bash
nmap -sC 10.129.155.61
```
## Port Scan top 10
```bash
nmap 10.129.2.28 --top-ports=10 
```
## Port scan packet trace
```bash
nmap 10.129.2.28 -p 139 --packet-trace -n --disable-arp-ping -Pn
```
## Open UDP Port
```bash
nmap 10.129.2.28 -F -sU
```
## TCP SYN Port
```bash
nmap 10.129.2.28 -F -sS
```
## Connect TCP Port
```bash
nmap 10.129.2.28 -F -sT
```
## Aggressive Scan
```bash
sudo nmap 10.129.2.28 -p 80 -A
```

## Complex commands
```bash
Scanning for devices:
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
Scan from list:
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
Scan single IP:
sudo nmap 10.129.2.18 -sn -oA host 
Scan IP Range:
sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5
Scan if device is alive:
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace 
Bypass IPS Protection (-sS [SYN] -Pn [Disable ICMP] -n [Disable DNS] -D [Decoy] =vv [Super Verbose])
sudo nmap -p- 10.129.2.47 -sS -Pn -n -D RND:5 -vv --max-retrie 0 --source-port 53 --disable-arp-ping --stats-every=30s
```

## Saving Results
Normal output (-oN) with the .nmap file extension  
Grepable output (-oG) with the .gnmap file extension  
XML output (-oX) with the .xml file extension  
We can also specify the option (-oA) to save the results in all formats. The command could look like this  
Open hmtl export  
xsltproc target.xml -o target.html  

## Get Stats updates
--stats-every=5s  
Here we can specify the number of seconds (s) or minutes (m).  
We can also increase the verbosity level (-v / -vv), which will show us the open ports directly when Nmap detects them  

## Timings

|Short|Long|
|---|---|
|-T 0 |-T paranoid|
|-T 1 |-T sneaky|
|-T 2 |-T polite|
|-T 3 |-T normal|
|-T 4 |-T aggressive|
|-T 5 |-T insane|

## Other network commands
TCP Dump:  
```bash
sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28  
```
Netcat:  
```bash
nc -nv 10.129.2.28 25
```

## SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans  
  -sU: UDP Scan  
  -sN/sF/sX: TCP Null, FIN, and Xmas scans  
  --scanflags <flags>: Customize TCP scan flags  
  -sI <zombie host[:probeport]>: Idle scan  
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans  
  -sO: IP protocol scan  
  -b <FTP relay host>: FTP bounce scan  

## Notes
If our target sends an SYN-ACK flagged packet back to the scanned port, Nmap detects that the port is open.  
If the packet receives an RST flag, it is an indicator that the port is closed.  
If Nmap does not receive a packet back, it will display it as filtered. Depending on the firewall configuration, certain packets may be dropped or ignored by the firewall.  

We can use various options to tell Nmap how fast (-T <1-5>), with which frequency (--min-parallelism <number>), which timeouts (--max-rtt-timeout <time>) the test packets should have, how many packets should be sent simultaneously (--min-rate <number>), and with the number of retries (--max-retries <number>) for the scanned ports the targets should be scanned.  

Another way to increase the scans' speed is to specify the retry rate of the sent packets (--max-retries). The default value for the retry rate is 1, so if Nmap does not receive a response for a port, it will not send any more packets to the port and will be skipped.  

When setting the minimum rate (--min-rate <number>) for sending packets, we tell Nmap to simultaneously send the specified number of packets. It will attempt to maintain the rate accordingly.  

Nmap's TCP ACK scan (-sA) method is much harder to filter for firewalls and IDS/IPS systems than regular SYN (-sS) or Connect scans (sT) because they only send a TCP packet with only the ACK flag. So if you want to get around firewall filtering use -sA.  

the Decoy scanning method (-D) is the right choice. With this method, Nmap generates various random IP addresses inserted into the IP header to disguise the origin of the packet sent. Example is (-D RND:5)  
Another scenario would be that only individual subnets would not have access to the server's specific services. So we can also manually specify the source IP address (-S)  
The argment (-O) performs an Operating system scan.  
To send requests through a specific tunnel use (-e tun0)  
we can use TCP port 53 as a source port (--source-port) for our scans.  
