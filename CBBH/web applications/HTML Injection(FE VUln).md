#### 🔹 **Input Validation & Sanitization**

- Must be done on **both front end and back end**.
    
- Protects against attacks like **HTML Injection** and **XSS**.
    
### ⚠️ **HTML Injection**

- Happens when **unfiltered user input** is displayed as HTML.
    
- Can occur via:
    
    - Data retrieved from the **back end** (e.g., user comments)
        
    - Input handled entirely on the **front end** (e.g., JavaScript prompts)
        

---

### 🧪 **Dangers of HTML Injection**

- **Malicious HTML** can:
    
    - Display **fake login forms** to steal credentials
        
     - Another example of `HTML Injection` is web page defacing. This consists of injecting new `HTML` code to change the web page's appearance, inserting malicious ads, or even completely changing the page
        
    - Insert **malicious ads or scripts**
        
- Leads to **reputational damage** and possible **data breaches**.
    

---

### 📄 **Example (Vulnerable Code)**

```html
<script>
    function inputFunction() {
        var input = prompt("Enter your name", "");
        if (input != null) {
            document.getElementById("output").innerHTML = "Your name is " + input;
        }
    }
</script>
```

- Vulnerable because it uses `innerHTML` without sanitizing input.
    

---

### ✅ **Prevention**

- **Sanitize user input** before displaying it.
    
- Use `.textContent` instead of `innerHTML` if HTML rendering is not needed.
    
- Never trust client-side input; always validate again on the server.
    
