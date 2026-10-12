1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable and Outdated Components
7. Identification and Authentication Failures
8. Software and Data Integrity Failures
9. Security Logging & Monitoring Failures
10. Server-Side Request Forgery (SSRF)
##  1.Broken Access Control
  

Websites have pages that are protected from regular visitors. For example, only the site's admin user should be able to access a page to manage other users. If a website visitor can access protected pages they are not meant to see, then the access controls are broken.

A regular visitor being able to access protected pages can lead to the following:

- Being able to view sensitive information from other users
- Accessing unauthorized functionality

Simply put, broken access control allows attackers to bypass **authorisation**, allowing them to view sensitive data or perform tasks they aren't supposed to.

#### Broken Access Control (IDOR Challenge)
**IDOR** or **Insecure Direct Object Reference** refers to an access control vulnerability where you can access resources you wouldn't ordinarily be able to see. This occurs when the programmer exposes a Direct Object Reference, which is just an identifier that refers to specific objects within the server. By object, we could mean a file, a user, a bank account in a banking application, or anything really.

## 2.Cryptographic Failures

A **cryptographic failure** refers to any vulnerability arising from the misuse (or lack of use) of cryptographic algorithms for protecting sensitive information. Web applications require cryptography to provide confidentiality for their users at many levels.

- When you are accessing your email account using your browser, you want to be sure that the communications between you and the server are encrypted. That way, any eavesdropper trying to capture your network packets won't be able to recover the content of your email addresses. When we encrypt the network traffic between the client and server, we usually refer to this as **encrypting data in transit**.  
    
- Since your emails are stored in some server managed by your provider, it is also desirable that the email provider can't read their client's emails. To this end, your emails might also be encrypted when stored on the servers. This is referred to as **encrypting data at rest**.

A ==**flat-file database**== is a single file storing data, simpler than a full database server like MySQL. It's often used in small apps but can be risky if stored in a web app's public directory, as anyone could download and access it.

SQLite is a common flat-file database format, easy to query using the **sqlite3** tool on the command line, which is often pre-installed on Linux. If such files are exposed, sensitive data could be at risk!

==**sqlite3**== is a command-line tool for interacting with SQLite databases. It lets you:

1. **Create** databases and tables.
2. **Insert, update, delete, or query** data using SQL commands.
3. **Export or import** data in various formats like `.csv`.
4. **Analyze** and manage database files.

It's lightweight, easy to use, and great for small or embedded database applications.

````
 PRAGMA table_info(customers)
````
The SQL command `PRAGMA table_info(customers)` is used in SQLite to retrieve detailed information about the structure of the table named `customers`.

==Crackstation : website for cracking weak hash passwords.==

## 3. Injection

ChatGPT

Injection flaws happen when applications mistakenly treat user input as commands or code instead of data. Attackers exploit this by crafting inputs that can manipulate how the system behaves.
How It Works:

    User Input: The app takes input from the user (e.g., forms, URLs, or cookies).
    Improper Handling: The app doesn't properly validate or sanitize the input.
    Exploitation: The attacker inserts malicious input (like SQL, commands, or scripts), which the app executes
- **SQL Injection:** This occurs when user-controlled input is passed to SQL queries. As a result, an attacker can pass in SQL queries to manipulate the outcome of such queries. This could potentially allow the attacker to access, modify and delete information in a database when this input is passed into database queries. This would mean an attacker could steal sensitive information such as personal details and credentials.
- **Command Injection:** This occurs when user input is passed to system commands. As a result, an attacker can execute arbitrary system commands on application servers, potentially allowing them to access users' systems.

The main defence for preventing injection attacks is ensuring that user-controlled input is not interpreted as queries or commands. There are different ways of doing this:

- **Using an allow list:** when input is sent to the server, this input is compared to a list of safe inputs or characters. If the input is marked as safe, then it is processed. Otherwise, it is rejected, and the application throws an error.
- **Stripping input:** If the input contains dangerous characters, these are removed before processing.

## 3.1 Command Injection
**Command Injection** happens when a web application improperly allows user input to be passed directly to the server's command line (e.g., PHP executing system commands). An attacker can exploit this by injecting malicious commands, which the server then executes.

For example, they could list files, read them, or run commands as if they were logged into the server. Once in control, the attacker can explore the system further and find ways to escalate their attack or move through the network.

## 4. Insecure Design
**Insecure design** refers to flaws in the **overall structure or architecture** of an application, not just coding mistakes. These vulnerabilities arise when the app's design doesn't properly consider security from the start. For example, bad planning or skipping proper ==**threat modeling** (process of identifying and understanding potential security threats and vulnerabilities in an application or system during the design== ==phase==)during development can lead to security issues. Sometimes, developers might also take shortcuts, like disabling security features (e.g., OTP validation) for easier testing, but forget to turn them back on before the app goes live.

## 5. Security Misconfiguration
**Security misconfigurations** refer to mistakes or oversights in setting up or managing an application, system, or infrastructure, leading to vulnerabilities. Some common examples include:

1. **Poorly configured cloud service permissions** (e.g., unsecured S3 buckets) allow unauthorized access to sensitive data.
2. **Unnecessary features enabled** (e.g., unused services, accounts, or privileges) increase the attack surface.
3. **Default accounts with unchanged passwords** make it easier for attackers to guess or crack the credentials.
4. **Detailed error messages** give attackers more information than needed, potentially revealing system weaknesses.
5. **Not using HTTP security headers** leaves the application vulnerable to common attacks like XSS or clickjacking.

## Debugging Interfaces

A common security misconfiguration concerns the exposure of debugging features in production software. Debugging features are often available in programming frameworks to allow the developers to access advanced functionality that is useful for debugging an application while it's being developed. Attackers could abuse some of those debug functionalities if somehow, the developers forgot to disable them before publishing their applications.
## 6. Vulnerable and Outdated Components
**Vulnerable and outdated components** refer to software or tools that have known security flaws because they haven’t been updated with the latest patches or versions. This is a major risk in cybersecurity, as attackers can exploit these flaws using publicly available tools or exploits.

For example, if a company is using an old version of **WordPress** (like 4.6), which has a known **remote code execution (RCE)** vulnerability, attackers can easily exploit it.


## 7. Identification and Authentication Failures

Authentication and session management ensure secure user access to web applications.
1. **How Authentication Works**:
    - A user logs in with a **username and password**.
    - The server verifies the credentials and sends a **session cookie** to the user's browser.
    - The session cookie identifies the user because HTTP(S) is stateless.(meaning it doesn't retain information about users or their interactions between requests)
2. **Common Vulnerabilities**:
    
    - **Brute Force Attacks**: Attackers repeatedly try passwords to guess the correct one.
    - **Weak Passwords**: Simple passwords like "password1" are easy to guess.
    - **Weak Session Cookies**: If session cookies are predictable, attackers can forge them to impersonate users.
3. **Mitigations**:
    
    - Enforce **strong password policies** to prevent weak passwords.
    - Implement **account lockout** after too many failed attempts to stop brute force attacks.
    - Use **Multi-Factor Authentication (MFA)** to add extra security by requiring a second verification step, like a code sent to the user's phone.

## 8. Software and Data Integrity Failures

**Data/Information Integrity**: Ensuring data is accurate, consistent, and unaltered.
**1. Software Integrity Failures**
**Software integrity failure** occurs when third-party libraries(e,g,) used in your website are altered without checks. For example, if your website loads jQuery directly from an external server:

```html
<script src="https://code.jquery.com/jquery-3.6.1.min.js"></script>
```

If an attacker hacks the jQuery server and injects malicious code, every visitor to your website will unknowingly execute the malicious code.

### Solution: Subresource Integrity (SRI)

SRI ensures the library's code is safe by verifying its **hash**. If the hash doesn't match the expected value, the browser won't run the code.

**Secure Example**:

```html
<script src="https://code.jquery.com/jquery-3.6.1.min.js" 
        integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" 
        crossorigin="anonymous"></script>
```

==You can go to [https://www.srihash.org/](https://www.srihash.org/) to generate hashes for any library if needed.==

## 2. Data Integrity Failures

**Data Integrity Failures** happen when a system trusts data that can be tampered with by users or attackers, leading to potential security issues. Here's a more detailed explanation :

### **How Session Management Relates to Data Integrity**

- When a user logs into a web application, the server assigns a **session token** to identify them.
- This token is often stored in a **cookie** on the user's browser and sent with each request to the server.
- If the session token contains sensitive data (e.g., a **username**) without protection, users could **modify the cookie** to impersonate another user.

**Solution**
### JSON Web Tokens (JWT)

**JWT** ensures data integrity with three parts:

1. **Header**: Metadata (e.g., algorithm info).
2. **Payload**: Stores key-value pairs (e.g., username).
3. **Signature**: Verifies data integrity using a **server-held secret key**.

If a user tampers with the payload, the signature won’t match, and the server will reject it. This prevents unauthorized modifications.
![[Pasted image 20250122173230.png]]

JWT and the None Algorithm

A data integrity failure vulnerability was present on some libraries implementing JWTs a while ago. As we have seen, JWT implements a signature to validate the integrity of the payload data. The vulnerable libraries allowed attackers to bypass the signature validation by changing the two following things in a JWT:

1. Modify the header section of the token so that the `alg` header would contain the value `none`.
2. Remove the signature part.

Taking the JWT from before as an example, if we wanted to change the payload so that the username becomes "admin" and no signature check is done, we would have to decode the header and payload, modify them as needed, and encode them back. Notice how we removed the signature part but kept the dot at the end.
![[Pasted image 20250122192243.png]]
## 9. Security Logging and Monitoring Failures
Logging in web applications is essential to track user actions and detect potential security breaches. By logging important information such as HTTP status codes, timestamps, usernames, API endpoints, and IP addresses, administrators can trace the actions of attackers.

Key points:

- **Why logging matters**:
    
    - Helps trace attacker actions in case of an incident.
    - Assists in determining the risk and impact of a breach.
    - Prevents regulatory issues (like fines) if sensitive data is compromised.
- **Suspicious activity**:
    
    - Multiple failed login attempts.
    - Requests from unusual IP addresses or locations.
    - Use of automated tools or common attack payloads.
- **Importance of monitoring**:
    
    - Suspicious activities should be monitored to stop or reduce an attack's impact.
    - High-impact suspicious actions should raise alarms to ensure a prompt response.

In short, logging is critical for incident detection and response, ensuring the security of the application and compliance with regulations.

## 10. Server -Side Request Forgery(SSRF)
Server Side Request Forgery (SSRF) is a web vulnerability where an attacker manipulates a vulnerable application to make requests to internal or external resources on behalf of the server. This can lead to data exposure, unauthorised access to internal systems, or service disruptions.

SSRF can be used for:  

- Enumerate internal networks, including IP addresses and ports.
- Abuse trust relationships between servers and gain access to otherwise restricted services.
- Interact with some non-HTTP services to get remote code execution (RCE).


nc -lnvp [PORT]
- **`-l`** → Listen mode (waits for connections).
- **`-n`** → No DNS resolution (speeds up execution).
- **`-v`** → Verbose mode (shows detailed output).
- **`-p [PORT]`** → Specifies the port to listen on.