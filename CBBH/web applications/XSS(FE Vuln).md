#### 🔹 **What is XSS?**

- A type of vulnerability that allows attackers to **inject JavaScript** into web pages viewed by other users.
    
- XSS can be used to:
    
    - **Steal cookies/session tokens**
        
    - **Hijack user accounts**
        
    - **Redirect users to malicious sites**
        
    - **Control browser behavior**
        

### 🧬 **Relation to HTML Injection**

- XSS is an advanced form of **HTML Injection**.
    
- Instead of injecting just HTML, attackers inject **JavaScript code**.
    
- Example payload:
    
    ```html
    #"><img src=/ onerror=alert(document.cookie)>
    ```
    
    - Pops up the user's **cookie** in an alert box (DOM XSS).
        

---

### 📚 **Types of XSS**

| Type          | Description                                                                                                                                                                                                              |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Reflected** | Input is reflected in the page immediately (e.g., in search or error pages)                                                                                                                                              |
| **Stored**    | Input is stored in the **back end** and later displayed (e.g., comments)                                                                                                                                                 |
| **DOM**       | Input affects the **DOM directly** via JavaScript without back end processing. Occurs when user input is directly shown in the browser and is written to an `HTML` DOM object (e.g., vulnerable username or page title). |

---

### 🛡️ **Impact**

- Theft of session cookies
    
- Unauthorized access to accounts
    
- Browser exploitation
    
- Full control over user interaction
    

---

### ✅ **Prevention**

- **Sanitize** and **escape** all user input/output.
    
- Use `textContent` instead of `innerHTML` when rendering text.
    
- Implement **Content Security Policy (CSP)**.
    
- Use **HTTP-only cookies** to protect from JavaScript access.
----
