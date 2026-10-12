A Poison Null Byte is actually a NULL terminator. By placing a NULL character in the string at a certain byte, the string will tell the server to terminate at that point, nulling the rest of the string.

When Broken Access Control exploits or bugs are found, it will be categorised into one of **two types**:

|                                     |                                                                                                                 |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Horizontal** Privilege Escalation | Occurs when a user can perform an action or access data of another user with the **same** level of permissions. |
| **Vertical** Privilege Escalation   | Occurs when a user can perform an action or access data of another user with a higher level of permissions.     |
**There are three major types of XSS attacks:**

|                          |                                                                                                                                                                                                                                   |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| DOM (Special)            | DOM XSS _(Document Object Model-based Cross-site Scripting)_ uses the HTML environment to execute malicious javascript. This type of attack commonly uses the _<script></script>_ HTML tag.                                       |
| Persistent (Server-side) | Persistent XSS is javascript that is run when the server loads the page containing it. These can occur when the server does not sanitise the user data when it is **uploaded** to a page. These are commonly found on blog posts. |
| Reflected (Client-side)  | Reflected XSS is javascript that is run on the client-side end of the web application. These are most commonly found when the server doesn't sanitise **search** data.                                                            |
### **What’s Happening in This XSS Attack?**

1. **Injection of Malicious Code:**
    
    - You are inserting this code into a search bar:
        
        ```html
        <iframe src="javascript:alert(`xss`)">
        ```
        
    - The website takes your input and **displays it back** without properly sanitizing it.
2. **Execution of JavaScript:**
    
    - The browser processes the injected **`iframe`** tag.
    - The **`src`** attribute of the iframe points to `javascript:alert('xss')`.
    - This executes the JavaScript `alert('xss')`, showing a pop-up message.
3. **Why It Works (The Vulnerability)**
    
    - The website does not properly **sanitize** or **escape** user input before rendering it in the HTML.
    - Instead of treating your input as plain text, the browser **interprets it as HTML + JavaScript**, leading to execution.
    - Since iframes are widely used in web apps, this form of XSS is sometimes called **Cross-Frame Scripting (XFS)**.

### **How to Prevent This?**

✅ **Escape user input** before inserting it into the webpage.  
✅ Use **Content Security Policy (CSP)** to restrict `javascript:` URLs.  
✅ **Sanitize inputs** to remove harmful tags like `<iframe>` and `<script>`.  
✅ Implement **HTTP-only cookies** to prevent session theft via XSS.
