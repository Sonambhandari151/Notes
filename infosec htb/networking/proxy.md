- Operate at layer 7

## 1. Dedicated /Forward Proxy

**What it is:**

- A server that sits between you (the client) and the Internet.
    
- You send requests to the proxy → the proxy sends them to the website/server → sends the response back to you.
    

---

### **Why it’s used**

- **Control & Security** – Blocks unwanted sites, filters traffic, logs activity.
    
- **Protection** – Stops malware from talking directly to the Internet.
    

---

### **Corporate Example**

- Computers in a company can’t directly access the Internet.
    
- They must go through the proxy (or web filter).
    
- If malware is on one of these computers, it must:
    
    1. Know there’s a proxy.
        
    2. Be able to use it to send data.
        

---

### **Browser Behavior**

- **Internet Explorer / Edge / Chrome**: Use **System Proxy** settings automatically → more likely to be proxy-aware.
    
- **Firefox**: Uses its own settings (**libcurl**) → malware would have to find these separately (unlikely).
    

---

### **Malware Evasion**

- Could try using **DNS** as a communication channel.
    
- But if DNS traffic is being monitored (e.g., with **Sysmon**), this can be detected quickly.
    

---

### **Tools Example**

- **Burp Suite** – Often used as a forward proxy to intercept web requests.
    
    - Can also work as a **Reverse Proxy** or **Transparent Proxy**.
        

![[Pasted image 20250812211722.png]]

---
## **Reverse Proxy**

**What it is:**

- The opposite of a forward proxy.
    
- **Forward Proxy** → filters outgoing traffic (client → Internet).
    
- **Reverse Proxy** → filters incoming traffic (Internet → internal servers).
    
- It listens on a public address and forwards requests to private/internal servers.
    

---

### **Why it’s used**

- **Security** – Filters, inspects, and blocks harmful incoming requests.
    
- **Protection** – Hides the real server from the public.
    
- **Performance** – Can cache responses and balance traffic between multiple servers.
    
- **DDOS Protection** – Absorbs and filters massive amounts of traffic before it hits the real server.
    

---

### **Common Examples**

1. **Cloudflare**
    
    - Protects websites from **DDoS attacks**.
        
    - Can also filter and inspect incoming requests.
        
    - Can act as a **WAF** (Web Application Firewall) if HTTPS traffic is decrypted by them.
        
2. **ModSecurity (WAF)**
    
    - Inspects requests for malicious content.
        
    - Uses **Core Rule Set (CRS)** to detect attacks (SQLi, XSS, etc.).
        
    - Blocks requests that match attack patterns.
        

---

### **In Penetration Testing / Attacks**

- Reverse proxies can be used **offensively**:
    
    - Example: Attacker controls an infected endpoint inside a target network.
        
    - Endpoint listens on a port → forwards requests back to the attacker.
        
    - Can send traffic through **SSH tunnels** to bypass:
        
        - **Firewalls**
            
        - **IDS (Intrusion Detection Systems)**
            
![[Pasted image 20250812212236.png]]

---


## **Transparent vs Non-Transparent Proxy**

### **Transparent Proxy**

- **Client is unaware** of the proxy’s presence.
    
- Intercepts communication automatically.
    
- No special configuration needed on the client side.
    
- Common in:
    
    - Public Wi-Fi filtering
        
    - Corporate monitoring without user setup
        
- From the Internet’s perspective, works like any other proxy — just invisible to the user.
    

---

### **Non-Transparent Proxy**

- **Client is aware** of the proxy.
    
- Requires **manual configuration** in the software or system settings:
    
    - Proxy IP/hostname
        
    - Port
        
    - Authentication (if required)
        
- Without configuration, the client **cannot access** the Internet (if proxy is the only allowed path).
    
- Common in:
    
    - Corporate networks with strict outbound rules
        
    - Tools like Burp Suite during testing
        

---

**Key Difference:**

> **Transparent** → No setup needed, works silently.  
> **Non-Transparent** → Must be configured by the user/software to work.

---
