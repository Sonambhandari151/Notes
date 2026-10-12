
## **Authentication**

`Authentication` is verifying the identity of a user or client.
    - HTML form-based authentication
    - Multi-factor Mechanisms
    - Windows-integrated authentication using NTLM or Kerberos.

#### **3FA**
1. Something yoy `know`. Eg : Password or security question.
    (*Knowledge factors*).
2. Somwthing you `have`. Eg: Mobile phone or security token. (*Possession factors*)
3. Something you `are` or do. Eg: Biometrics. (*Inherence factors*)

---

##  **`Authentication vulnerabilities`**
Arise from insecure implementation of the authentication mechinsms in an application.

### 1. Weak password Requirements
- Having no or minimal control over the quality f users passwords.

		- very short or blank
		- Common dictionary words or names
		- Passwors same as username
		- Missing or ineffective MFA
		- Use of default Password.

### 2. Improper Restriction of Authentication attempts

Application permits brute ofrce or other automated attacks.

- login page
- OTP/MFA page
- Change password page.

### 3.  Verbose Error Message

The application outputs a verbose error message that allows for username enumeration.

Eg : `Sign in failed. Incorrect username.`
    `Sign in failed. Incorrect password.`


### 4. Vulnerable Transmission of Credentials

The application uese an unencrypted  HTTP connection to transmit login credentials.
- We can see sensitive data on wireshark.

### 5. Insecure Forgot Password Functionality

Design weakness in the forgotten password functionality usually make the weakest link that can be used to attack the application's overall authentication logic.
 Eg : `secret questions`

### 6. Defects in Multistage Login Mechanism

Insecure Implementation of the MFA function.


##### REQ1 
```
POST /login-step/1st HTTP/1.1
Host : Vulnerable-website.com
...
username = carlos&password=qwerty
```

###### REQ 2
```
POST /login-steps/2nd HTTP/1.1
Host: vuln-website.com 
Cookie: account = carlos
...
Verification-code =123456
```

**How can this be exploited?**
 Change the "account" cookie to hte victim's username and compromise the victim's account.

###### 7. Insecure Storage of Credentials
Uses plain text, encrypted , or weekly hashes password data stores.

![[Pasted image 20250723122454.png]]

---

## **Impact of Authentication Vulnerabilities**
- Unauthorised access to the application.
      - `Confidentiality` : Access to other users data.
      - `Integrity`: Access to update other users data.
      - `Availability`: Access to delete users and their data.
- Can sometimes be chained with other vulnerabilties to gain remote code execution on the host OS.


---
## **How to find and exploit authentication flaws.**

### 1. Weak pass Complexity Requirements

![[Pasted image 20250723123017.png]]

## 2. Improper Restriction of Authentication Attempts

- Manually submit several bad login attempts for an account you control.
- After 10 login attempts, if the application doesn't return a meassge about acc lockout, attempt to login correctly.
     - Run brute force. Tools: hudra, burp Intruder .etc.
 - If account is locked out, monitor the request and responses to determine if hte lockout mechanisms is insecure.
- Apply this test on all auth pages.


### 3. Verbose Error Message

- Submit a request with a valid username and an invalid password.
    
- Submit a request with an invalid username.
    
- Review both responses for any differences in the status code, any redirects, information displayed on the screen, HTML page source, or even the time to process the request.
    
- If there is a difference, run a brute force attack to enumerate the list of valid usernames in the application.
    

 **NOTE:** Apply this test on all authentication pages.

### 4.Vulnerable Transmission of Credentials

- **Perform a successful login while monitoring all traffic** in both directions between the client and server.
    
- **Look for instances** where credentials are submitted in a URL query string or as a cookie, or are transmitted back from the server to the client.
    
- **Attempt to access the application over HTTP** and check if there are any redirections to HTTPS.



### 5.Insecure Forgot Password Functionality

- Identify if the application has a **forgot password functionality**.
    
- If yes, perform a complete walkthrough of the forgot password functional flow ensuring that you have captured all traffic between the application and your browser.
    
- Review the functionality to determine if it discloses **username enumeration** or brute-force indicators.
    
- If the application generates a reset token using UUID, also determine if there is the potential to identify any predictable patterns or sensitive information included in the UUID, that leaks the structure, length, format, and value range.
    

---

### 6.Defects in Multistage Login Mechanism

- Identify if the application uses a **multistage login mechanism**.
    
- If so, perform a complete walkthrough using **invalid usernames** to observe errors at different points in the login/authentication process.
    
- Review the functionality to determine if it allows **username enumeration** or brute-force indicators.
    

---

### 7.Insecure Storage of Credentials

- Review all the application's documentation and functionality to identify if **storing credentials** in the server’s session or transmitting to the client happens anywhere.
    
- Review how/where the application is **storing passwords** (e.g., plaintext, weak hash, or encrypted), and whether **passwords are exposed** in logs or responses.
    
- Identify if the **password hash is stored in the server** versus the database to determine if the passwords are stored insecurely.
    
- Consider online database leaks or file disclosures to review how passwords are stored in the backend database.
    

---
session=7lpciQ44QxHWU3A9xAkzN3uoSa87wLCl

d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw