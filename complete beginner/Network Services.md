## SMB(Server Message Block) Protocol

- Server Message Block Protocol - is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network.
- The SMB protocol is known as a response-request protocol, meaning that it transmits multiple messages between the client and server to establish a connection. Clients connect to servers using TCP/IP (actually NetBIOS over TCP/IP as specified in RFC1001 and RFC1002), NetBEUI or IPX/SPX.
- ![[Pasted image 20250117214557.png]]
- Once they have established a connection, clients can then send commands (SMBs) to the server that allow them to access shares, open files, read and write files, and generally do all the sort of things that you want to do with a file system. However, in the case of SMB, these things are done over the network.
- **What runs SMB?**
    Microsoft Windows operating systems since Windows 95 have included client and server SMB protocol support. Samba, an open source server that supports the SMB protocol, was released for Unix systems.

## Enumerating SMB

ENUMERATION
- Enumeration is the process of gathering information on a target in order to find potential attack vectors and aid in exploitation.
- Enumeration can be used to gather usernames, passwords, network information, hostnames, application data, services, or any other information that may be valuable to an attacker.
**SMB**  

Typically, there are SMB share drives on a server that can be connected to and used to view or transfer files. SMB can often be a great starting point for an attacker looking to discover sensitive information — you'd be surprised what is sometimes included on these shares.
The first step of enumeration is to conduct a port scan, to find out as much information as you can about the services, applications, structure and operating system of the target machine.

**Enum4Linux**

Enum4linux is a tool used to enumerate SMB shares on both Windows and Linux systems. It is basically a wrapper around the tools in the Samba package and makes it easy to quickly extract information from the target pertaining to SMB.

**TAG**            **FUNCTION**  

-U             get userlist  
-M             get machine list  
-N             get namelist dump (different from -U and-M)  
-S             get sharelist  
-P             get password policy information  
-G             get group and member list

-a             all of the above (full basic enumeration)

### Exploiting SMB
#### SMBclient
**`smbclient`** is a command-line tool that allows you to interact with **SMB (Server Message Block)** and **CIFS (Common Internet File System)** shares from a Linux or Unix system. It's part of the **Samba suite** and works like an FTP client, enabling you to browse, upload, download, or manage files on a remote SMB server.
`smbclient //[IP]/[SHARE]`

Followed by the tags:

-U [name] : to specify the user

-p [port] : to specify the port

### Telnet

Telnet is an application protocol which allows you, with the use of a telnet client, to connect to and execute commands on a remote machine that's hosting a telnet server.  

The telnet client will establish a connection with the server. The client will then become a virtual terminal- allowing you to interact with the remote host.  

**Replacement**  

Telnet sends all messages in clear text and has no specific security mechanisms. Thus, in many applications and services, Telnet has been replaced by SSH in most implementations.  
   
****How does Telnet work?****

The user connects to the server by using the Telnet protocol, which means entering **"telnet"** into a command prompt. The user then executes commands on the server by using specific Telnet commands in the Telnet prompt. You can connect to a telnet server with the following syntax: **"telnet [ip] [port]"**


**What is a Reverse Shell?**

**![](https://i.imgur.com/EUC7VS6.png)**

A **"shell"** can simply be described as a piece of code or program which can be used to gain code or command execution on a device.

A reverse shell is a type of shell in which the target machine communicates back to the attacking machine.

The attacking machine has a listening port, on which it receives the connection, resulting in code or command execution being achieved.

## FTP
File Transfer Protocol (FTP) is, as the name suggests , a protocol used to allow remote transfer of files over a network
****How does FTP work?****

A typical FTP session operates using two channels:

- a command (sometimes called the control) channel
- a data channel.

As their names imply, the command channel is used for transmitting commands as well as replies to those commands, while the data channel is used for transferring data.

FTP operates using a client-server protocol. The client initiates a connection with the server, the server validates whatever login credentials are provided and then opens the session.

While the session is open, the client may execute FTP commands on the server.

**Active vs Passive**

The FTP server may support either Active or Passive connections, or both. 

- In an Active FTP connection, the client opens a port and listens. The server is required to actively connect to it. 
- In a Passive FTP connection, the server opens a port and listens (passively) and the client connects to it. 

This separation of command information and data into separate channels is a way of being able to send commands to the server without having to wait for the current data transfer to finish. If both channels were interlinked, you could only enter commands in between data transfers, which wouldn't be efficient for either large file transfers, or slow internet connections.

## Network File System(NFS)
NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.
HOW NFS WORKS:
https://docs.oracle.com/cd/E19683-01/816-4882/6mb2ipq7l/index.html

## RPC
**RPC (Remote Procedure Call)** is a protocol that allows a program to execute code on a remote system as if it were a local function. It enables communication between distributed systems, making remote services appear transparent to users and applications.

### **RPC in NFS**

NFS relies on **RPC services** to handle file-sharing requests. Some key RPC-based daemons in NFS include:

1. **`rpc.mountd`** – Handles client mount requests.
2. **`rpc.nfsd`** – Processes file access requests.
3. **`rpc.statd`** – Monitors file locks and detects server crashes.
4. **`rpc.lockd`** – Manages file locking for concurrent access.

RPC enables NFS to function efficiently by allowing clients to interact with remote file systems seamlessly.
- If someone wants to access a file using NFS, an RPC call is placed to NFSD (the NFS daemon) on the server. This call takes parameters such as:

-  The file handle
-  The name of the file to be accessed
-  The user's, user ID
-  The user's group ID


**NFS-Common**

It is important to have this package installed on any machine that uses NFS, either as client or server. It includes programs such as: **l****ockd, statd**, **showmount**, **nfsstat,** **gssd**, **idmapd** and **mount.nfs**. Primarily, we are concerned with "showmount" and "mount.nfs" as these are going to be most useful to us when it comes to extracting information from the NFS share.

**Mounting NFS shares**
**sudo mount -t nfs IP:share /tmp/mount/ -nolock**

|**Tag**|**Function**|
|sudo|Run as root|
|mount|Execute the mount command|
|-t nfs|Type of device to mount, then specifying that it's NFS|
|IP:share|The IP Address of the NFS server, and the name of the share we wish to mount|
|-nolock|Specifies not to use NLM locking|


## Exploiting NFS
#### Root Squash
**Root Squash** is a security feature in **NFS (Network File System)** that prevents remote users from having **root privileges** on shared files.

### How It Works:

- When **root** from a remote system tries to access the NFS share, the server **downgrades** their permissions to a low-privileged user, usually **"nfsnobody"**.
- This prevents remote root users from making dangerous changes to files on the server.

### Why Disable It?

- If you **turn off** root squash, the remote root user **keeps** their root privileges on the NFS share.
- This allows them to create **SUID files**—special files that let anyone execute them with root privileges.
- If misused, this could give an attacker **root access** to the system, creating a serious security risk.

So, while disabling **root squash** might be useful in some cases, it **increases security risks** significantly.

## SUID
A **SUID (Set User ID) bit** is a special permission in Linux that allows a file to run **with the privileges of its owner**, rather than the user executing it.
### **How It Works:**
- Normally, when you run a program, it runs with **your** permissions.
- If a file has the **SUID bit set**, it runs **as the file owner** (often root), regardless of who executes it.

### **SMTP (Simple Mail Transfer Protocol)**

SMTP is a system used to **send emails**. When you send an email, SMTP is responsible for:

1. Checking if the sender is allowed to use the email server.
2. Sending your email to the recipient’s email server.
3. If the email can't be delivered, SMTP will let you know and send the email back to you.

### **POP (Post Office Protocol) and IMAP (Internet Message Access Protocol)**

Both POP and IMAP are systems used to **retrieve emails** from a mail server, but they work in different ways:

- **POP** downloads the emails from the server to your device (like your computer or phone), and once downloaded, the emails are stored only on your device and removed from the server.
    
- **IMAP** keeps the emails on the server, so if you check your email from multiple devices (like your phone and computer), the inbox will always be the same on both. It only downloads new emails when you check them.
    

In short, SMTP helps you send emails, while POP and IMAP help you receive and manage your emails.
## how does SMTP work
- **Starting the Process**:  
    You use an email program (like Gmail or Outlook) to send an email. This program connects to your email provider's **SMTP server** (like smtp.google.com) through a specific port (usually 25). This starts the process of sending the email.
    
- **Submitting the Email**:  
    Your email program tells the SMTP server who the sender is, who the recipient is, and what the email says (including any attachments like files or pictures).
    
- **Checking the Domains**:  
    The SMTP server checks if the sender's email domain (like @google.com) matches the recipient's email domain. If they are the same, the email stays within the same server. If not, it continues to the next step.
    
- **Sending to Recipient's SMTP Server**:  
    If the domains are different, the SMTP server sends the email to the **recipient's SMTP server** (like smtp.yahoo.com). If that server is down or can't be reached, the email is placed in a queue and tried again later.
    
- **Recipient's SMTP Server Verifies the Email**:  
    The recipient's SMTP server checks if the recipient's email address is valid and if the email is from a trusted source. Then, it passes the email to the recipient's **POP or IMAP server**.
    
- **Email Arrives in Inbox**:  
    Finally, the email shows up in the recipient’s inbox, and they can read it!
    ![[Pasted image 20250206202912.png]]

**Fingerprinting SMTP Servers:**

When we want to gather information about a mail server before attacking it, we need to know things like the version of the server. The **"smtp_version"** module in **Metasploit** helps with this by scanning a range of IP addresses to find any mail servers and determine their version.

This is useful because knowing the version of the mail server can give us insights into potential vulnerabilities or weaknesses specific to that version, helping us target the system more effectively.

**Enumerating Users from SMTP:**

SMTP (Simple Mail Transfer Protocol) is the protocol used for sending emails. SMTP servers have internal commands that allow us to check if specific users exist on the server:

- **VRFY Command**: It allows us to check if a username is valid.
- **EXPN Command**: It shows the actual email addresses associated with user aliases or mailing lists.

For example, if we ask an SMTP server about a username (using VRFY), the server will tell us if that user exists.

Instead of doing this manually by connecting to the SMTP server using **Telnet** and typing commands, **Metasploit** provides a module called **"smtp_enum"**. This module automates the process of enumerating (listing) users from the SMTP server.

### How It Works:

1. **"smtp_enum" Module**:
    
    - You give the module a list of hosts (servers) to scan.
    - You provide a wordlist of usernames to check.
    - The module uses the **VRFY** and **EXPN** commands to find valid users or mailing list details.

## hydra
The syntax for the command we're going to use to find the passwords is this:

**"hydra -t 16 -l USERNAME -P /usr/share/wordlists/rockyou.txt -vV 10.10.58.148 ssh"**

Let's break it down:

  

|                         |                                                                                      |
| ----------------------- | ------------------------------------------------------------------------------------ |
| **SECTION**             | **FUNCTION**                                                                         |
| hydra                   | Runs the hydra tool                                                                  |
| -t 16                   | Number of parallel connections per target                                            |
| -l [user]               | Points to the user who's account you're trying to compromise                         |
| -P [path to dictionary] | Points to the file containing the list of possible passwords                         |
| -vV                     | Sets verbose mode to very verbose, shows the login+pass combination for each attempt |
| [machine IP]            | The IP address of the target machine                                                 |
| ssh / protocol          | Sets the protocol                                                                    |
### ==John the ripper==
==is a popular password cracking tool that supports various hash algorithms.==