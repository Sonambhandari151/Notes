## **what is Sensitive Data Exposure?**

- Occurs when **sensitive info (e.g., credentials, hashes, hidden links)** is visible in the **front-end code** (HTML, JS).
    
- Usually found in:
    
    - Page source (`Ctrl + U`)
        
    - Developer comments
        
    - External JavaScript files
---
#### 🔎 **Risks**

- May lead to:
    
    - **Unauthorized access** (e.g., admin panel)
        
    - **Sensitive data leaks**
        
    - **Service disruption**
        
    - **Further back end exploitation**
        

---

#### 🔍 **Example**

```html
<!-- TODO: remove test credentials test:test -->
```

- Exposes credentials in HTML comments.
    

---

#### 🛠️ **Where to Look**

- View Page Source (`Ctrl + U`)
    
- Use tools like **Burp Suite** or **automated scanners**
    
- Check:
    
    - Comments
        
    - External JS files
        
    - Hidden form fields
        
    - URLs & directories
        

---

#### ✅ **Prevention**

- Do **not leave test credentials or comments** in production code.
    
- Minimize visible front-end code.
    
- **Obfuscate JavaScript** where needed.
    
- Classify & restrict what data is exposed to the client.
    
- Review and test client-side code before deployment.
    

---
