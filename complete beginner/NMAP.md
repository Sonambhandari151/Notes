We begin any attack with a port scan.

nmap will connect to each port of the target in turn. Depending on how the port responds, it can be determined as being open, closed, or filtered (usually by a firewall).
Once we know which ports are open, we can then look at enumerating which services are running on each port – either manually, or more commonly using nmap.
1024 well known ports.
### port Scanning

When port scanning with Nmap, there are three basic scan types. These are:

- TCP Connect Scans (`-sT`)
- SYN "Half-open" Scans (`-sS`)
- UDP Scans (`-sU`)

Additionally there are several less common port scan types, some of which we will also cover (albeit in less detail). These are:

- TCP Null Scans (`-sN`)
- TCP FIN Scans (`-sF`)
- TCP Xmas Scans (`-sX`)
- 
RFC 9293 defines the appropriate behaviour for the TCP protocol
### TCP connect scan
f Nmap sends a TCP request with the _SYN_ flag set to a **_closed_** port, the target server will respond with a TCP packet with the _RST_ (Reset) flag set. By this response, Nmap can establish that the port is closed
![[Pasted image 20250115210130.png]]
If, however, the request is sent to an _open_ port, the target will respond with a TCP packet with the SYN/ACK flags set.
Many firewalls are configured to simply **drop** incoming packets. Nmap sends a TCP SYN request, and receives nothing back. This indicates that the port is being protected by a firewall and thus the port is considered to be _filtered_

The command: 
`iptables -I INPUT -p tcp --dport <port> -j REJECT --reject-with tcp-reset`
is used to **reject incoming TCP connections** on a specific port while sending a **TCP reset (RST) packet** to the client.

### SYN Scans
SYN scans (`-sS`) are used to scan the TCP port-range of a target or targets.SYN scans are sometimes referred to as "_Half-open"_ scans, or _"Stealth"_ scans.
Where TCP scans perform a full three-way handshake with the target, SYN scans sends back a RST TCP packet after receiving a SYN/ACK from the server (this prevents the server from repeatedly trying to make the request). In other words, the sequence for scanning an **open** port looks like this:![[Pasted image 20250115212253.png]]
They require sudo permissions[1] in order to work correctly in Linux. This is because SYN scans require the ability to create raw packets (as opposed to the full TCP handshake), which is a privilege only the root user has by default.
SYN scans are the default scans used by Nmap _if run with sudo permissions_. If run **without** sudo permissions, Nmap defaults to the TCP Connect scan.

If a port is closed then the server responds with a RST TCP packet. If the port is filtered by a firewall then the TCP SYN packet is either dropped, or spoofed with a TCP reset.

[1]SYN scans can also be made to work by giving Nmap the CAP_NET_RAW, CAP_NET_ADMIN and CAP_NET_BIND_SERVICE capabilities; however, this may not allow many of the NSE scripts to run properly.

### UDP Scan
When a packet is sent to an open UDP port, there should be no response. When this happens, Nmap refers to the port as being `open|filtered`. In other words, it suspects that the port is open, but it could be firewalled. If it gets a UDP response (which is very unusual), then the port is marked as _open_. More commonly there is no response, in which case the request is sent a second time as a double-check. If there is still no response then the port is marked _open|filtered_ and Nmap moves on.  

When a packet is sent to a _closed_ UDP port, the target should respond with an ICMP (ping) packet containing a message that the port is unreachable. This clearly identifies closed ports, which Nmap marks as such and moves on.


### NULL, FIN and Xmas
- NULL scans (`-sN`) are when the TCP request is sent with no flags set at all. As per the RFC, the target host should respond with a RST if the port is closed.
![[Pasted image 20250116123757.png]]


- FIN scans (`-sF`) work in an almost identical fashion; however, instead of sending a completely empty packet, a request is sent with the FIN flag (usually used to gracefully close an active connection). Once again, Nmap expects a RST if the port is closed.
![[Pasted image 20250116123917.png]]

- Xmas scans (`-sX`) send a malformed TCP packet and expects a RST response for closed ports. It's referred to as an xmas scan as the flags that it sets (PSH, URG and FIN) give it the appearance of a blinking christmas tree when viewed as a packet capture in Wireshark.
![[Pasted image 20250116124104.png]]

-   The expected response for _open_ ports with these scans is also identical, and is very similar to that of a UDP scan. If the port is open then there is no response to the malformed packet. Unfortunately (as with open UDP ports), that is _also_ an expected behaviour if the port is protected by a firewall, so NULL, FIN and Xmas scans will only ever identify ports as being _open|filtered_, _closed_, or _filtered_. If a port is identified as filtered with one of these scans then it is usually because the target has responded with an ICMP unreachable packet.
- Many firewalls are configured to drop incoming TCP packets to blocked ports which have the SYN flag set (thus blocking new connection initiation requests). By sending requests which do not contain the SYN flag, we effectively bypass this kind of firewall. Whilst this is good in theory, most modern IDS solutions are savvy to these scan types, so don't rely on them to be 100% effective when dealing with modern systems.
### ICMP Network Scanning

On first connection to a target network in a black box assignment, our first objective is to obtain a "map" of the network structure -- or, in other words, we want to see which IP addresses contain active hosts, and which do not.

One way to do this is by using Nmap to perform a so called "ping sweep". This is exactly as the name suggests: Nmap sends an ICMP packet to each possible IP address for the specified network. When it receives a response, it marks the IP address that responded as being alive.
To perform a ping sweep, we use the `-sn` switch in conjunction with IP ranges which can be specified with either a hypen (`-`) or CIDR notation. i.e. we could scan the `192.168.0.x` network using:

- `nmap -sn 192.168.0.1-254`

or

- `nmap -sn 192.168.0.0/24`
- The `-sn` switch tells Nmap not to scan any ports -- forcing it to rely primarily on ICMP echo packets (or ARP requests on a local network, if run with sudo or directly as the root user) to identify targets. In addition to the ICMP echo requests, the `-sn` switch will also cause nmap to send a TCP SYN packet to port 443 of the target, as well as a TCP ACK (or TCP SYN if not run as root) packet to port 80 of the target.
### NSE Scripts
- The **N**map **S**cripting **E**ngine (NSE) is an incredibly powerful addition to Nmap, extending its functionality quite considerably. NSE Scripts are written in the _Lua_ programming language, and can be used to do a variety of things: from scanning for vulnerabilities, to automating exploits for them. The NSE is particularly useful for reconnaisance, however, it is well worth bearing in mind how extensive the script library is.- 
- `safe`:- Won't affect the target
- `intrusive`:- Not safe: likely to affect the target  
    
- `vuln`:- Scan for vulnerabilities
- `exploit`:- Attempt to exploit a vulnerability
- `auth`:- Attempt to bypass authentication for running services (e.g. Log into an FTP server anonymously)
- `brute`:- Attempt to bruteforce credentials for running services
- `discovery`:- Attempt to query running services for further information about the network (e.g. query an SNMP server).


- we looked very briefly at the `--script` switch for activating NSE scripts from the `vuln` category using `--script=vuln`. It should come as no surprise that the other categories work in exactly the same way. If the command `--script=safe` is run, then any applicable safe scripts will be run against the target.
- Some scripts require arguments (for example, credentials, if they're exploiting an authenticated vulnerability). These can be given with the `--script-args` Nmap switch. An example of this would be with the `http-put` script (used to upload files using the PUT method). This takes two arguments: the URL to upload the file to, and the file's location on disk.  For example:

`nmap -p 80 --script http-put --script-args http-put.url='/dav/shell.php',http-put.file='./shell.php'`

Note that the arguments are separated by commas, and connected to the corresponding script with periods (i.e.  `<script-name>.<argument>`).

---

Nmap scripts come with built-in help menus, which can be accessed using `nmap --script-help <script-name>`.

### Searching for Scripts
We have two options to search for a script
1. https://nmap.org/nsedoc/
2. local storage on your attacking machine.
     Nmap stores its scripts on Linux at `/usr/share/nmap/scripts`. All of the NSE scripts are stored in this directory by default -- this is where Nmap looks for scripts when you specify them.
     - There are two ways to search for installed scripts. 
     1. One is by using the `/usr/share/nmap/scripts/script.db` file.
      ![[Pasted image 20250117184116.png]]
      - we can also _grep_ through it to look for scripts. For example: `grep "ftp" /usr/share/nmap/scripts/script.db`.
      - ![[Pasted image 20250117184221.png]]
      2. The second way to search for scripts is quite simply to use the `ls` command. For example, we could get the same results as in the previous screenshot by using `ls -l /usr/share/nmap/scripts/*ftp*`:
         ![[Pasted image 20250117184409.png]]
         - The same techniques can also be used to search for categories of script. For example:  
           `grep "safe" /usr/share/nmap/scripts/script.db`
           ![[Pasted image 20250117184525.png]]
           - what happens if one of these is missing in the `scripts` directory locally? A standard `sudo apt update && sudo apt install nmap` should fix this; however, it's also possible to install the scripts manually by downloading the script from Nmap (`sudo wget -O /usr/share/nmap/scripts/<script-name>.nse https://svn.nmap.org/nmap/scripts/<script-name>.nse`). This must then be followed up with `nmap --script-updatedb`, which updates the `script.db` file to contain the newly downloaded script.
### Firewall Evasion

Sometimes, a Windows firewall blocks **ICMP packets**, which are used by tools like `ping` and Nmap to check if a computer is "alive" or active on a network. When this happens, Nmap thinks the computer is **offline** and won't bother scanning it.

To get around this problem, you can use Nmap's **`-Pn` option**, which skips the ping check and treats the target as always "alive." This allows Nmap to scan the target even if ICMP is blocked.

### Key Points:

1. **Problem**: Firewalls block ICMP (like ping), making Nmap think a target is offline.
2. **Solution**: Use `-Pn` to skip the "Is this host alive?" check and scan it anyway.
3. **Drawback**: If the target is actually offline, the scan takes longer because Nmap still tries all the ports.
4. **Local Networks**: If you're on the same network, Nmap can use **ARP requests** instead of ICMP to check if a computer is active. ARP works even if ICMP is blocked.

This approach ensures you can still scan targets protected by a firewall, but it might take more time.

The following switches are of particular note:

- `-f`:- Used to fragment the packets (i.e. split them into smaller pieces) making it less likely that the packets will be detected by a firewall or IDS.
- An alternative to `-f`, but providing more control over the size of the packets: `--mtu <number>`, accepts a maximum transmission unit size to use for the packets sent. This _must_ be a multiple of 8.
- `--scan-delay <time>ms`:- used to add a delay between packets sent. This is very useful if the network is unstable, but also for evading any time-based firewall/IDS triggers which may be in place.
- `--badsum`:- this is used to generate in invalid checksum for packets. Any real TCP/IP stack would drop this packet, however, firewalls may potentially respond automatically, without bothering to check the checksum of the packet. As such, this switch can be used to determine the presence of a firewall/IDS.
- The `--data-length` option appends random data to packets to help bypass detection by firewalls/IDS. It's useful for stealthy scanning but may increase scan time.

In `nmap`, the command `nmap -A -T4 -Pn` performs the following:

1. **`-A`**: Enables advanced scanning features, including:
    
    - OS detection
    - Version detection
    - Script scanning
    - Traceroute
2. **`-T4`**: Sets the timing template to level 4, which is **faster** but still reliable. It's good for reasonably fast scans on less-reliable networks.
    
3. **`-Pn`**: Disables host discovery (ping scan) and treats all target hosts as **online**, even if they do not respond to ping requests.
    

This command is useful when you want a detailed scan of targets that may not respond to pings, and you need faster results with detailed information.

## useful command

nmap -vv -A -p- -oN scanport 10.10.110.118 | tee scanport.bak
### **Breakdown of Options:**

- **`nmap`** – Runs the Nmap network scanner.
- **`-vv`** – Increases verbosity level (shows detailed output).
- **`-A`** – Enables advanced scanning, including:
    - OS detection
    - Version detection
    - Script scanning
    - Traceroute
- **`-p-`** – Scans all **65,535** TCP ports (instead of just common ones).
- **`-oN scanport`** – Saves the scan results in normal output format to the file **`scanport`**.
- **`10.10.110.118`** – The target IP address being scanned.
- **`| tee scanport.bak`** – Duplicates the output:
    - Displays scan results in real-time on the terminal.
    - Saves a copy to **`scanport.bak`**.