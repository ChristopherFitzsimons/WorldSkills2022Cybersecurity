Lists of kali tools and useful commands for them

## Reconnaisance :artificial_satellite:

Nmap: Nmap is a multi use tool, as it can help with both reconnaisance, enumeration and also offesnive(Not Hugely)
<details opne>
  <summary>Target Specification</summary>
  <br>
-iL = List IPS

--exclude <host1> = This will tell Nmap to exclude the said IPs

--exclude <exclude_file> = similar to --exclude, except that you can load list of IPs from file to exclude. The IPs must start in newline,space, or tab-delimited.

-n = This will disable DNS Resolution for the scan, which can lead to scanning time being reduced.

-R = This will tell Nmap to do reverse DNS resolution to the target IPs. This is normaly done against Online hosts.

--resolve-all = This will scan each resolved address that nmap finds, default behaviour is that Nmap will only scan the first resolved address.

--unique = Scan each IP address only once, default behaviour is that Nmap will scan the target as many times as it is specified in the target list.

--system-dns = Use system DNS resolver

--dns-servers = Servers to use for reverse DNS queries
  </details>
<details open>
  <summary>Host Discovery</summary>
<br>
-sL = List Scan. This will simpaly list each host of the network(s) specified, without sending any packets to the target hosts. 

-sn = No port scan.

-Pn = No ping

-PS = TCP SYN Ping

-PA = TCP ACK Ping

-PU = UDP Ping

-PY = SCTP INIT Ping

-PE; -PP; -PM = ICMP Ping Types

-PO = IP Protocol Ping

--disable-arp-ping = NO ARP or ND Ping

--discovery-ignore-rst = Since some firewalls can spoof TCP reset replies in response to unoccupied or disallowed addressess, this will make it so that Nmap doesnt consider them when scanning.

--traceroute = Trace path to host

  </details>
<details open>
  <summary>Misc</summary> 
<br>
--packet-trace = shows where the packets are going

--stats-every=5s/1m = This will show the progress of the scan every X seconds or minutes

  </details>

 <details open>
   <summary>Usage Example</summary>
<br>
```
nmap 172.12.14.1 -p 80,443,22,21 -sS -Pn -n --disable-arp-ping
```
  </details>
