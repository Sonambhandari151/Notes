<html>

    <head>

<title>Page Title</title>

    </head>

    <body>

<h1>A Heading</h1>

<p>A Paragraph</p>

    </body>

</html>
## URL Encoding

- Browsers use **ASCII encoding** in URLs
    
- Only **alphanumeric** and a few **special characters** are allowed
    
- **Unsafe characters** (like spaces, symbols, non-English text) must be **encoded**
    
### 🔁 How it works:

- Replaces unsafe characters with:  
    `%` + **2-digit hex code** of the character's ASCII value
    
### ✅ Purpose:

- Ensures URLs are **safe and readable** by browsers
    
- Helps avoid **misinterpretation** of special characters
    
EG
|Character|Encoding|
|---|---|
|space|%20|
|!|%21|
|"|%22|
|#|%23|
|$|%24|
|%|%25|
|&|%26|
|'|%27|
|(|%28|
|)|%29|

---
### 🧠 **What’s in the `<head>` and `<body>`?**

- The `<head>` contains things **not shown directly** on the page like:
    
    - `<title>` (page title in browser tab)
        
    - `<style>` (CSS for page design)
        
    - `<script>` (JavaScript for page behavior)
        
- The `<body>` contains the **main content** that you see on the web page (text, images, buttons, etc.)
    
Each of these elements is called a [DOM (Document Object Model)](https://en.wikipedia.org/wiki/Document_Object_Model). The [World Wide Web Consortium (W3C)](https://www.w3.org) defines `DOM` as:

`"The W3C Document Object Model (DOM) is a platform and language-neutral interface that allows programs and scripts to dynamically access and update the content, structure, and style of a document."`

### 🔧 3 Types of DOM:

1. **Core DOM** – Works for all kinds of documents
    
2. **XML DOM** – For XML files
    
3. **HTML DOM** – For HTML web pages
    

---

### 🛠️ How we use it:

- In JavaScript, you can target elements like:  
    `document.head` → points to the `<head>`  
    `document.h1` → points to the first `<h1>` tag
    
- You can also find elements by:
    
    - **ID** → `document.getElementById("my-id")`
        
    - **Tag** → `getElementsByTagName("p")`
        
    - **Class** → `getElementsByClassName("info")`
        

---

### 🔐 Why it matters for security (like XSS):

- Knowing the DOM helps you:
    
    - Understand **how the page is built**
        
    - Find **vulnerable places** to inject code (like XSS)
        
    - Manipulate or create new elements using JavaScript
        
----
