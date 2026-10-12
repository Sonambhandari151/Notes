## Introduction
- Web applications are interactive applications that run on a web browser.
- Client-server architecture.
- Frontend(client side) and backend (server/database).

## Web application vs Websites
- ### Traditional Websites (Web 1.0)

- **Static content**: Fixed pages created manually by developers.
    
- **No interactivity**: User interaction does not affect page content.
    
- **Update process**: Manual editing needed to change content.
    
- **Limited functionality**: No built-in functions or real-time updates.
    
- **Presentation only**: Acts like an online brochure or document.
    
- **Examples**: Early informational sites, personal blogs.
    

---

### 🔹 Web Applications (Web 2.0)

- **Dynamic content**: Content changes based on user input and interaction.
    
- **Interactive**: Can perform tasks like login, search, form submission.
    
- **Modular**: Built using reusable components (e.g., React, Angular).
    
- **Responsive**: Works on any screen size (desktop, mobile, tablet).
    
- **Cross-platform**: Runs on different OS/browser without special optimization.
    
- **Examples**: Gmail, Facebook, YouTube, online banking portals.

---
## Web application Vs Native OS application

### 🌐 **Web Applications**

- **Platform-independent**: Run in browsers on any OS (Windows, macOS, Linux, Android, etc.).
    
- **No installation required**: Executed on a remote server, not stored on the user’s device.
    
- **Centralized updates**: One version for all users, updated from the server.
    
- **Low maintenance cost**: No need to push updates to each device.
    
- **Limited hardware access**: Restricted to browser capabilities.
    
- **Slower** than native apps, especially for complex tasks.
    

---

### 💻 **Native OS Applications**

- **Platform-specific**: Designed for a specific OS (e.g., Android, Windows).
    
- **Installed locally**: Occupies space on the user's device.
    
- **High performance**: Faster due to local execution and access to OS libraries.
    
- **Full hardware access**: Deep integration with OS and hardware.
    
- **Separate builds** needed for each platform.
    
- **Costlier maintenance**: Updates must be pushed per platform/user.
    

### **Modern Approach** – Hybrid & Progressive Web Apps (PWAs)

- Combine **web app flexibility** with **native-like performance**.
    
- Can work **offline**, access **device features**, and offer **app-like UX**.
    
- Examples: Twitter Lite, Gmail PWA.
- ---

## 🌐 **Web Application Distribution**

### 🔸 **Open-Source Web Applications**

- Freely available and customizable by organizations.
    
- Common examples:
    
    - **WordPress**
        
    - **OpenCart**
        
    - **Joomla**
        

### 🔸 **Closed-Source (Proprietary) Web Applications**

- Developed by one organization, used via **purchase or subscription**.
    
- Customization limited; source code is not available.
    
- Common examples:
    
    - **Wix**
        
    - **Shopify**
        
    - **DotNetNuke**
        

---

## 🔐 **Security Risks of Web Applications**

### 🔸 **Why Web Applications Are Risky**

- Exposed to the internet — **accessible globally**.
    
- Often contain **large attack surfaces**.
    
- **Automated tools** can easily scan for and exploit vulnerabilities.
    
- Web apps grow complex, increasing the risk of **design-level flaws**.
    

### 🔸 **Potential Impacts of an Attack**

- **Sensitive data breaches** (user data, corporate data).
    
- **Server compromise**, leading to access to databases or internal systems.
    
- **Service downtime**, causing business interruption and losses.
    

### 🔸 **Prevention & Mitigation**

- **Regular vulnerability testing** and **patching**.
    
- Perform **penetration testing** and secure coding from the start.
    
- Use  [OWASP Web Security Testing Guide](https://github.com/OWASP/wstg/tree/master/document/4-Web_Application_Security_Testing). for structured testing.
    
- Test from:
    
    - **Unauthenticated** view (public access)
        
    - **Authenticated** view (logged-in user roles)
        

---

## 🛠️ **Web Application Penetration Testing Approach**

### 🔸 **Front-end Testing (HTML, CSS, JavaScript)**

- Look for:
    
    [Sensitive Data Exposure](https://owasp.org/www-project-top-ten/2017/A3_2017-Sensitive_Data_Exposure) and [Cross-Site Scripting (XSS)](https://owasp.org/www-project-top-ten/2017/A7_2017-Cross-Site_Scripting_\(XSS\))
        

### 🔸 **Back-end Testing**

- Analyze interaction between **browser and server**.
    
- Discover **server technologies** and hidden functionalities.
    
- Review login systems and **session handling**.
    

---

## 🚨 **Common Web Application Vulnerabilities & Attacks**
| Flaw                                                                                                                                                                                | Real-world Scenario                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [SQL injection](https://owasp.org/www-community/attacks/SQL_Injection)                                                                                                              | Obtaining Active Directory usernames and performing a password spraying attack against a VPN or email portal.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| [File Inclusion](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion) | Reading source code to find a hidden page or directory which exposes additional functionality that can be used to gain remote code execution.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| [Unrestricted File Upload](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)                                                                                | A web application that allows a user to upload a profile picture that allows any file type to be uploaded (not just images). This can be leveraged to gain full control of the web application server by uploading malicious code.                                                                                                                                                                                                                                                                                                                                                                                                  |
| [Insecure Direct Object Referencing (IDOR)](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html)                            | When combined with a flaw such as broken access control, this can often be used to access another user's files or functionality. An example would be editing your user profile browsing to a page such as /user/701/edit-profile. If we can change the `701` to `702`, we may edit another user's profile!                                                                                                                                                                                                                                                                                                                          |
| [Broken Access Control](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control)                                                                                   | Another example is an application that allows a user to register a new account. If the account registration functionality is designed poorly, a user may perform privilege escalation when registering. Consider the `POST` request when registering a new user, which submits the data `username=bjones&password=Welcome1&email=bjones@inlanefreight.local&roleid=3`. What if we can manipulate the `roleid` parameter and change it to `0` or `1`. We have seen real-world applications where this was the case, and it was possible to quickly register an admin user and access many unintended features of the web application |

---

## 🔁 **Example Attack Chain**

- **SQL Injection** → Extract emails → **Password spraying** on external services → **Account takeover**
    
- **File Upload** → Upload web shell → Access server → **Pivot to internal systems**
    

---

## 📘 **Why Learn Web Application Attacks?**

- Every company has at least one web app — **prime target for attackers**.
    
- Even minor code changes can introduce **severe vulnerabilities**.
    
- Mastering web app attacks sets you apart in:
    
    - **Penetration testing**
        
    - **Red teaming**
        
    - **Bug bounty**
        

---
