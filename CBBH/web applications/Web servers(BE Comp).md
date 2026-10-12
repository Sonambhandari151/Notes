
A **web server** is a backend application that processes **HTTP requests** from client-side browsers and returns the appropriate **HTTP responses**. It listens on **TCP port 80 (HTTP)** or **443 (HTTPS)**.

---

### ⚙️ **Workflow**

![[Pasted image 20250705082102.png]]

1. Client (e.g., smartphone, laptop) sends an HTTP request.
    
2. Web server routes request to correct page or service.
    
3. Server responds with:
    
    - **200 OK** (Success)
        
    - **404 Not Found**
        
    - **403 Forbidden**
        
    - etc.
        

---

### 📜 **Common HTTP Response Codes**

| Code | Type         | Description                |
| ---- | ------------ | -------------------------- |
| 200  | Success      | Request succeeded          |
| 301  | Redirect     | Resource moved permanently |
| 302  | Redirect     | Resource moved temporarily |
| 400  | Client Error | Bad request syntax         |
| 401  | Client Error | Unauthorized               |
| 403  | Client Error | Forbidden                  |
| 404  | Client Error | Not found                  |
| 405  | Client Error | Method not allowed         |
| 408  | Client Error | Request timeout            |
| 500  | Server Error | Internal error             |
| 502  | Server Error | Bad gateway                |
| 504  | Server Error | Gateway timeout            |

---

### 📦 **Web Server Inputs**

- Accepts: Text, JSON, binary (e.g., file uploads)
    
- Routes to core web application files
    
- Executes server-side logic, then responds
    

---

### 🛠️ **Example with `curl`**

- `curl -I <URL>` → Shows headers
    
- `curl <URL>` → Shows HTML content (page source)
    

---

### 🔧 **Popular Web Server Technologies**

- **Languages:** Python, JavaScript (Node.js), PHP, etc.
    
- **Servers:** Apache, Nginx, IIS (optimized for large traffic)
    
---
### 🅰️ **Apache (httpd) Web Server 

- **Most widely used** web server — powers over **40%** of websites.
    
- Works on **Linux (pre-installed)**, **Windows**, and **macOS**.
    
- Commonly paired with **PHP**, but supports:
    
    - **Python**
        
    - **.NET**
        
    - **Perl**
        
    - **Bash** (via CGI)
        

---

### ⚙️ **Apache Functionality**

- Extended using **modules**, e.g.:
    
    - `mod_php` → serves PHP files
        
- Requires back-end **language installation** (e.g., PHP) + proper module.
    

---

### 🔓 **Open Source & Security**

- **Open-source** → Source code is accessible for auditing.
    
- **Regularly patched** to fix vulnerabilities.
    
- **Well-documented**, making setup and troubleshooting easier.
    

---

### 🏢 **Used by:**

- **Startups & small businesses** (ease of development)
    
- **Big companies** like:
    
    - **Apple**
        
    - **Adobe**
        
    - **Baidu**
        

---

Here are **well-structured notes** on **NGINX** and **IIS**, strictly based on the content you provided earlier:

---

## 🟩 **NGINX Web Server**

- **Second most common** web server (~30% of all internet websites).
    
- Designed to handle **many concurrent web requests** with low memory and CPU usage.
    
- Uses an **asynchronous (async) architecture** for performance and efficiency.
    
- Most popular among **high-traffic websites**: ~**60% of the top 100,000** use NGINX.
    
### ✅ Key Features

- Efficient with **high concurrency**
    
- **Low resource usage**
    
- Serves both **static and dynamic content**
    
- Highly **reliable** and **scalable**
    
- **Open-source** and free

### 🔒 Security & Community

- Benefits of open-source:
    
    - Transparent source code
        
    - Community-driven development
        
    - Regular updates and patches
        
### 🌐 Popular Users

- **Google**
    
- **Facebook**
    
- **Twitter**
    
- **Cisco**
    
- **Intel**
    
- **Netflix**
    
- **HackTheBox**
    

---

## 🟦 **IIS (Internet Information Services)**
- **Third most common** web server (~15% of websites).
    
- Developed and maintained by **Microsoft**.
    
- Runs mainly on **Microsoft Windows Servers**.

### ✅ Key Features

- Optimized for **Microsoft .NET framework**.
    
- Can also host:
    
    - **PHP applications**
        
    - **FTP services**
        
- Well-suited for **Active Directory integration**.
    
- Supports **Windows Authentication** for seamless logins.
    
### 🏢 Ideal Use Case

- Organizations using **Windows Server** and **Active Directory**.
    
- Enterprises running **.NET-based web applications**.
    
- Environments requiring **integrated user authentication**.
### 🌐 Popular Users

- **Microsoft**
    
- **Office365**
    
- **Skype**
    
- **Stack Overflow**
    
- **Dell**
    
---
