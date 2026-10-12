VULNERABILITY SEARCHING

helpful websites to exploit specific software.

https://www.exploit-db.com/ 
https://nvd.nist.gov/vuln/search
https://cve.mitre.org/

you're inclined towards the CLI on Linux, Kali comes pre-installed with a tool called "searchsploit" which allows you to search ExploitDB from your own machine.

### Traceroute
Traceroute can be used to map the path your request takes as it heads to the target machine.
The internet is made up of many, many different servers and end-points, all networked up to each other. This means that, in order to get to the content you actually want, you first need to go through a bunch of other servers. Traceroute allows you to see each of these connections -- it allows you to see every intermediate step between your computer and the resource that you requested. The basic syntax for traceroute on Linux is this: `traceroute <destination>`

By default, the Windows traceroute utility (`tracert`) operates using the same ICMP protocol that ping utilises, and the Unix equivalent operates over UDP. This can be altered with switches in both instances.

![[Pasted image 20250114201917.png]]

### WHOIS
Whois essentially allows you to query who a domain name is registered to.
```
`sudo apt update && sudo apt-get install whois`
whois <domain>
```

### Dig
Ever wondered how a URL gets converted into an IP address that your computer can understand? The answer is a TCP/IP protocol called DNS (**D*
omain **N**ame **S**ystem).

Dig allows us to manually query recursive DNS servers of our choice for information about domains:  
`dig <domain> @<dns-server-ip>`

It is a very useful tool for network troubleshooting.![[Pasted image 20250114212300.png]]
