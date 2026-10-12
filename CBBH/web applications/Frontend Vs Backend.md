## Frontend
- The **front end** is what users see and interact with in their **web browser**.
    
- Built using **HTML** (structure), **CSS** (design), and **JavaScript** (functionality).
    
- Interpreted **in real-time by the browser**.
### 🔹 Includes:

- Titles, text → **HTML**
    
- Layout, colors, animation → **CSS**
    
- Page behavior & interactivity → **JavaScript**
### 📱 Key Features:

- Must **adapt to all screen sizes** (responsive)
    
- Should **work on all browsers/devices**
    
- Poor front-end design = **slow/unresponsive site** (even if the backend is fine)
    

### 🛠️ Other Tasks related to frontend web app development:

- **Visual Concept web design**
    
- **UI (User Interface) design**
    
- **UX (User Experience) design**
---

## Back End
Four main components for web app backend.

| component              | Description                                                                                                                                                                                                                 |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Back end Servers       | The hardware and operating system that hosts all other components and are usually run on operating systems like `Linux`, `Windows`, or using `Containers`.                                                                  |
| Web Servers            | Web servers handle HTTP requests and connections. Some examples are `Apache`, `NGINX`, and `IIS`.                                                                                                                           |
| Databases              | atabases (`DBs`) store and retrieve the web application data. Some examples of relational databases are `MySQL`, `MSSQL`, `Oracle`, `PostgreSQL`, while examples of non-relational databases include `NoSQL` and `MongoDB`. |
| Development Frameworks | Development Frameworks are used to develop the core Web Application. Some well-known frameworks include `Laravel` (`PHP`), `ASP.NET` (`C#`), `Spring` (`Java`), `Django` (`Python`), and `Express` (`NodeJS JavaScript`).   |
![[Pasted image 20250703224345.png]]
- **Back-end components** can be hosted on:
    
    - **Isolated servers** (resource-heavy), or
        
    - **Isolated Docker containers** (lightweight, efficient)
        
- Example:
    
    - **Database** in one Docker container
        
    - **Web app** in another  
        → This ensures **separation** and **security** — one part’s vulnerability doesn’t affect the others.
        
### 🔧 Main Jobs of Back-End Components:

- Develop **main logic** and **core services** of the web app
    
- Build **main application code** and features
    
- **Develop & maintain the database**
    
- Create and manage **libraries** used by the app
    
- Fulfill **technical and business requirements**
    
- Implement **APIs** for front-end communication
    
- **Integrate** with cloud services or other remote servers
    

---
## **Securing Front/Back End** 

### ⚠️ Access Limitations:

- Usually, we **don’t have access** to **back end code**
    
- But the app can still be vulnerable to:
    
    - **SQL Injection**
        
    - **Command Injection**
### 🔍 Example:

- A **search function** not handling input properly  
    → attacker may inject malicious queries  
    → gain **unauthorized database access** or run **OS commands**
    
### 🧪 Code Review & Pentesting:

#### 🔹 Front End:

- **Source code is accessible** (HTML, JS, CSS)
    
- We can **review it for vulnerabilities**
    
- Part of **Whitebox Pentesting**
    

#### 🔹 Back End:

- **Source code not accessible by default**
    
- Usually tested through **Blackbox Pentesting**
    
- But possible to access via:
    
    - **Open-source apps**
        
    - **Local File Inclusion (LFI)**
        

> If source code is obtained → we can perform **code review** to:

- Understand app logic
    
- Find **hardcoded secrets (e.g., passwords)**
    
- Discover hidden **vulnerabilities**
    
---
he `top 20` most common mistakes web developers make that are essential for us as penetration testers are:

| **No.** | **Mistake**                                        |
| ------- | -------------------------------------------------- |
| `1.`    | Permitting Invalid Data to Enter the Database      |
| `2.`    | Focusing on the System as a Whole                  |
| `3.`    | Establishing Personally Developed Security Methods |
| `4.`    | Treating Security to be Your Last Step             |
| `5.`    | Developing Plain Text Password Storage             |
| `6.`    | Creating Weak Passwords                            |
| `7.`    | Storing Unencrypted Data in the Database           |
| `8.`    | Depending Excessively on the Client Side           |
| `9.`    | Being Too Optimistic                               |
| `10.`   | Permitting Variables via the URL Path Name         |
| `11.`   | Trusting third-party code                          |
| `12.`   | Hard-coding backdoor accounts                      |
| `13.`   | Unverified SQL injections                          |
| `14.`   | Remote file inclusions                             |
| `15.`   | Insecure data handling                             |
| `16.`   | Failing to encrypt data properly                   |
| `17.`   | Not using a secure cryptographic system            |
| `18.`   | Ignoring layer 8                                   |
| `19.`   | Review user actions                                |
| `20.`   | Web Application Firewall misconfigurations         |

---
These mistakes lead to the [OWASP Top 10](https://owasp.org/www-project-top-ten/) vulnerabilities for web applications.

| **No.** | **Vulnerability**                          |
| ------- | ------------------------------------------ |
| `1.`    | Broken Access Control                      |
| `2.`    | Cryptographic Failures                     |
| `3.`    | Injection                                  |
| `4.`    | Insecure Design                            |
| `5.`    | Security Misconfiguration                  |
| `6.`    | Vulnerable and Outdated Components         |
| `7.`    | Identification and Authentication Failures |
| `8.`    | Software and Data Integrity Failures       |
| `9.`    | Security Logging and Monitoring Failures   |
| `10.`   | Server-Side Request Forgery (SSRF)         |

---
