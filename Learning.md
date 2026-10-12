Nmap uses port-services database of well known services to know what's running on a particular port.
- Windows Remote Management, or WinRM, is a Windows-native built-in remote management protocol
that basically uses Simple Object Access Protocol to interact with remote computers and servers, as well as
Operating Systems and applications. WinRM allows the user to :
→ Remotely communicate and interface with hosts
→ Execute commands remotely on systems that are not local to you but are network accessible.
→ Monitor, manage and configure servers, operating systems and client machines from a remote location


### Name based virtual hosting


**Name-Based Virtual Hosting** lets **one web server serve multiple websites** from a **single IP address**, based on the **domain name (Host header)** in the HTTP request.

### ✅ **How It Works**

- You have **one server**, one IP — but many domains like:
    
    - `unika.htb`
        
    - `blog.unika.htb`
        
    - `admin.unika.htb`
        
- The **web server (Apache/Nginx)** reads the `Host:` header in the HTTP request and serves the matching site.
    

---

### 📌 Example

If browser sends:

```
GET / HTTP/1.1
Host: unika.htb
```

Apache knows to serve the `unika.htb` site, not some other site hosted on the same IP.

---

### 🧱 How to Make It Work in HTB/CTF

1. Add the domain to `/etc/hosts` so your system resolves it:
    
    ```bash
    echo "10.129.128.223 unika.htb" | sudo tee -a /etc/hosts
    ```
    
2. Then visit:
    
    ```
    http://unika.htb
    ```
    
3. Apache/Nginx sees `Host: unika.htb` and serves the right page.
    

---

