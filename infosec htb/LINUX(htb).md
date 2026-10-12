- Apropos : Search description for instance of a given keyword.
![[Pasted image 20250809185339.png]]

- [https://explainshell.com/](https://explainshell.com/) :  explains commands.

### new commands
`lsblk `:	Lists block devices.
`lsusb` :	Lists USB devices
`lsof` 	:Lists opened files.
`lspci `:	Lists PCI devices.
`uname -a`: In order details - kernel name, hostname, the kernel release, kernel version, machine hardware name, and operating system.


---
```
find / -type f -name *.conf -newermt 2020-03-03 -size +25k -size -28k -exec ls -la {} \; 2>/dev/null

```
- find .conf files newer than 2020-03-03 of size bw 25k -28k

---
```shell-session
 find /etc/ -name passwd >> stdout.txt 2>/dev/null
```
- output to stdout.txt and stderr to /dev/null
`>` : overwrite
`>>`: append

---
```
cat << EOF > stream.txt
```
- starts a **here document**.  
It means:

- Take everything you type until you write `EOF` on a line by itself.
    
- Redirect it into `stream.txt` (overwrite file).

---

```shell-session
find /etc/ -name *.conf 2>/dev/null | grep systemd
```
- Searches for `.conf` files under `/etc/` and filters the results to show only those with “systemd” in their path, hiding permission errors.

---

```shell-session
find /etc/ -name *.conf 2>/dev/null | grep systemd | wc -l
```
- word count for stdout.

---
```
apt list --installed | grep -c installed
```
- lists no of installed pkgs.
---

- `head`: first 10 lines of files by default.
- `tail`: last 10 lines by default.
- `sort`: sort alphabetically or numerically.
- `grep`: Search specific results.
- `grep -v`: exclude the specified filter.
- `cut`: `
- `cut -d":" -f1` : cut and st delimiter to : , output file position 1.
- `tr ":" " "`: used to replace characters. eg , replace : with space.
- `coumn -t`: represent O/P in tabular form.
- `awk '{print $1, $NF}'`:  - `$1` → first column
                          - `$NF` → last column (`NF` = number of fields).
- `sed/stream editor`: Substitute texts.
	    ` sed 's/bin/HTB/g'` : s - substitute command.
	                     bin: replace it.
	                     HTB: with HTB.
	                     g: replace all  matches.

- wc : word count.

---
Q1. How many services are listening on the target system on all interfaces? (Not on localhost and IPv4 only)
- netstat -l | grep LISTEN | grep -v localhost | grep 0.0.0.0

Q2. Determine what user the ProFTPd server is running under. Submit the username as the answer.
- ps aux | grep proftpd

Q3. Use cURL from your Pwnbox (not the target machine) to obtain the source code of the "https://www.inlanefreight.com" website and filter all unique paths (https://www.inlanefreight.com/directory" or "/another/directory") of that domain. Submit the number of these paths as the answer.
- curl https://www.inlanefreight.com | tr " " "\n" | grep -oE "https://www.inlanefreight.com/([^*#]+)" | sort -u | wc -l


---

**Regular Expressions (RegEx)**

- Patterns for matching, finding, replacing text with precision.
    
- Used in tools like `grep`, `sed`, many programming languages.
    
- Metacharacters define search rules, not literal characters.
    

**Grouping Operators**

1. **`(pattern)`** – Round brackets group parts of a regex.
    
2. **`[a-z]`** – Square brackets define a character class (set of allowed characters).
    
3. **`{1,10}`** – Curly brackets define a quantifier (number of repetitions).
    
4. **`|`** – OR operator, matches if either pattern exists.
    
5. **`.*`** – Acts like AND when used in sequence (both must appear in order).
    

**Examples**

- **OR** (`grep -E "(my|false)" /etc/passwd`) → matches lines containing _either_ “my” or “false”.
    
- **AND** (`grep -E "(my.*false)" /etc/passwd`) → matches lines containing “my” followed later by “false”.
    
- **Two greps** for AND (`grep "my" /etc/passwd | grep "false"`) → same result without complex regex.
    

---

# Permission management

![[Pasted image 20250810121039.png]]

- `chmod a+r shell && ls -l shell` :  Read  permission for all.
- `chmod 754 shell && ls -l shell`: 
   - `7` (owner) → **read + write + execute** (4+2+1) → `rwx`
    
   - `5` (group) → **read + execute** (4+0+1) → `r-x`
    
   - `4` (others) → **read only** (4+0+0) → `r--`

#### Chown
- `chown <user>:<group> <file/directory>` 

### SUID and SGID
**SUID** – Runs a file with the **owner’s privileges** (`chmod 4xxx`).  
**SGID** – Runs a file with the **group’s privileges** or makes new files inherit the directory’s group (`chmod 2xxx`).

EG :
- `4755` → SUID (`4`) + normal perms `755`
- `2755` → SGID (`2`) + normal perms `755`

---
#### Sticky bits
- **Sticky Bit (`t` / `T`)** → When set on a directory, only the **file’s owner**, the **directory’s owner**, or **root** can delete or rename files inside it.
    
- **`t` (lowercase)** → Sticky bit is set **and** “others” have execute (`x`) permission (can enter the directory).
    
- **`T` (uppercase)** → Sticky bit is set, but “others” **do not** have execute permission (cannot enter the directory)

---
### User management

| Command    | Description                                                                               |
| ---------- | ----------------------------------------------------------------------------------------- |
| `sudo`     | Execute command as a different user.                                                      |
| `su`       | Switch to another user account (default is root) after authentication, and start a shell. |
| `useradd`  | Create a new user or update default new user information.                                 |
| `userdel`  | Delete a user account and related files.                                                  |
| `usermod`  | Modify a user account.                                                                    |
| `addgroup` | Add a group to the system.                                                                |
| `delgroup` | Remove a group from the system.                                                           |
| `passwd`   | Change a user’s password.                                                                 |

---

1. **What package management is**
    
    - Packages = archives containing software binaries, configs, dependency info, and update metadata.
        
    - Package managers handle:
        
        - Downloading
            
        - Dependency resolution
            
        - Installation/removal
            
        - Standardized locations
            
        - Version tracking
            
        - Quality control
            
2. **Common package managers mentioned**
    
    - `dpkg` – low-level Debian package tool (works directly with `.deb` files).
        
    - `apt` – high-level tool for Debian/Ubuntu-based systems, automatically resolves dependencies.
        
    - `aptitude` – alternative frontend for APT.
        
    - `snap` – for self-contained snap packages.
        
    - `gem` – Ruby package manager.
        
    - `pip` – Python package manager.
        
    - `git` – version control system, also used to grab tools from GitHub.
        

---

### **APT in detail**

- Uses `.deb` packages + a dependency database called **APT cache**.
    
- Gets packages from repositories defined in `/etc/apt/sources.list` or `/etc/apt/sources.list.d/`.
    
- Can search (`apt-cache search <term>`), show info (`apt-cache show <package>`), list installed (`apt list --installed`), and install (`sudo apt install <package> -y`).
    

Example: Installing `impacket-scripts`:

```bash
sudo apt install impacket-scripts -y
```

---

### **Using Git**

- Once `git` is installed, you can clone projects from GitHub:
    

```bash
mkdir ~/nishang && git clone https://github.com/samratashok/nishang.git ~/nishang
```

---

### **Using DPKG directly**

- Useful for installing a `.deb` package downloaded manually:
    

```bash
wget http://archive.ubuntu.com/.../strace_4.21-1ubuntu1_amd64.deb
sudo dpkg -i strace_4.21-1ubuntu1_amd64.deb
```

---

# Service and Process Management
- **Services/daemons** run in background:
    
    - _System services_ are critical for OS functionality.
        
    - _User-installed services_ add extra features.

  - All processes in a Linux system are assigned a `PID` and can be viewed under the `/proc/` directory, which contains information about each process. Processes may also have a Parent Process ID (`PPID`), indicating that they were started by another process (the parent), making them child processes.


- Common **systemctl** commands:
    
    - `systemctl start <service>` — start a service now
        
    - `systemctl stop <service>` — stop it
        
    - `systemctl restart <service>` — restart
        
    - `systemctl status <service>` — check service status
        
    - `systemctl enable <service>` — enable on boot
        
    - `systemctl disable <service>` — disable on boot
        
    - `systemctl list-units --type=service` — list all running services
        
- For troubleshooting:
    
    - Use `journalctl -u <service>` to view logs for that service
        
- You can view running processes with `ps aux | grep <process_name>`
    

---

### Example:

```bash
systemctl start ssh
systemctl status ssh
systemctl enable ssh
ps aux | grep ssh
journalctl -u ssh --no-pager
```

---

### Kill a Process & Signals

- Processes can be **Running**, **Waiting**, **Stopped**, or **Zombie**.
    
- Use `kill`, `pkill`, `pgrep`, `killall` to send **signals** to control processes.
    
- List all signals:
    
    ```bash
    kill -l
    ```
    
- Common signals:
    
    - `SIGHUP (1)` — terminal closed
        
    - `SIGINT (2)` — Ctrl+C (interrupt)
        
    - `SIGQUIT (3)` — Ctrl+D (quit)
        
    - `SIGKILL (9)` — force kill, no cleanup
        
    - `SIGTERM (15)` — graceful termination
        
    - `SIGSTOP (19)` — stop process (can’t be caught)
        
    - `SIGTSTP (20)` — Ctrl+Z (suspend, user can resume)
        
- Kill a frozen process:
    
    ```bash
    kill -9 <PID>
    ```
    

---

### Background & Foreground Processes

- **Ctrl + Z** suspends current process (sends `SIGTSTP`).
    
- View suspended/background jobs:
    
    ```bash
    jobs
    ```
    
- Resume process in background:
    
    ```bash
    bg
    ```
    
- Start process directly in background:
    
    ```bash
    command &
    ```
    
- Bring background job to foreground:
    
    ```bash
    fg <job_number>
    ```
    

---

### Running Multiple Commands

- Commands separated by **semicolon (;)** run sequentially regardless of errors:
    
    ```bash
    cmd1; cmd2; cmd3
    ```
    
- Commands separated by **double AND (&&)** run next only if previous succeeds:
    
    ```bash
    cmd1 && cmd2 && cmd3
    ```
    
- Commands separated by **pipe (|)** send output of one as input to next; execution depends on previous command’s output.
    

---

# systemd Timers and Services

**systemd** is a service manager used in many Linux distributions like Ubuntu, RedHat, and Solaris. It can start processes and scripts automatically at specific times or intervals. This allows scheduling tasks based on time or system events.

---

## Key Concepts

- **Timer Unit:** Defines _when_ to run a service.
    
- **Service Unit:** Defines _what_ to run (a script or command).
    
- **Activation:** Enabling and starting the timer triggers the service execution automatically.
    

---

## Steps to Schedule a Task Using systemd

### 1. Create a Timer Unit

- Timer units control the schedule for running services.
    
- Timer files are usually stored in `/etc/systemd/system/` with `.timer` extension.
    
- Create a directory for timer configuration if needed:
    
    ```bash
    sudo mkdir /etc/systemd/system/mytimer.timer.d
    ```
    
- Create the timer unit file:
    
    ```bash
    sudo vim /etc/systemd/system/mytimer.timer
    ```
    
- Example `mytimer.timer` content:
    `
    ini
    [Unit]
    Description=My Timer
    
    [Timer]
    OnBootSec=3min            # Run 3 minutes after system boot
    OnUnitActiveSec=1hour     # Repeat every 1 hour
    
    [Install]
    WantedBy=timers.target    # Start this timer when timers.target is reached
`


- **Explanation:**
    
    - `OnBootSec=3min` triggers the timer 3 minutes after boot.
        
    - `OnUnitActiveSec=1hour` causes the timer to trigger repeatedly every hour.
        
    - You can adjust these values to run once or repeatedly.
        
    - `WantedBy=timers.target` enables the timer at the appropriate system target.
        

---

### 2. Create a Service Unit

- Service units specify the actual script or command to execute.
    
- Create the service file:
    
    ```bash
    sudo vim /etc/systemd/system/mytimer.service
    ```
    
- Example `mytimer.service` content:
    
    ini
    [Unit]
    Description=My Service
    
    [Service]
    ExecStart=/full/path/to/my/script.sh   # Specify the script to run
    
    [Install]
    WantedBy=multi-user.target              # Runs in normal multi-user mode
    
    
- **Explanation:**
    
    - `ExecStart` defines the absolute path to the script or command to execute.
        
    - `WantedBy=multi-user.target` means this service is part of normal multi-user system startup targets.
        

---

### 3. Reload systemd to Apply Changes

- Inform systemd about the new or changed unit files:
    
    ```bash
    sudo systemctl daemon-reload
    ```
    

---

### 4. Start and Enable the Timer

- Start the timer immediately:
    
    ```bash
    sudo systemctl start mytimer.timer
    ```
    
- Enable the timer to start on boot automatically:
    
    ```bash
    sudo systemctl enable mytimer.timer
    ```
    
- Once enabled, **mytimer.service** will run automatically according to the schedule defined in **mytimer.timer**.
    

---

# Cron — Task Scheduling in Linux

**Cron** is a traditional Linux daemon used to schedule and automate tasks at specific times or intervals.

---

## How Cron Works

- Tasks to run are specified in **crontab** files.
    
- Each line in a crontab defines a task and when it should run.
    
- The cron daemon reads crontabs and executes scheduled commands.
    

---

## Crontab Time Fields

| Field        | Allowed Values         | Description        |
| ------------ | ---------------------- | ------------------ |
| Minutes      | 0-59                   | Minute of the hour |
| Hours        | 0-23                   | Hour of the day    |
| Day of Month | 1-31                   | Day of the month   |
| Month        | 1-12                   | Month of the year  |
| Day of Week  | 0-7 (0 and 7 = Sunday) | Day of the week    |

---

## Example Crontab Entries

```cron
# System Update every 6 hours
0 */6 * * * /path/to/update_software.sh

# Run scripts on 1st day of every month at midnight
0 0 1 * * /path/to/scripts/run_scripts.sh

# Cleanup database every Sunday at midnight
0 0 * * 0 /path/to/scripts/clean_database.sh

# Backup every Sunday at midnight
0 0 * * 7 /path/to/scripts/backup.sh
```

---

## Explanation of Examples

- `0 */6 * * *`: Run at minute 0, every 6th hour.
    
- `0 0 1 * *`: Run at midnight on the 1st day of each month.
    
- `0 0 * * 0`: Run at midnight every Sunday (day 0).
    
- `0 0 * * 7`: Also Sunday (day 7).
    

---

## Additional Features

- Cron can send notifications on task success or failure.
    
- Logs can be created to monitor task execution.
    

---


### 1. SSH (Secure Shell)

- **Purpose:** Secure remote system access and management over encrypted channels.
    
- **Common Server:** OpenSSH (free, open-source).
    
- **Use:** Execute remote commands, transfer files securely.
    
- **Install:**
    
    ```bash
    sudo apt install openssh-server -y
    ```
    
- **Check status:**
    
    ```bash
    systemctl status ssh
    ```
    
- **Connect via SSH:**
    
    ```bash
    ssh user@remote-ip
    ```
    
- **Configuration:** `/etc/ssh/sshd_config` (controls login methods, max connections, keys, etc.)
    

---

### 2. NFS (Network File System)

- **Purpose:** Share and manage files across a network as if local.
    
- **Common Servers:** nfs-kernel-server (Ubuntu), NFS-Ganesha (Solaris).
    
- **Install:**
    
    ```bash
    sudo apt install nfs-kernel-server -y
    ```
    
- **Check status:**
    
    ```bash
    systemctl status nfs-kernel-server
    ```
    
- **Config file:** `/etc/exports` (defines shared directories and permissions).
    
- **Common Access Rights:**
    

| Permission       | Description                                       |
| ---------------- | ------------------------------------------------- |
| `rw`             | Read and write access                             |
| `ro`             | Read-only access                                  |
| `no_root_squash` | Client root retains root privileges               |
| `root_squash`    | Maps client root to normal user for security      |
| `sync`           | Writes data synchronously to disk                 |
| `async`          | Writes data asynchronously (faster but less safe) |

- **Example: Share a folder**
    
    ```bash
    echo '/path/to/share hostname(rw,sync,no_root_squash)' >> /etc/exports
    ```
    
- **Mount NFS share:**
    
    ```bash
    mount server_ip:/path/to/share /local/mount_point
    ```
    

---

### 3. Web Server

- **Purpose:** Serve websites, applications, files; used for hosting, file transfers, phishing simulations.
    
- **Popular Servers:** Apache, Nginx, Lighttpd, Caddy.
    
- **Install Apache:**
    
    ```bash
    sudo apt install apache2 -y
    ```
    
- **Apache config:** `/etc/apache2/apache2.conf`  
    Example for default web folder `/var/www/html`:
    
    ```apache
    <Directory /var/www/html>
      Options Indexes FollowSymLinks
      AllowOverride All
      Require all granted
    </Directory>
    ```
    
- **.htaccess:** Directory-level overrides for access control, mod_rewrite, security modules.
    
- **Python Simple HTTP Server:**
    
    ```bash
    python3 -m http.server 8000
    ```
    
    Or specify directory:
    
    ```bash
    python3 -m http.server --directory /path/to/folder 8000
    ```
    

---

### 4. VPN (Virtual Private Network)

- **Purpose:** Secure, encrypted tunnel connecting remote clients to a network; protects data confidentiality and user anonymity.
    
- **Common VPNs:** OpenVPN, L2TP/IPsec, PPTP, SSTP, SoftEther.
    
- **OpenVPN:**
    
    - Open-source and widely used.
        
    - Enables secure remote access to internal networks.
        
- **Install OpenVPN:**
    
    ```bash
    sudo apt install openvpn -y
    ```
    
- **Config file:** `/etc/openvpn/server.conf`
    
- **Connect using client config:**  
    Use `.ovpn` client configuration file with OpenVPN client.
    

---
Here are the important notes summarized from your detailed explanation on **Working with Web Services**:

---

### Working with Web Services

* **Apache Web Server:**

  * Apache is a widely used web server that powers websites by handling communication between browsers and servers.
  * It’s modular and extendable with modules like:

    * `mod_ssl` for secure encrypted communication (HTTPS),
    * `mod_proxy` for proxying and routing requests,
    * `mod_headers` and `mod_rewrite` for modifying HTTP headers and URLs dynamically.
  * Supports server-side scripting (PHP, Perl, Ruby, Python, etc.) for dynamic web content.
  * Installed via `sudo apt install apache2 -y`.
  * Default port is 80; changeable in `/etc/apache2/ports.conf` (e.g., to port 8080 if port 80 is occupied).
  * Restart Apache after changes using `sudo systemctl restart apache2`.
  * Default page (e.g., “It works!”) confirms Apache is running.

* **Communicating with Web Servers:**

  * Use **curl** to fetch and inspect web content from the command line. `curl -I http://localhost:8080`
  * Use **wget** to download files or entire websites.
  * Both tools are essential for testing and automating web interactions.

* **Python 3 Simple HTTP Server:**

  * Quickly start a basic web server in any directory with `python3 -m http.server`.
  * By default, serves on port 8000, accessible via browser or curl.
  * Useful for quick file sharing or testing without installing full web servers.


---
### Backup and Restore in Linux (Ubuntu)

**Purpose:**  
Tools to efficiently, securely back up and restore data locally or remotely, ensuring data safety and availability.

---

### Common Backup Tools:

- **Rsync:**
    
    - Efficient, open-source file synchronization tool.
        
    - Transfers only changed parts of files (incremental).
        
    - Can be used locally or over a network.
        
- **Duplicity:**
    
    - Builds on rsync with **encryption** support.
        
    - Secure backups to remote servers, cloud storage (e.g., Amazon S3).
        
    - Ideal for sensitive data requiring protection.
        
- **Deja Dup:**
    
    - User-friendly GUI frontend for backups.
        
    - Uses rsync and supports encrypted backups.
        
    - Good for users preferring simplicity.
        

---

### Backup Analogy:

- Rsync = fast transport sending only changed data.
    
- Duplicity = secure safe with encryption.
    
- Deja Dup = easy-to-use safe for everyone.
    

Encrypting backups adds a lock to protect data even if storage is compromised.

---

### Installing Rsync:

```bash
sudo apt install rsync -y
```

---

### Basic Rsync Commands:

- **Backup a directory to a remote server:**
    

```bash
rsync -av /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

- `-a` preserves permissions, timestamps, etc.
    
- `-v` verbose output.
    
- **Backup with compression, incremental backups, and deletion:**
    

```bash
rsync -avz --backup --backup-dir=/path/to/backup/folder --delete /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

- `-z` enables compression for faster transfer.
    
- `--backup` and `--backup-dir` create incremental backups.
    
- `--delete` removes files on remote no longer present locally.
    
- **Restore from backup:**
    

```bash
rsync -av user@backup_server:/path/to/backup/directory /path/to/mydirectory
```

---

### Securing Rsync with SSH:

- Use SSH to encrypt rsync data transfer:
    

```bash
rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

- Ensures confidentiality and integrity of data during transfer.
    

---

### Automating Backup with Cron:

1. **Generate SSH key for passwordless login:**
    

```bash
ssh-keygen -t rsa -b 2048
ssh-copy-id user@backup_server
```

2. **Create a backup script (`RSYNC_Backup.sh`):**
    

```bash
#!/bin/bash
rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

3. **Make the script executable:**
    

```bash
chmod +x RSYNC_Backup.sh
```

4. **Set up cron to run the script every hour:**
    

```bash
crontab -e
```

Add this line to crontab:

```
0 * * * * /path/to/RSYNC_Backup.sh
```

This schedules automatic hourly synchronization.

---

## File System Management

### File Systems Supported in Linux

- **ext2** — No journaling, lightweight, used for simple drives like USB.
    
- **ext3/ext4** — Journaling, ext4 is default on modern systems.
    
- **Btrfs** — Snapshots, data integrity.
    
- **XFS** — High performance, large files.
    
- **NTFS** — Windows-compatible file system.
    

---

### Inodes

- Store file metadata: permissions, owner, size, timestamps, pointers to data.
    
- Like library index cards that point to the actual books (files).
    

**View files with inode numbers:**

```bash
ls -il
```

---

### File Types

- **Regular files:** Text, binary, executable files.
    
- **Directories:** Containers for files/folders.
    
- **Symbolic links:** Shortcuts or references to other files/directories.
    

---

### Disk & Partition Management

**List disks and partitions:**

```bash
sudo fdisk -l
```

---

### Mounting

- **Mount** links a device/partition to a directory, making it accessible.
    

**Mount a USB drive:**

```bash
sudo mount /dev/sdb1 /mnt/usb
cd /mnt/usb && ls -l
```

**View mounted file systems:**

```bash
mount
```

**Unmount a drive:**

```bash
sudo umount /mnt/usb
```

**Check processes using mount point:**

```bash
lsof | grep /mnt/usb
```

---

### Automatic Mount on Boot

- Define mount points in `/etc/fstab` for auto-mount.
    

Example `/etc/fstab` entries:

```
/dev/sda1 / ext4 defaults 0 0
/dev/sdb1 /mnt/usb ext4 rw,noauto,user 0 0
```

---

### Swap Space

- Extends RAM by moving inactive pages to disk.
    
- Created with `mkswap`, activated with `swapon`.
    

**Create and enable swap (example):**

```bash
sudo mkswap /dev/sdXn    # Prepare swap partition
sudo swapon /dev/sdXn    # Enable swap
```

- Swap is also used for system hibernation.
    

---

Q : How many partitions exist in our Pwnbox?
- lsblk