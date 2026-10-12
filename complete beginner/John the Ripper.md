
### **P vs NP in Computer Science (Algorithms)**

- **P (Polynomial Time):** Problems that can be **solved efficiently** (in polynomial time) by a **deterministic** algorithm.
    
    - Example: Sorting (Merge Sort, Quick Sort) → **O(n log n)**
- **NP (Nondeterministic Polynomial Time):** Problems where a **solution can be verified efficiently** but may not be easy to **solve efficiently**.

       - Example : sudoku
- Syntax: `john [options] [file path]`

### Automatic cracking Syntax
- `john --wordlist=[path to wordlist] [path to file]`

- `--wordlist=`: Specifies using wordlist mode, reading from the file that you supply in the provided path
- `[path to wordlist]`: The path to the wordlist you’re using, as described in the previous task
-  online hash identifier like(hash.com) [this site](https://hashes.com/en/tools/hash_identifier). [hash-identifier](https://gitlab.com/kalilinux/packages/hash-identifier/-/tree/kali/master)
- After hash identification use : `john --format=[format] --wordlist=[path to wordlist] [path to file]`

     - `--format=`: This is the flag to tell John that you’re giving it a hash of a specific format and to use the following format to crack it
    - `[format]`: The format that the hash is in.
    - Example: `john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt`
    - john --show --format=raw-md5 hash1.txt : to show password

    - NOTE : if you’re dealing with a standard hash type, e.g. md5, you have to prefix it with `raw-` to tell John you’re just dealing with a standard hash type ,though this doesn’t always apply. To check if you need to add the prefix or not, you can list all of John’s formats using `john --list=formats` and either check manually or grep for your hash type using something like `john --list=formats | grep -iF "md5"`
## Unshadow
When cracking **Linux password hashes**, `john` requires both the **`/etc/passwd`** and **`/etc/shadow`** files because:

- `/etc/passwd` contains **usernames** and **UIDs**, but only an `x` instead of the actual password hash.
- `/etc/shadow` contains **password hashes**, but it does not have user information in a format that John can use directly.

Since `john` needs both pieces of information together, we use the **`unshadow`** tool to merge them.

---

### **Basic Syntax:**
unshadow [path_to_passwd]  [path_to_shadow] > unshadowed.txt
- **`unshadow`** → Combines the two files into a format John can understand.
- **`[path_to_passwd]`** → Path to the copied **`/etc/passwd`** file.
- **`[path_to_shadow]`** → Path to the copied **`/etc/shadow`** file.
- **`>`** → Redirects the output to a new file (`unshadowed.txt`).

### **Cracking the Hash with John:**

Once we have `unshadowed.txt`, we can crack it using a wordlist:
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt

- `$1$` → MD5
- `$5$` → SHA-256
- `$6$` → SHA-512 (common in modern Linux)
## Single crack Mode
- John uses only the info provided in the username to crack password.
- **Word Mangling:rule-based modification** of words in a dictionary to increase the chances of cracking passwords.(e.g., `password` → `P@ssw0rd`)
- **GECOS (General Electric Comprehensive Operating System) refers to a **field in the `/etc/passwd` file** that stores additional user information(such as the user’s full name, office number, and telephone number, among other things). It is not directly related to authentication but can provide useful metadata about a user.**
- `john --single --format=[format] [path to file]`

### **Custom Rules in John the Ripper: Explanation**

Custom rules allow you to modify dictionary words dynamically using **John the Ripper**'s built-in rules engine. This is useful when targeting passwords that follow **predictable patterns**, such as adding numbers or symbols at the end.

---

## **1️⃣ Why Use Custom Rules?**

Many organizations enforce **password complexity policies** that require:  
✔ **Uppercase letters**  
✔ **Lowercase letters**  
✔ **Numbers**  
✔ **Symbols**

Users often **follow predictable patterns** when meeting these requirements. Example:  
🔹 **Weak password** → `polopassword`  
🔹 **Modified for complexity** → `Polopassword1!`

Custom rules allow **John** to generate such variations **dynamically**, reducing the need for massive pre-generated wordlists.

---

## **2️⃣ How to Create Custom Rules**

Custom rules are defined in **john.conf** (location varies based on installation):

- **TryHackMe AttackBox**: `/opt/john/john.conf`
- **Linux (Package Install)**: `/etc/john/john.conf`

### **Basic Syntax**

A rule starts with:

```plaintext
[List.Rules:RuleName]
```

Then, **modifiers** define how words will be altered.

---

## **3️⃣ Understanding Rule Modifiers**

| Modifier | Effect                                   |
| -------- | ---------------------------------------- |
| `Az`     | **Appends** characters at the end        |
| `A0`     | **Prepends** characters at the beginning |
| `c`      | **Capitalizes** the first letter         |

### **Character Sets**

| Syntax    | Effect                                |
| --------- | ------------------------------------- |
| `[0-9]`   | Includes numbers `0-9`                |
| `[A-Z]`   | Includes **only** uppercase letters   |
| `[a-z]`   | Includes **only** lowercase letters   |
| `[A-z]`   | Includes **both** upper & lowercase   |
| `[!£$%@]` | Includes these **special characters** |

---

## **4️⃣ Example: Matching "Polopassword1!"**

We create a rule named **PoloPassword** in `john.conf`:

```plaintext
[List.Rules:PoloPassword]
cAz"[0-9] [!£$%@]"
```

### **What This Rule Does**

✔ `c` → **Capitalizes** the first letter (`polopassword` → `Polopassword`)  
✔ `Az"[0-9]"` → **Appends a digit** (e.g., `Polopassword1`)  
✔ `Az"[!£$%@]"` → **Appends a special character** (e.g., `Polopassword1!`)

Generated variations:  
✅ `Polopassword1!`  
✅ `Polopassword2@`  
✅ `Polopassword3$`

---

## **5️⃣ Using Custom Rules in John**

Once defined, you can use the rule in a **cracking attempt** like this:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --rules=PoloPassword hash.txt
```

---
## Zip2john
- Convert the Zip file into a Hash format.
- `zip2john [options] [zip file] > [output file]`
   - Eg: `zip2john zipfile.zip > zip_hash.txt`
**CRACKING ZIP2JOHN: `john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt`**

## Rar2John
tool to convert the RAR file into a hash format that John can understand. The basic syntax is as follows:

`rar2john [rar file] > [output file]`

- `rar2john`: Invokes the `rar2john` tool
- `[rar file]`: The path to the RAR file you wish to get the hash of
- `>`: This redirects the output of this command to another file
- `[output file]`: This is the file that will store the output from the command  
    

**Example Usage**

`/opt/john/rar2john rarfile.rar > rar_hash.txt`


## SSH2John
- `ssh2john` converts the `id_rsa` private key, which is used to log in to the SSH session, into a hash format that John can work with.
- `python /usr/share/john/ssh2john.py`
- syntax: `ssh2john [id_rsa private key file] > [output file]`
- Cracking: `john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt`