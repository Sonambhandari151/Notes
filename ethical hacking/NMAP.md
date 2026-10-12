https://www.nirsoft.net/countryip/ : major ip blocks of countries.
- whois IP_ADD: search on google to check.
- **nslookup:** To search IP_add of a domain name or vice versa.
---
```
nmap scanme.nmap.org
```
- without flags it scans  1000 most common ports.
- `-oG` option in **Nmap**: **"Output in Grepable format"**.
- -A: OS detection
- -sV : service versions.
- -F : fast scan , top 100 ports.
-  --open: to scan for only open ports.
- -T4: 

```
nmap -F IP1 IP2
```
- To scan multiple IPs.
    
- **zenmap:** GUI version og nmap

### WebMap: web version for nmap

---
## Python3 Nmap Scanner

```
#!/usr/bin/python3

import nmap

scanner = nmap.PortScanner()

print("Welcome, this is a simple nmap automation tool")
print("<----------------------------------------------------->")

ip_addr = input("Please enter the IP address you want to scan: ")
print("The IP you entered is: ", ip_addr)
type(ip_addr)

resp = input("""\nPlease enter the type of scan you want to run
                1)SYN ACK Scan
                2)UDP Scan
                3)Comprehensive Scan \n""")
print("You have selected option: ", resp)
resp_dict={'1':['-v -sS','tcp'],'2':['-v -sU','udp'],'3':['-v -sS -sV -sC -A -O','tcp']}
if resp not in resp_dict.keys():
    print("enter a valid option")
else:
    print("nmap version: "sccaner.nmap_version())
    scanner.scan(ip_addr,"1-1024",resp_dict[resp][0]) #the # are port range to scan, the last part is the scan type
    print(scanner.scaninfo())
    if scanner.scaninfo()=='up':
        print("Scanner Status: ",scanner[ip_addr].state())
        print(scanner[ip_addr].all_protocols())
        print("Open Ports: ",scanner[ip_addr][resp_dict[resp][1]].keys())  #display all open ports







```

---
## TCP FLAGS
1. URG : packet to be processed immediately.
2. PSH : Transmits data immediately.
3. FIN : no furtgher transmission.
4. ACK : acknowledges receipt of packet.
5. SYN: INitialises connection between host and target
6. RST : reset the connection.
---
## Ping SWEEP
A **ping sweep** is used to discover **live hosts** on a network by sending ICMP echo requests or similar probes.

```
nmap -sn 192.168.1.0/24

```
- `-sn`: "Ping scan" — disables port scanning, only checks host availability.
- -Pn : Treat all hosts online. Skip host discovery.
- -O : OS Detection.
- -sV : Service Versionj.
- -A : Aggressive scan
    What it includes:
    - `-sS` or `-sT`: Port scanning
        
    - `-O`: OS detection
        
    - `-sV`: Service/version detection
        
    - `--traceroute`: Traceroute
        
    - `-T4`: Faster timing template

- ### `-oX` = **XML output**
- `-oN` = **Normal (text) output**
- -oA: for all three formats( each for Grep, XML, normal)

- -sT : TCP Connection
- sS: Stealth(hald scan)
  Here’s a clear comparison of **`-sT` vs `-sS`** in Nmap:

| Feature           | `-sT` (TCP Connect Scan)                     | `-sS` (SYN Scan / Stealth Scan)     |
| ----------------- | -------------------------------------------- | ----------------------------------- |
| **Privileges**    | No root required                             | Requires root (to send raw packets) |
| **TCP Handshake** | Full (SYN → SYN-ACK → ACK)                   | Partial (SYN → SYN-ACK → RST)       |
| **Stealthy?**     | ❌ Easily detected                            | ✅ Less detectable                   |
| **Speed**         | Slower                                       | Faster                              |
| **Detection**     | Logged by target system                      | Often bypasses logging              |
| **Usage default** | When **not root**                            | When **running as root**            |
| **Reliability**   | More reliable if firewalls block raw packets | May be blocked by firewalls         |


---

### 🔍 How Nmap uses ICMP in UDP scanning:(-sU)


When Nmap sends a UDP packet to a port:

- If it receives an **ICMP Port Unreachable (Type 3, Code 3)** →  
    🔴 The port is **closed**.
    
- If **no response at all** →  
    ⚠️ The port is marked **open|filtered** (could be open or blocked by firewall).
    
- If it gets a **valid UDP response** →  
    🟢 The port is **open**.
    
- If it gets **other ICMP errors (e.g., Type 3 Code 1, 2, 9, 10, 13)** →  
    🚫 The port is **filtered**.
---
-v: info about the scan progress
-vv: packets and network
-vvv:  about the scan



---
### NULL, FIN and Xmas
- NULL scans (`-sN`) are when the TCP request is sent with no flags set at all. As per the RFC, the target host should respond with a RST if the port is closed.
![[Pasted image 20250116123757.png]]


- FIN scans (`-sF`) work in an almost identical fashion; however, instead of sending a completely empty packet, a request is sent with the FIN flag (usually used to gracefully close an active connection). Once again, Nmap expects a RST if the port is closed.
![[Pasted image 20250116123917.png]]

- Xmas scans (`-sX`) send a malformed TCP packet and expects a RST response for closed ports. It's referred to as an xmas scan as the flags that it sets (PSH, URG and FIN) give it the appearance of a blinking christmas tree when viewed as a packet capture in Wireshark.
![[Pasted image 20250116124104.png]]

--reason : gives reason for the state of each port.
- **bypass firewalls and intrusion detection systems (IDS)**.