## Documentation

- Use Burp Scanner to crawl the API.
- Browse applications manually using Burp's browser.
- Look for endpoints that may refer to API Documentation. 
EG        
   - /api
   - /swagger/index.html
   - /openai.json
- If you identify an endpoint,  Also investigate  the base path.
 Eg: 
     Endpoint : ==/api/swagger/v1/users/123==
     ==Investigate :==
     
    - /api/swagger/v1
    - /api/swagger
    - /api
- Can use list of Common paths to find documentation using intruder.

## Using Machine-readable documentation

1. **Burp Scanner can crawl and audit OpenAPI documentation:**
    
    - **Crawl:** It can automatically follow and explore the endpoints listed in your OpenAPI documentation.
        
    - **Audit:** It can test those endpoints for **vulnerabilities** like:
        
        - Broken authentication
            
        - Injection flaws
            
        - BOLA (Broken Object Level Authorization)
            
        - Other OWASP API Top 10 issues
            
    
    > 📘 OpenAPI documentation is a standardized format (JSON or YAML) used to describe RESTful APIs (like their routes, parameters, expected responses, etc.).
    > 
**2.You can use the OpenAPI Parser BApp:**
-  This is an **extension/plugin** (called a BApp) available in **Burp Suite’s BApp Store**.
    
 - It helps **import** OpenAPI definitions into Burp Suite.
    
- Once imported, Burp knows all the endpoints, parameters, and methods defined in the API, making testing easier and more complete.

3. use a specialized tool to test the documented endpoints, such as Postman or SoapUI.

---

### Example Flow:

1. You have an `openapi.json` or `openapi.yaml` file describing your API.
    
2. You load it into Burp Suite using:
    
    - The **OpenAPI Parser** extension.
        
3. Burp will:
    
    - Discover endpoints automatically.
        
    - Send requests to them.
        
    - **Scan for vulnerabilities** using Burp Scanner.

---
## Identifying API endpoints

Even with OpenAPI docs, always explore the application manuallydocs may be outdated or incomplete.

### 🧭 Key Steps:

1. **Use Burp's Browser**
    
    - Browse the app to capture API traffic.
        
2. **Run Burp Scanner**
    
    - Crawl the app to auto-discover endpoints and parameters.
        
3. **Look for API Clues**
    
    - Watch for URL patterns like `/api/`, `/v1/`, `/graphql`.
        
4. **Review JavaScript Files**
    
    - They often contain hidden API endpoints.
        
5. **Use JS Link Finder BApp**
    
    - Extract URLs from JS files Burp might miss.
        
6. **Manual Analysis**
    
    - Check for `fetch`, `axios`, or `XHR` in JS; note new endpoints.
        

---
## Interacting with API endpoints
- Interact with endpoints using **Burp Repeater** and **Burp Intruder.**  
- You can observe API's behaviour and discover additional attack surface.
    - Eg. you could investigate how the API responds to changing the HTTP method and media type.
- As you interact with API endpoints, review error messages and other responses closely. These may contain info to construct valid HTTP requests.

---

## HTTP Methods

1. `GET` -  Retrieves data from a resource.
2. `PATCH` - Applies partial changes to a resource.
3. `OPTIONS` - Retrieves information about which **HTTP methods** (e.g., GET, POST, PUT) are allowed on a specific resource.

#### Test All HTTP Methods on Endpoints

API endpoints can support **multiple methods** (GET, POST, DELETE, etc.), each revealing different functionality and **attack surfaces**.

#### 📌 Example:

```
GET    /api/tasks      → Get tasks  
POST   /api/tasks      → Create task  
DELETE /api/tasks/1    → Delete task
```

---

### 🧪 How to Test:

- Use **Burp Intruder’s built-in HTTP verbs list** to cycle through methods.
    
- This can help uncover **undocumented or unintended behavior**.
    

---

### Note:

Always test on **low-priority or test objects** to avoid:

- Deleting real data
    
- Creating excessive records
    
- Disrupting the system
---

### 🔍 Identifying Supported Content Types

API endpoints may behave differently based on the `Content-Type` of a request. Testing different types can help you:

- Trigger errors that leak useful information
    
- Bypass weak security controls
    
- Exploit inconsistent parsing logic
    
    > e.g., JSON is secure, but XML allows injection(EG)
    
### 🔧 How to Test:

- Change the `Content-Type` header (e.g., from `application/json` to `application/xml`)
    
- Reformat the request body to match the new type
    
- Use the **Content Type Converter** BApp to auto-convert between JSON ↔ XML
    

---

## 📌 **Using Intruder to Discover Hidden API Endpoints**

### 🔍 **Initial Recon**

Once you identify a known endpoint like:

```
PUT /api/user/update
```

This gives you a **path structure** to work with:

```
/api/user/[FUNCTION]
```

---


Find other API functionalities like:

- `/api/user/delete`
    
- `/api/user/add`
    
- `/api/user/view`
    

---

### 🛠️ **Step-by-Step Process in Burp Intruder:**

1. **Send the request to Intruder:**
    
2. **Configure the attack type:**
    
    - Use **"Sniper"** if replacing one parameter.
        
    - Use **"Cluster Bomb"** or **"Pitchfork"** if multiple values are tested in sequence.
        
    - For this case, **"Sniper"** is enough.
        
3. **Mark the endpoint segment as payload:**
    
    - Highlight `/update` and click **“Add §”**.
        
    - It becomes `/api/user/§update§`
        
4. **Load a wordlist of common API actions:**
    
    - Use built-in wordlists in Burp or from:
        
        - SecLists: `APIs/common-endpoints.txt`, `Fuzzing/api-words.txt`
            
        - Custom wordlists based on app context
            
5. **Run the attack.**
    

---

##Sure! Here are **short, clear notes** based on the content you shared:

---

###  **Finding Hidden Parameters**

####  Burp Intruder:

- Add or replace parameters in requests using a parameter name wordlist (e.g., `admin`, `debug`, `test`).
    
- Look for changes in response behavior.
    

####  Param Miner (Burp Extension):

- Automatically guesses up to 65,536 parameters per request.
    
- Uses common and context-specific parameter names.
    
- Very effective for finding debug or admin parameters.
    

####  Content Discovery Tool:

- Finds unlinked files, directories, and URLs that may include hidden parameters.
    

---

Use parameter names and endpoint words relevant to the application, based on recon (e.g., from JavaScript files, error messages, etc.).

---
##  **Mass Assignment (Auto-binding) ** Vulnerability

- Occurs when a web app **automatically maps user input** to backend object fields.
    
- Can allow attackers to modify **sensitive fields** (e.g., `isAdmin`, `role`, `user_id`).
    
- Caused by developers **not filtering input** during updates.
    

### ⚠️ **Example**

```json
{
  "name": "Bob",
  "isAdmin": true
}
```

If `isAdmin` is not blocked, the attacker becomes admin.

### 🧪 **How to Test**

- Send extra parameters like:
    
    - `"role": "admin"`
        
    - `"balance": 9999`
        
- If accepted — vulnerable to mass assignment.
    

### ✅ **Prevention**

- Use **whitelisting** (only allow certain fields).
    
- Don’t directly bind all user input to backend objects.
    

---
## Identifying hidden parameters

Since mass assignment creates parameters from object fields, you can often identify these hidden parameters by manually examining objects returned by the API.

For example, consider a `PATCH /api/users/` request, which enables users to update their username and email, and includes the following JSON:

`{ "username": "wiener", "email": "wiener@example.com", }`

A concurrent `GET /api/users/123` request returns the following JSON:

`{ "id": 123, "name": "John Doe", "email": "john@example.com", "isAdmin": "false" }`

This may indicate that the hidden `id` and `isAdmin` parameters are bound to the internal user object, alongside the updated username and email parameters.


---

##  **How to Test for Mass Assignment Vulnerabilities 

Check if the backend allows you to change **sensitive fields** (like `isAdmin`) that are not meant to be controlled by users.

---

### 🧪 **Step 1: Test with a valid false value**

Send this `PATCH` request:

```json
{
  "username": "wiener",
  "email": "wiener@example.com",
  "isAdmin": false
}
```

- **Purpose:** Check if the server accepts the `isAdmin` field at all.
    
- If the server processes this, it suggests the field is being **accepted** or **validated**.
    

---

### 🧪 **Step 2: Send invalid value**

```json
{
  "username": "wiener",
  "email": "wiener@example.com",
  "isAdmin": "foo"
}
```

- **Purpose:** Check how the server handles an **unexpected value**.
    
- If this causes an error or changes the response, it means the field is being **used** in logic — even if improperly.
    

---

### 🧪 **Step 3: Attempt to exploit**

```json
{
  "username": "wiener",
  "email": "wiener@example.com",
  "isAdmin": true
}
```

- **Purpose:** Try to exploit the mass assignment vulnerability.
    
- If `isAdmin` is not filtered out and gets saved, you may now be an **admin**.
    

---

### 🔍 **Step 4: Verify if you're an admin**

- Log in or refresh the application as the user `wiener`.
    
- Check if you can access **admin pages**, **user lists**, or any privileged functionality.
    

---

## ✅ Conclusion

If adding `isAdmin: true` works and you become admin, the application is **vulnerable to mass assignment**. This means user input is being bound to backend objects **without proper filtering**.

---
## Preventing vulnerabilities in APIs

When designing APIs, make sure that security is a consideration from the beginning. In particular, make sure that you:

- Secure your documentation if you don't intend your API to be publicly accessible.
- Ensure your documentation is kept up to date so that legitimate testers have full visibility of the API's attack surface.
- Apply an allowlist of permitted HTTP methods.
- Validate that the content type is expected for each request or response.
- Use generic error messages to avoid giving away information that may be useful for an attacker.
- Use protective measures on all versions of your API, not just the current production version.

To prevent mass assignment vulnerabilities, allowlist the properties that can be updated by the user, and blocklist sensitive properties that shouldn't be updated by the user.

---

##  **Server-Side Parameter Pollution (SSPP)**

### 📘 **Definition:**

Server-side parameter pollution happens when **user input is inserted into a server-side request** (e.g., to an internal API) **without proper encoding or validation**, allowing the attacker to **inject extra parameters**.

---

### **What Can Go Wrong?**

- Override existing parameters
    
-  Modify application behavior
    
-  Access unauthorized data
    

---

### 🧪 **Example:**

Frontend request sent by the user:

```
GET /product?category=toys
```

If the server-side code forwards it to an internal API like this:

```
GET http://internal-api/products?category=toys
```

But the attacker sends:

```
GET /product?category=toys&category=admin
```

The server might build:

```
GET http://internal-api/products?category=toys&category=admin
```

Result: The internal API might pick **`category=admin`**, giving access to admin-only data.

---

### ✅ **Where to Test for SSPP:**

- Query parameters (e.g., `?id=1&id=2`)
    
- Form fields
    
- HTTP headers (like `X-Forwarded-For`)
    
- URL path variables (e.g., `/user/x/profile`)
    

---
## 🔹 **Truncating Query Strings (%23 attack)**

- `%23` is the URL-encoded version of `#`.
    
- Used to **cut off the rest of a URL** in **internal server-side requests**.
### 🧪 **How It Works:**

1. You send:
    
    ```
    /userSearch?name=peter%23abc
    ```
    
2. Backend builds:
    
    ```
    /users/search?name=peter#abc&publicProfile=true
    ```
    
3. The `#` **truncates** the rest:
    
    ```
    /users/search?name=peter
    ```

### ✅ **Why It's Useful:**

- Can **bypass security filters** like `publicProfile=true`.
    
- May allow access to **private data**.
    

### 🔍 **How to Test:**

- Add `%23test` to a parameter.
    
- If the app behaves differently, truncation likely worked.
    
---
## **Injecting Invalid Parameters**

### **How It Works:**

- You send:
    
    ```
    /userSearch?name=peter%26foo=xyz&back=/home
    ```
    
- Backend may decode `%26` to `&`, resulting in:
    
    ```
    /users/search?name=peter&foo=xyz&publicProfile=true
    ```
    

---

### **Why Do This?**

- To **inject unexpected parameters** into the server’s internal request.
    
- Useful for testing **how extra parameters are handled**, especially in **internal APIs**.
    

---

### 🔍 **What to Look For:**

- If the **response changes**, your parameter was likely used.
    
- If the **response is the same**, it may have been ignored — but still processed internally.
    
- If the app errors, it may reveal **parsing issues**.
    

---
Here’s a **simple explanation** of **injecting valid parameters** using the `%26` (URL-encoded `&`) technique, based on what you shared:

---

## 🔍 **Injecting Valid Parameters – Explained**


modifying the query string in a **front-end request** to **inject a valid parameter** (like `email=foo`) into the **server’s internal request**, by using `%26` (which stands for `&`).
### 🧪 **Example:**

You send:

```
GET /userSearch?name=peter%26email=foo&back=/home
```

If the application doesn’t re-encode `%26`, the internal request becomes:

```
GET /users/search?name=peter&email=foo&publicProfile=true
```

 **injected a valid parameter**: `email=foo`


### 🔍 **How to Analyze the Response:**

After sending your payload, look for:

| Behavior                    | What It Might Mean                            |
| --------------------------- | --------------------------------------------- |
| Different user result shown | Parameter was accepted and processed          |
| Same result as before       | Parameter may be ignored or sanitized         |
| Error shown                 | App tried to use parameter but failed parsing |

---

## GOAL

- To **inject known/valid parameters** into a backend request
    
- To test whether the server **accepts and acts on them**
    
- To find potential **authorization flaws** or **data leaks**
    

---

Here are **short and clear notes** with explanation on **Overriding Existing Parameters (Server-Side Parameter Pollution):**

---
## 🔹 **Overriding Existing Parameters – Notes**

Overriding parameters means injecting **a second parameter with the same name** to see how the **backend interprets duplicates** in a server-side request.

### 🧪 **Example:**

You send:

```
GET /userSearch?name=peter%26name=carlos&back=/home
```

Server may decode this to:

```
GET /users/search?name=peter&name=carlos&publicProfile=true
```

---

###  **What Happens Depends on Server Tech:**

| Backend (Language)    | Behavior                                   |
| --------------------- | ------------------------------------------ |
| **PHP**               | Uses the **last** parameter → `carlos`     |
| **Node.js (Express)** | Uses the **first** → `peter`               |
| **ASP.NET**           | Combines both → `peter,carlos` (may error) |

---

### ✅ **Why It's Useful:**

- May allow you to **override trusted parameters** (like `user`, `role`, `email`).
    
- Could help **bypass security filters**.
    
- Might let you **impersonate users** (e.g., inject `name=administrator`).
    
---
## EXample:  Exploiting server-side parameter pollution in a query string

- To solve the lab, log in as the `administrator` and delete `carlos`.
SOLUTION: 

1. In Burp's browser, trigger a password reset for the `administrator` user.
    
2. In **Proxy > HTTP history**, notice the `POST /forgot-password` request and the related `/static/js/forgotPassword.js` JavaScript file.
    
3. Right-click the `POST /forgot-password` request and select **Send to Repeater**.
    
4. In the **Repeater** tab, resend the request to confirm that the response is consistent.
    
5. Change the value of the `username` parameter from `administrator` to an invalid username, such as `administratorx`. Send the request. Notice that this results in an `Invalid username` error message.
    
6. Attempt to add a second parameter-value pair to the server-side request using a URL-encoded `&` character. For example, add URL-encoded `&x=y`:
    
    `username=administrator%26x=y`
    
    Send the request. Notice that this returns a `Parameter is not supported` error message. This suggests that the internal API may have interpreted `&x=y` as a separate parameter, instead of part of the username.
    
7. Attempt to truncate the server-side query string using a URL-encoded `#` character:
    
    `username=administrator%23`
    
    Send the request. Notice that this returns a `Field not specified` error message. This suggests that the server-side query may include an additional parameter called `field`, which has been removed by the `#` character.
    
8. Add a `field` parameter with an invalid value to the request. Truncate the query string after the added parameter-value pair. For example, add URL-encoded `&field=x#`:
    
    `username=administrator%26field=x%23`
    
    Send the request. Notice that this results in an `Invalid field` error message. This suggests that the server-side application may recognize the injected field parameter.
    
9. Brute-force the value of the `field` parameter:
    
    1. Right-click the `POST /forgot-password` request and select **Send to Intruder**.
    2. In the **Intruder** tab, add a payload position to the value of the `field` parameter as follows:
        
        `username=administrator%26field=§x§%23`
        
    3. In the **Payloads** side panel, under **Payload configuration**, click **Add from list**. Select the built-in **Server-side variable names** payload list, then start the attack.
    4. Review the results. Notice that the requests with the username and email payloads both return a `200` response.
10. Change the value of the `field` parameter from `x#` to `email`:
    
    `username=administrator%26field=email%23`
    
    Send the request. Notice that this returns the original response. This suggests that `email` is a valid field type.
    
11. In **Proxy > HTTP history**, review the `/static/js/forgotPassword.js` JavaScript file. Notice the password reset endpoint, which refers to the `reset_token` parameter:
    
    `/forgot-password?reset_token=${resetToken}`
    
12. In the **Repeater** tab, change the value of the `field` parameter from `email` to `reset_token`:
    
    `username=administrator%26field=reset_token%23`
    
    Send the request. Notice that this returns a password reset token. Make a note of this.
    
13. In Burp's browser, enter the password reset endpoint in the address bar. Add your password reset token as the value of the `reset_token` parameter . For example:
    
    `/forgot-password?reset_token=123456789`
    
14. Set a new password.
    
15. Log in as the `administrator` user using your password.
    
16. Go to the **Admin panel** and delete `carlos` to solve the lab.
---
## 🔹 **REST Path Parameter Pollution**

### 📘 **What it is:**

Injecting path traversal sequences into RESTful API paths to manipulate which resource is accessed.

### 🧪 **Example Attack:**

You send:

```
/edit_profile.php?name=peter%2f..%2fadmin
```

Backend becomes:

```
/api/private/users/peter/../admin
```

Which may resolve to:

```
/api/private/users/admin
```

---

### ✅ **Why it's Dangerous:**

- Can **bypass access control**.
    
- May allow access to **other users' data** (e.g., admin).
    
- Happens when the server **normalizes `../`** in paths.
    

---

### 🔍 **What to Look For:**

- You see another user’s data.
    
- Path resolves differently.
    
- No proper input validation.
    

---
## 🔹 **Structured Data Parameter Pollution**

Injecting structured data (like JSON or XML) into user input to **modify how the server builds internal API requests**.

---

### 🧪 **Example Scenario:**

You send:

```
POST /myaccount  
name=peter","access_level":"administrator
```

Server creates:

```json
{"name":"peter","access_level":"administrator"}
```

---

### ✅ **Why It’s Dangerous:**

- If the server **blindly injects user input into JSON**, attackers can add new fields (e.g., `access_level`).
    
- May lead to **privilege escalation**, unauthorized access, or data tampering.
    

---

### 🔍 **What to Look For:**

- Unexpected changes in behavior (e.g., user becomes admin).
    
- Server accepts new fields or values.
    
- API response or app features change after injection.
    

---

### 🎯 **Goal:**

- Test if the server uses user input to **build structured requests** without proper sanitization.
    
- Inject valid keys/values to **override or add internal parameters**.
    
---

## 🔹 **Testing for Server-Side Parameter Pollution in Structured Data Formats (JSON) – Continued**

Injecting crafted data into JSON to exploit how the server **builds internal API requests**. This works if the server **blindly inserts** user input into structured formats like JSON or XML.

### 🧪 **Example Attack:**

#### ✅ Normal Request:

```http
POST /myaccount
{"name": "peter"}
```

→ Becomes:

```http
PATCH /users/7312/update
{"name":"peter"}
```

---

#### 🔥 Injected Request:

```http
POST /myaccount
{"name": "peter\",\"access_level\":\"administrator"}
```

→ Server-side JSON becomes:

```json
{"name":"peter","access_level":"administrator"}
```

➡️ User `peter` may now gain **admin privileges**.

---

### ⚠️ **Why It Happens:**

- Server fails to **escape or sanitize** user input before placing it inside another JSON/XML structure.
    
- This leads to **injection of new keys or values** into structured data.
    

---

### 🔍 **Structured Format Injection in Responses:**

- If the server **includes unescaped user input** in a JSON response:
    
    ```json
    {"message": "Welcome, peter\",\"isAdmin\":true"}
    ```
    
- The injected data may be **interpreted by the frontend**, causing incorrect behavior or security risks.
    

---

### 🧠 **Can Happen In:**

- **Requests**: API calls, form submissions, PATCH/PUT requests.
    
- **Responses**: API responses with dynamic content.
    

---

### 📌 **Also Applies To:**

- JSON
    
- XML
    
- YAML
    
- HTML (template injections)
    

---

## 🔹 **Automated Tools for SSPP Testing **

### 🔧 **1. Burp Scanner**

- **Automatically detects suspicious input transformations** during auditing.
    
- Flags cases where input is **modified before processing**.
    
- Doesn't confirm a vulnerability — **requires manual testing**.
    
- Refer to: **Suspicious input transformation** issue.
    

---

### 🚀 **2. Backslash Powered Scanner (BApp)**

- Extension for Burp Suite.
    
- Sends **fuzzed payloads with backslashes and special chars**.
    
- **Classifies input** as:
    
    - **Boring** – no effect
        
    - **Interesting** – might lead to unexpected behavior
        
    - **Vulnerable** – confirmed risky input behavior
        
- Helps detect **unknown injection vulnerabilities**.
    
- Refer to: _Backslash Powered Scanning_ whitepaper.
    

---

### 🎯 **Why Use These Tools:**

- Automates part of the **discovery phase**.
    
- Finds **unusual input handling**.
    
- Flags **potential SSPP and injection points** for manual follow-up.
    

---
## 🔒 **Preventing Server-Side Parameter Pollution – Notes**

###  **1. Use an Allowlist**

- Define which characters are **safe and allowed**.
    
- Reject or encode all **unexpected/special characters** (like `&`, `=`, `#`, `/`, `\`).
    
### 🧼 **2. Encode User Input**

- Always **URL-encode or escape** user input before including it in:
    
    - API paths
        
    - Query strings
        
    - JSON/XML structures
        
## 🧪 **3. Validate Format and Structure**

- Enforce strict input formats (e.g., regex for usernames, emails).
    
- Reject input that breaks **expected schema** or structure.
    

---

### 🔍 **4. Sanitize at Every Layer**

- Don’t trust client-side validation alone.
    
- **Validate on the server** before using input in internal requests.
    

---
