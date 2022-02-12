<[Back to Main Page](https://github.com/ChristopherFitzsimons/WorldSkills2022Cybersecurity)

# NMap Enumeration Fundamentals
Created by Christopher Fitzsimons

## Usefull URL's
https://www.tutorialspoint.com/nmap-cheat-sheet

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
Here we can specify the number of seconds (s) or minutes (m),
We can also increase the verbosity level (-v / -vv), which will show us the open ports directly when Nmap detects them

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
