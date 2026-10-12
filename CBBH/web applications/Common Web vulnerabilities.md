 (OWASP Top 10 Based)

---

### 1. **Broken Authentication / Access Control**

#### 🔹 Broken Authentication:

- Happens when attackers **bypass login mechanisms**.
    
- They may:
    
    - Log in without valid credentials.
        
    - Gain higher privileges (e.g., become admin).
        

**Example:**

- In _College Management System 1.2_, authentication can be bypassed using:
    
    ```
    Email: ' or 0=0 #
    Password: anything
    ```
    

#### 🔹 Broken Access Control:

- Attackers gain access to **restricted pages/features**.
    
- Example: A normal user accessing the admin dashboard.
    

---

### 2. **Malicious File Upload**

- Web applications may allow file uploads (e.g., profile pictures).
    
- If **file types are not validated**, attackers can upload **malicious scripts** (like `shell.php`) to gain server access.
    

#### 🔹 Common Bypasses:

- Using **double extensions** like `shell.php.jpg`
    
- Renaming executable files to look harmless
    

**Example:**

- _WordPress Plugin Responsive Thumbnail Slider 1.0_:
    
    - Allows arbitrary file upload.
        
    - Can be exploited using Metasploit.
        

---

### 3. **Command Injection**

- Happens when user input is used in **OS-level commands**.
    
- If input is not sanitized, attackers can inject **extra commands**.
    

#### 🔹 Effect:

- Remote code execution on the **back-end server**.
    

**Example:**

- _WordPress Plugin Plainview Activity Monitor 20161228_:
    
    - Vulnerable via the `ip` parameter:
        
        ```
        ip=127.0.0.1 | whoami
        ```
        

---

### 4. **SQL Injection (SQLi)**

- Occurs when user input is used directly in **SQL queries** without proper validation.
    
- Allows:
    
    - Unauthorized login
        
    - Data theft
        
    - Full server/database takeover
        

**Example (Insecure Code):**

```php
$query = "select * from users where name like '%$searchInput%'";
```

**Injection Payload:**

```
' OR 1=1 --
```

**Affected System:**

- _College Management System 1.2_ also has SQLi vulnerabilities.
    

---
