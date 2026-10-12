`/proc/self/environ` is a **virtual file** in Linux that shows the **environment variables** of the **current process** accessing it.

---

### 🔍 Meaning of Each Part:

- `/proc`: virtual filesystem for process and system info.
    
- `self`: alias for the **current process ID** (i.e., the process accessing the file).
    
- `environ`: contains the **environment variables** of that process.
- In web exploitation (like LFI), if you access `/proc/self/environ` through a web server, it might reveal:

- `USER`, `PATH`, `HOME`, etc.
    
- Possibly leaked credentials or uploaded payloads (like in PHP via `User-Agent` injection).
---
**`/proc/self/cwd/app.py`** points to the `app.py` file in the **current working directory** of the running process.

### 🔧 Use:

- Read the script file via LFI or debug tools.
    
- Bypass path restrictions to access `app.py`.
    

📁 It's like saying:  
**“Give me the `app.py` where this process started from.”**

---
## Gobuster

```
gobuster vhost -u http://titanic.htb -w /usr/share/SecLists/Discovery/DNS/subdomains-top1million-20000.txt --append-domain -r
```

is used to enumerate virtual hosts (vhosts) on the titanic.htb domain.
🔍 Breakdown:

    gobuster vhost: Run Gobuster in VHOST mode (for virtual host discovery).

    -u http://titanic.htb: Target URL.

    -w <wordlist>: Wordlist of possible subdomains (like admin, dev, etc.).

    --append-domain: Appends .titanic.htb to each word in the list (e.g., admin.titanic.htb).

    -r: Follow redirects (useful if vhost redirects to the correct one).

