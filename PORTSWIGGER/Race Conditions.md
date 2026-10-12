
**Definition:**  
A race condition occurs when two or more processes (or threads) access a shared resource (like a database or file) concurrently, and the final outcome depends on the timing of these accesses.

**Why it’s a logic flaw:**  
The application behaves unexpectedly **not because of broken code**, but due to how logic is implemented around **timing and sequencing**. The code does what it's supposed to—but **in the wrong order**, or **in parallel** without proper control.

---

###  **Multiple Threads Interacting with the Same Data**

**What happens:**

- Imagine a web application that allows a user to **redeem a gift card**.
    
- If two requests are sent at the **exact same time**:
    
    - Both may get past the **validation check**.
        
    - Both may **reduce the gift card balance**, even though it should only be used once.
        
- This is a **"collision"**—unintended and unsafe behavior due to concurrency.
    

---

###  **Exploitation via Timed Requests**

**Race Condition Attack:**

- An attacker **automates and times** multiple requests **very precisely** (using tools like Burp Suite Intruder or Turbo Intruder).
    
- The goal is to **trick the app into processing multiple actions** before the system realizes what’s happening.
    
- Example: Redeeming the **same coupon code** multiple times to get free items.
    

---

### **The “Race Window”**

**What it means:**

- A small **timeframe** when the shared resource is in a **vulnerable state**.
    
- For example:
    
    - The time between a **validation** (e.g., “is gift card unused?”) and the **update** (e.g., “mark as used”).
        
    - If two requests hit within this tiny window, both might succeed.
        

---

###  **Impact Depends on the Functionality**

**Context is key:**

- In an **e-commerce site**, it could be abusing payment systems.
    
- In a **banking app**, it might be double-transferring funds.
    
- So, the **risk level** depends on **where** the race condition is found.
    


###  **What is a Limit Overrun Race Condition?**

A **limit overrun** occurs when an application imposes a **limit** (e.g., one-time use), but **fails to enforce it properly under concurrent access**.  
This is the **most well-known form** of race condition.

---

###  **Typical Discount Code Workflow**

Let’s say a user applies a discount code. The backend process typically follows this logic:

1. ✅ **Check**: Has this code been used by the user?
    
2. 💸 **Apply**: If not, apply the discount.
    
3. 📝 **Update**: Mark the code as used in the database.
    

---

###  **Where the Race Window Appears**

- Between steps **1 and 3**, there's a **temporary vulnerable state** (race window).
    
- If two requests are sent **almost simultaneously**:
    
    - Both may pass the **check** before the **update** happens.
        
    - Result: The discount is **applied multiple times**.
        

---

###  **Example Attack Scenario**

- A user sends two requests **at the same time** with the same valid coupon.
    
- Both requests pass the “check” stage.
    
- Both get the discount.
    
- But the system only **updates once**, or updates after both are processed.
    

---

### 🧪 **Real-World Variations of Limit Overrun Attacks**

| Type                     | Description                                                       |
| ------------------------ | ----------------------------------------------------------------- |
| 🎁 **Gift card**         | Redeem the same gift card multiple times.                         |
| ⭐ **Ratings abuse**      | Rate the same product multiple times to inflate reviews.          |
| 🏧 **Banking abuse**     | Withdraw more money than allowed by racing withdrawal requests.   |
| 🤖 **CAPTCHA**           | Reuse a single CAPTCHA solution across multiple forms.            |
| 🔐 **Rate limit bypass** | Bypass brute-force rate limiting by sending requests in parallel. |

---

###  **TOCTOU Flaw (Time-Of-Check to Time-Of-Use)**

- **Check:** “Has this been used?”
    
- **Use:** “Okay, apply it.”
    

The flaw: There’s **no atomic operation** linking check and use.  
So an attacker can **sneak in between**.

---

###  **Key Insight**

> Limit overrun race conditions exploit the **gap between validation and action**, allowing attackers to **abuse single-use or restricted functionality**.

---

## **Detecting and Exploiting Limit Overrun Race Conditions with Burp Repeater**


###  **Basic Steps**

1. **Identify** a single-use or rate-limited endpoint with security or functional impact.
    
2. **Send multiple requests** rapidly to try overrunning the limit (triggering the race condition).
    

---
###  **Key Challenge: Timing**

- The **race window** (the vulnerable time period) is often just **a few milliseconds**.
    
- Even if you send all requests "at once", delays like:
    
    - **Network jitter** (delay over internet)
        
    - **Server processing order**  
        ...can cause the exploit to fail.
    ![[Pasted image 20250722165046.png]]

---

####  **For HTTP/1**

- Uses **Last-Byte Synchronization**
    
    - Sends requests, holding back the last byte.
        
    - Releases all last bytes together so the server processes them nearly simultaneously.
        

####  **For HTTP/2**

- Uses **Single-Packet Attack**
    
    - Packs **20–30 requests** into a **single TCP packet**.
        
    - Server receives and handles them **at the exact same time**.
        
    - **Eliminates network jitter entirely**.
        
![[Pasted image 20250722165101.png]]
---

###  **Why Send Many Requests?**

- In theory, only **2 requests** may be needed.
    
- But sending **more** helps overcome:
    
    - **Internal delays** (server-side jitter)
        
    - **Unpredictable timing variations**
        
- Especially helpful during **discovery phase**.
---

### **Detecting and exploiting limit overrun race conditions with Turbo Intruder**

- **Purpose:**  
    Designed for complex, high-speed HTTP attacks (e.g. race conditions, retries, timing attacks).
    

- **Use Case:**  
    Ideal for attacks needing:
    
    - Multiple retries
        
    - Precise timing (e.g., staggered delays)
        
    - Huge volume of parallel requests
        

---

###  Single-Packet Attack (Race Condition Exploitation)

- **Requirements:**
    
    - Target must support **HTTP/2**.
            - Use `engine=Engine.BURP2`
        
    - Set `concurrentConnections=1` (ensures single connection).
        
    - Group requests using `gate='name'`.
        
    - Use `engine.openGate('name')` to send them **simultaneously**.
        

---

### 🧪 Example Code Summary

```python
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=1,
                           engine=Engine.BURP2)
    
    for i in range(20):
        engine.queue(target.req, gate='1')  # queue 20 requests
    
    engine.openGate('1')  # release them at once
```

---

###  Reference

- Example scripts: `race-single-packet-attack.py` in Turbo Intruder's template directory.
    
- Add Turbo Intruder via Burp → Extender → Extensions → Add JAR.
    

---

## To solve lab
1. Login as carlos and attempt for pass.
2. Intercept the HTTP2 login req.
3. Send to repeater, and create 20+ requests.
4. Add them in a group and send sequence in parallel.
5. Send them to turbo Extension .
6. In password , add %s .
7. Choose `race-single-packet-attack.py` script.
8. Edit loop,  `for word  in wordlists.clipboard:
        `engine.queue(target.req, word, gate='1')` (To use wordlist just copied )
        
    or to open any other list use `open(/usr/share/wordlists)` (eg).
9. Start attack, and look for http code. 


---

###  **What are Hidden Multi-Step Sequences?**

A **single HTTP request** might **internally trigger multiple steps** in an application—these are called **"sub-states"**. They are **invisible to the user**, but happen **behind the scenes** during request processing.

---

###  **Why This Matters (Vulnerability)**

Sometimes, in one of these sub-states, a user may **gain partial access** before all security checks are completed.  
Example: A session is created **before MFA (OTP)** is enforced.

---

###  **Example: MFA Bypass via Race**

**Code Logic:**

```python
session['userid'] = user.userid
if user.mfa_enabled:
    session['enforce_mfa'] = True
    # generate MFA code
    # redirect to OTP form
```

🧠 **What Happens:**

- Session is created 
    
- MFA enforcement is added **afterwards** 
    
- There's a **tiny window** (race condition) where user is **logged in but not MFA-verified**
    

---

### **How Attackers Exploit This**

An attacker sends:

1. A **login request** with valid credentials
    
2. **Immediately**, another request to a sensitive endpoint (e.g., dashboard)
    

If timed perfectly, the second request might be **accepted before MFA is enforced**, thus **bypassing MFA**.

---

###  **Key Insight**

This is a **logic flaw**, not just poor implementation.  
Such bugs are **application-specific**, so detecting them requires understanding how different parts of the app **interact in sequence**—even when it looks like a single step from the outside.

## **Methodology**

![[Pasted image 20250722215428.png]]

### **1. Predict Potential Collisions**

 **What to do:**  
Identify **endpoints** where two or more requests may interact with the **same data** (like the same user account, order, etc.).

 **Why:**  
Race conditions usually happen when multiple actions try to change the same thing at once.

 **Example:**  
Two requests to update the same password reset record = possible collision.

---

### **2. Probe for Clues**

🔍 **What to do:**  
Send the **same request multiple times**:

- First **sequentially** (one after another) — to see normal behavior.
    
- Then **in parallel** (at the same time) — to see if anything breaks.
    

 **Why:**  
Differences in responses, emails, app behavior, or timing may reveal a **race condition**.

 **Tools:**

- Burp Repeater (Send group in sequence / parallel)
    
- Turbo Intruder
    
- Trigger Race Condition (Burp Pro custom action)
    

---

### **3. Prove the Concept**

🔍 **What to do:**  
Refine the attack:

- **Remove unnecessary steps**
    
- Keep only the requests that trigger the bug
    
- Try to repeat the attack consistently
    

 **Why:**  
If it works reliably, you’ve confirmed the **vulnerability**.

 **Think of it as:**  
A structural flaw — the app mishandles concurrent access.

---
---

###  **Multi-Endpoint Race Conditions**

####  Concept:

- Exploiting logic flaws by racing requests to **different endpoints** (e.g., `/makePayment` and `/confirmOrder`) to manipulate application state.
    
- Happens in **multi-step workflows** where race windows exist between internal state transitions.
    

---

###  **Example Scenario:**

- Order status moves like:
    
    ```
    basket pending → [POST /makePayment] → payment validated → basket confirmed
    ```
    
- If you send another request (like adding more items) **between** `payment validated` and `basket confirmed`, you may **manipulate the order total**.
    

---

### 🛠️ **Challenges in Exploiting Multi-Endpoint Races**

#### 1.  **Different Processing Times**

- Endpoints perform different tasks → varying back-end delays.
    
- Makes race windows **hard to align**.
    

#### 2. 🌐 **Network Delays**

- **TCP Handshake**: Initial 3-way connection setup.
    
- **TLS Handshake**: Encryption setup for HTTPS.
    
- These cause **extra delay** in some requests, breaking sync.
    

---

###  **Bypass Techniques**

#### ✅ **Connection Warming**

- Send **non-essential requests** (like `GET /`) before the attack.
    
- Keeps TCP/TLS connection alive.
    
- Helps stabilize timing.
    

#### ✅ **Single-Connection Race**

- Use tools (e.g., **Burp Repeater**) with:
    
    - `Send group in sequence (single connection)` to reduce variation.
        

#### ✅ **Turbo Intruder**

- Sends multiple requests in the **same TCP packet**.
    
- Ideal for **precise timing attacks**.
    
- Can include **warming requests** + payloads.
    

---



#  Abusing Rate or Resource Limits to Exploit Race Conditions

---

##  The Core Problem

In race condition attacks, the attacker must send **multiple requests simultaneously** to exploit flawed application logic (e.g., skipping validation, bypassing steps like MFA or payment authorization).

### Challenges:

- **Different Endpoint Processing Times**
    
    - Requests like `/makePayment` and `/confirmOrder` may have **varying backend delays**.
        
- **Network Delays**
    
    - Factors like **TCP or TLS handshakes** can introduce unpredictable timing between requests.
        
- **High Jitter Targets**
    
    - Internet delays make simultaneous execution highly unreliable.
        

---

## Understanding the Image (Race Failure vs Success Attempt)



![[Pasted image 20250722221835.png]]
###  Top Bar: **Slow, Uncontrolled Race**

- Requests are sent close together.
    
- Backend timing causes one to lag (slow processing).
    
-  Exploit fails: requests don't hit the vulnerable logic at the same time.
    

###  Bottom Bar: **Improved Attempt with Delay**

- A small **client-side wait** is introduced before sending the second request.
    
- Still fails if backend behavior is unpredictable.
    
- Better timing, but **still unreliable**.
    

---

##  Techniques to Improve Race Condition Reliability

### ✅ 1. **Connection Warming**

- Pre-establish connections (TCP/TLS) to reduce delay before the real request is sent.
    
- Ensures faster dispatch of race-triggering requests.
    
- Not always enough, especially with backend delays.
    

### ✅ 2. **Client-Side Delays** (e.g., with Turbo Intruder)

- Add short delay between requests.
    
- Downside:
    
    - Involves **multi-packet TCP transmission** (slower than single packet).
        
    - ❌ Not effective on high-latency/jitter targets.
        

### ✅ 3. **Abusing Server-Side Rate/Resource Limits** 

- **Best technique** for unstable targets.
    
- **How it works:**
    
    1. Send **many dummy requests** to overwhelm the server or trigger **rate limiting**.
        
    2. This causes the server to **delay request processing**.
        
    3. Then send your real exploit requests.
        
    4. The server’s forced delay helps align both exploit requests precisely.
        
- ✅ Enables **single-packet attack reliability**, even when delayed execution is required.
    


![[Pasted image 20250722221851.png]]
---

##  Tools

- **Turbo Intruder** (from Burp Suite)
    
    - Automates sending requests in parallel.
        
    - Allows connection warming, precise delay control.
        
    - Can split requests across packets (or keep them in a single packet).
        

---

#  Single-endpoint Race Conditions – Session Hijacking via Race

---

##  Concept Overview

**Single-endpoint race conditions** occur when two (or more) parallel requests to the **same endpoint** modify **shared session or application state**, leading to **unintended behavior**.

---
![[Pasted image 20250722222444.png]]

### Context

- A password reset flow stores:
    
    - `session['reset-user']` = target username
        
    - `session['reset-token']` = generated token
        
- Then, it sends the token to the **username** set in the session.
    

---

###  Attack Breakdown

#### 🧵 Request 1 (Attacker)

1. `session['reset-user'] = hacker`
    
2. `session['reset-token'] = 1234`
    
3. Sends email to: **hacker**
    

#### 🧵 Request 2 (Attacker)

1. `session['reset-user'] = victim`
    
2. `session['reset-token'] = 9876`
    
3. Sends email to: **victim**
    

---

###  Race Window (the red block)

- Both requests **overlap in execution**.
    
- Final session state becomes:
    
    - `reset-user = victim`
        
    - `reset-token = 1234` (from Request 1)
        
- Result: Victim gets linked to attacker’s token.
    

---

###  Outcome

- Attacker receives a token (`1234`) **that resets the victim’s password**.
    
- Full **account takeover**.
    

---

## Why This Works

- The application logic stores temporary values in the session **sequentially**, but emails are often sent **asynchronously**.
    
- Backend race allows attacker to overwrite session variables **mid-process**.
    

---

##  Ideal Targets

- **Email-based operations**, like:
    
    - Password resets
        
    - Email confirmations
        
    - 2FA enrollments
        
- Especially vulnerable if:
    
    - Backend sends emails in background threads or queues.
        
    - Sessions are shared across requests.
        

---
Here are clean and concise notes on **Session-based Locking Mechanisms** — ideal for revision or study:

---

###  Session-based Locking Mechanisms

#### ✅ Purpose:

To prevent **accidental data corruption** in web applications when multiple requests are made concurrently from the **same session**.

---

#### ⚙️ How it Works:

Some frameworks (e.g., **PHP**) enforce a **lock per session**, meaning:

- **Only one request per session** is processed at a time.
    
- Any **parallel requests** wait until the current one finishes.
    

---

####  Why It's Important (Security Testing Perspective):

- This behavior can **hide race conditions**, which are usually exploitable.
    
- If the server processes requests **sequentially**, race condition attacks **fail silently**.
    

---

####  Testing Tip:

To bypass the lock and test for real race conditions:

> 📌 Use **a different session token** for each parallel request.

---
Here are concise and clear notes on **Partial Construction Race Conditions**:

---

###  Partial Construction Race Conditions

#### 🧱 Concept:
- Occur when an object (e.g., a **user account**) is created in **multiple steps**.
- Leaves a **temporary vulnerable state** where some fields (like API key or password) are **not yet set**.

---

####  Example Scenario:
- A user is added to the database → `INSERT INTO users ...`
- Then, an API key is set in a second query → `UPDATE users SET api_key = ...`
- Between these two steps, the user exists with a **null or empty API key**

---

####  Exploitation Idea:
- Send a request **during this race window** with:
  ```http
  GET /api/user/info?user=victim&api-key[]=
  ```
- This may **bypass API key checks**, if the server:
  - Treats the empty `api-key[]` as valid
  - Matches `null` or `""` with the uninitialized value in DB

---

####  Special Parameter Tricks:
- Some frameworks allow **non-standard input syntax**:
  - PHP:
    - `param[]=foo` → `param = ['foo']`
    - `param[]` → `param = []` (empty array)
  - Ruby on Rails:
    - `param[key]` with no value → `param = {"key" => nil}`

---

####  Password Hashing Note:
- Harder to exploit using **passwords** (since they are **hashed**).
- You'd need to craft an input that hashes to the same value as `null` or empty string.

---


##  Time-Sensitive Attacks

###  **Definition**

Attacks that exploit vulnerabilities based on **precise timing** rather than concurrent execution.

---

###  **Key Concept**

When applications use **timestamps instead of random values** for things like tokens or session IDs, attackers can:

- **Predict or duplicate tokens**
    
- **Hijack sessions or reset links** by syncing request timing
    

---

###  **Example: Predictable Password Reset Tokens**

- Password reset tokens are generated using **timestamps** (e.g., `token = timestamp`).
    
- Attacker triggers **two password reset requests** for:
    
    - `userA` (victim)
        
    - `userB` (attacker-controlled)
        
- If both requests hit the server at the **exact same moment**, they may receive the **same token**.
    
- Attacker gets the token for userB, which is also valid for userA → Reset victim's password.
    

---

###  **Techniques Used**

- Send parallel or precisely-timed requests
    
- Use tools like **Burp’s Turbo Intruder**, custom scripts, or **HTTP pipelining**
    
- Target functions generating tokens based on:
    
    - `Date.now()`
        
    - `System.currentTimeMillis()`
        
    - `time.time()` (Python)
        

---

### **Risk**

Tokens based on timestamps are **predictable**, **reusable**, and **vulnerable to replay attacks**.

---

### **Best Practice for Developers**

- Use **cryptographically secure random generators** (e.g., `crypto.randomBytes()`)
    
- Avoid using **timestamps** or sequential values for any security mechanism
    

---

## 🛡️ Preventing Race Condition Vulnerabilities

Race conditions occur when multiple processes access and manipulate shared data concurrently, leading to unexpected behavior. Preventing them requires **eliminating invisible sub-states** and ensuring **atomic, consistent operations**.

---

### ✅ **Secure Design Strategies**

---

### 1. **Avoid Mixing Data from Different Storage Places**

- Don’t pull related data from multiple sources (e.g., cache + DB + session) in the same process.
    
- ⚠️ Risk: Inconsistency due to out-of-sync data → race window.
    

---

### 2. **Make Sensitive State Changes Atomic**

- Use your **database’s concurrency features** (e.g., transactions, row-level locks).
    
- ✅ Example: Wrap order validation + payment in a **single DB transaction**.
    
- This ensures **all-or-nothing** execution, preventing partial or conflicting updates.
    

---

### 3. **Leverage Integrity & Consistency Features**

- Use **constraints** like:
    
    - Unique keys (`UNIQUE`)
        
    - Foreign keys
        
    - Checks (`CHECK`)
        
- 🛡️ Ensures even if logic fails, database blocks invalid or duplicate writes.
    

---

### 4. **Avoid Using One Storage Layer to Protect Another**

- ❌ Example: Don’t use **session variables** to limit how often the DB is updated.
    
- Reason: Sessions can be inconsistent under race conditions, and sessions ≠ true state.
    
- 🧠 Principle: Use **datastore rules** to protect the datastore itself.
    

---

### 5. **Ensure Session Consistency**

- Don't update session variables individually — always **batch-update** them.
    
- Frameworks (like some ORMs) may abstract this badly:
    
    - If your ORM doesn’t use **explicit transactions**, you risk partial writes.
        
- ⚠️ Danger: Session corruption or logic mismatches.
    

---

### 6. **Consider Stateless Designs (with Caution)**

- Use **encrypted client-side tokens** (e.g., JWTs) to carry state, avoiding server-side sessions.
    
- 🧠 Note: JWTs reduce race conditions by avoiding session storage altogether.
    
- ⚠️ But JWTs have their own risks (e.g., token tampering, expiration issues).
    
    - Read about **JWT attacks** before adopting this approach.
        

---

