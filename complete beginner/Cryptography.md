- Plaintext - Data before encryption or hashing, often text but not always as it could be a photograph or other file instead.

- Encoding - This is NOT a form of encryption, just a form of data representation like base64 or hexadecimal. Immediately reversible.

- Hash - A hash is the output of a hash function. Hashing can also be used as a verb, "to hash", meaning to produce the hash value of some data.

- Brute force - Attacking cryptography by trying every different password or every different key

- Cryptanalysis - Attacking cryptography by finding a weakness in the underlying maths.


- Message Digest 5 (MD5) is a cryptographic hash function that takes any input and produces a 128-bit hexadecimal number. The output of an MD5 hash function is called a digest. MD5 digests are often used to verify the integrity of files or data; however, MD5 is no longer considered secure and should not be used for sensitive applications.

- Hashing is used for 2 main purposes in Cyber Security. To verify integrity of data , or for verifying passwords.

| **Feature** | **Hashed** | **Encrypted** |
| ----------- | ---------- | ------------- |

|             |                                         |                                               |
| ----------- | --------------------------------------- | --------------------------------------------- |
| **Purpose** | Data integrity (one-way transformation) | Data confidentiality (two-way transformation) |

|                 |                |                          |
| --------------- | -------------- | ------------------------ |
| **Reversible?** | ❌ No (one-way) | ✅ Yes (can be decrypted) |

|              |                                   |                                                   |
| ------------ | --------------------------------- | ------------------------------------------------- |
| **Use Case** | Storing passwords, verifying data | Protecting sensitive data (e.g., messages, files) |

|                   |                                         |                                         |
| ----------------- | --------------------------------------- | --------------------------------------- |
| **Output Length** | Fixed-length (depends on hash function) | Variable-length (depends on input size) |

|                        |                      |               |
| ---------------------- | -------------------- | ------------- |
| **Example Algorithms** | MD5, SHA-256, bcrypt | AES, RSA, DES |
- Websites like Crackstation internally use HUGE rainbow tables to provide fast password cracking for hashes without salts. Doing a lookup in a sorted list of hashes is really quite fast, much much faster than trying to crack the hash.

- Windows New Technology LAN Manager (NTLM) is a suite of security protocols offered by Microsoft to authenticate users’ identity and protect the integrity and confidentiality of their activity.


- Automated hash recognition tools such as [https://pypi.org/project/hashID/](https://pypi.org/project/hashID/) exist, but they are unreliable for many formats. For hashes that have a prefix, the tools are reliable.
 - If you found the hash in a web application database, it's more likely to be md5 than **NTLM**.
 - The prefix tells you the hashing algorithm used to generate the hash. The standard format is`$format$rounds$salt$hash`.
- Windows passwords are hashed using NTLM, which is a variant of md4.
- On Linux, password hashes are stored in /etc/shadow.
- On Windows, password hashes are stored in the SAM.


- ### **Online Hash Cracking Services**

🔹 **[CrackStation](https://crackstation.net/)** – Uses a massive precomputed hash database.  
🔹 **[HashKiller](https://hashkiller.io/)** – Community-driven hash cracking.  
🔹 **[Hashes.com](https://hashes.com/) ** – Free online lookup & cracking service.  
🔹 **[OnlineHashCrack](https://www.onlinehashcrack.com/)** – Supports password recovery.  
🔹 **[MD5Online](https://www.md5online.org/)** – Focused on MD5 hashes.

### **Offline Tools (More Powerful)**

⚡ **John the Ripper** – Fast brute force/dictionary attack tool.  
⚡ **Hashcat** – GPU-accelerated hash cracking.  
⚡ **[Hydra](https://github.com/vanhauser-thc/thc-hydra)** – Online password cracker.

- ### **What is a GPU?** 🎮💻

A **GPU (Graphics Processing Unit)** is a specialized processor designed to handle **parallel processing tasks**, mainly for **graphics rendering** and **computational workloads** like AI, gaming, and crypto mining.

### **What is HMAC?** 🔐

HMAC (**Hash-based Message Authentication Code**) is a way to **verify both authenticity & integrity** of data using a **hash function and a secret key**.

---

### **How HMAC Works?**

1️⃣ **A secret key** is combined with the message.  
2️⃣ The **message + key** are hashed using a cryptographic function (e.g., **SHA-512**).  
3️⃣ The result is an **HMAC hash**, which can be used to verify the message.


A great place to find more hash formats and password prefixes is the hashcat example page, available here: [https://hashcat.net/wiki/doku.php?id=example_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)


- **Symmetric encryption** is a method in which the same key is used for both encryption and decryption. Consequently, this key must remain secure and never be disclosed to anyone except the intended party. 
- Examples of symmetric encryption are DES (Data Encryption Standard), 3DES (Triple DES) and AES (Advanced Encryption Standard).
- **Asymmetric encryption** is a method that uses two different keys: a public key for encryption and a private key for decryption.
- Examples are RSA, Diffie-Hellman, and Elliptic Curve cryptography (ECC).

- cryptography have basic algorithm like
- XOR Operation
0 ⊕ 0 = 0, 0 ⊕ 1 = 1, 1 ⊕ 0 = 1, and 1 ⊕ 1 = 0.
- Modulo Operation

- **Authentication**: You want to be sure you communicate with the right person, not someone else pretending.
- **Authenticity**: You can verify that the information comes from the claimed source.
- **Integrity**: You must ensure that no one changes the data you exchange.
- **Confidentiality**: You want to prevent an unauthorised party from eavesdropping on your conversations.
# Public Key Cryptography Basics
### Analogy

In this metaphor, the secret code represents a symmetric encryption cipher and key, the lock represents the server’s public key, and the key represents the server’s private key.

| Analogy     | Cryptographic System                |
| ----------- | ----------------------------------- |
| Secret Code | Symmetric Encryption Cipher and Key |
| Lock        | Public Key                          |
| Lock’s Key  | Private K                           |
## RSA ctf tool : 
- https://github.com/RsaCtfTool/RsaCtfTool
- https://github.com/ius/rsatool

### **RSA Encryption ** 🔐

#### **1️⃣ Why is RSA Secure?**

- RSA relies on the difficulty of **factoring large numbers**.
- Multiplying two large primes is **easy**, but finding the original primes from their product is **hard**.
- This makes RSA a strong encryption method.

#### **2️⃣ How RSA Works?**

- **Key Generation:**
    - Choose two large prime numbers (**p, q**) and compute **n = p × q**.
    - Compute **ϕ(n) = (p - 1) × (q - 1)**.
    - Choose **e** (public exponent) such that **e** and **ϕ(n)** are coprime.
    - Compute **d** (private exponent) where **(e × d) mod ϕ(n) = 1**.
- **Encryption:**
    - Public key = **(n, e)**
    - Ciphertext = **(plaintext^e) mod n**
- **Decryption:**
    - Private key = **(n, d)**
    - Plaintext = **(ciphertext^d) mod n**
1. Bob chooses two prime numbers: _p_ = 157 and _q_ = 199. He calculates _n_ = _p_ × _q_ = 31243.
2. With _ϕ_(_n_) = _n_ − _p_ − _q_ + 1 = 31243 − 157 − 199 + 1 = 30888, Bob selects _e_ = 163 such that _e_ is relatively prime to _ϕ_(_n_); moreover, he selects _d_ = 379, where _e_ × _d_ = 1 mod _ϕ_(_n_), i.e., _e_ × _d_ = 163 × 379 = 61777 and 61777 mod 30888 = 1. The public key is (_n_,_e_), i.e., (31243,163) and the private key is $(n,d), i.e., (31243,379).
3. Let’s say that the value they want to encrypt is _x_ = 13, then Alice would calculate and send _y_ = _x__e_ mod _n_ = 13163 mod 31243 = 16341.
4. Bob will decrypt the received value by calculating _x_ = _y__d_ mod _n_ = 16341379 mod 31243 = 13. This way, Bob recovers the value that Alice sent.
#### **3️⃣ RSA in CTFs**

- Often involves **recovering plaintext** from given RSA parameters.
- **Important variables:**
    - **p, q** → Prime numbers
    - **n** → Product of p & q
    - **e, d** → Public & private exponents
    - **m, c** → Plaintext & ciphertext

#### **4️⃣ Tools for Cracking RSA in CTFs**

- **RsaCtfTool**
- **rsatool**

### **Diffie-Hellman Key Exchange –**  🔐

#### **🔹 Purpose:**

- Allows two parties to establish a **shared secret key** over an **insecure** communication channel.
- This key is then used for **symmetric encryption** to secure future messages.

#### **🔹 How It Works:**

1. Alice and Bob generate secrets independently; let’s call these secrets A and B. They also have some public common material; let’s call this C.
2. Alice and Bob agree on the **public variables**: a large prime number _p_ and a generator _g_, where 0 < _g_ < _p_. These values will be disclosed publicly over the communication channel. Although insecurely small, we will choose _p_ = 29 and _g_ = 3 to simplify our calculations.
3. Each party chooses a private integer. As a numerical example, Alice chooses _a_ = 13, and Bob chooses _b_ = 15. Each of these values represents a **private key** and must not be disclosed.
4. It is time for each party to calculate their **public key** using their private key from step 2 and the agreed-upon public variables from step 1. Alice calculates _A_ = _g__a_ mod _p_ = 313 mod 29 = 19 and Bob calculates _B_ = _g__b_ mod _p_ = 315 mod 29 = 26. These are the public keys.
5. Alice and Bob send the keys to each other. Bob receives _A_ = _g__a_ mod _p_ = 19, i.e., Alice’s public key. And Alice receives _B_ = _g__b_ mod _p_ = 26, i.e., Bob’s public key. This step is called the **key exchange**.
6. Alice and Bob can finally calculate the **shared secret** using the received public key and their own private key. Alice calculates _B__a_ mod _p_ = 2613 mod 29 = 10 and Bob calculates _A__b_ mod _p_ = 1915 mod 29 = 10. Both calculations yield the same result, _g__a__b_ mod _p_ = 10, the shared secret key.
7. ![[Pasted image 20250218173333.png]]
#### **🔹 Why It’s Secure?**

- Even if an attacker sees p,g,A,Bp, g, A, B, they **cannot** compute the secret key efficiently.

#### **🔹 Diffie-Hellman & RSA Together:**

- **Diffie-Hellman**: Securely exchanges keys.
- **RSA**: Used for authentication, digital signatures, and key transport.
- **Together**, they prevent **Man-in-the-Middle (MITM) attacks** and enhance security.
**Real-world Use:** Found in TLS (HTTPS), VPNs, and secure messaging apps. 


###  SSH Authentication & Key Generation**

#### **1. Authenticating the Server**

- When connecting via SSH, the client checks if it recognizes the server’s public key fingerprint.
- If not recognized, a warning appears:
    
    ```
    The authenticity of host '10.10.244.173' can't be established.
    ED25519 key fingerprint is SHA256:lLzhZc7YzRBDchm02qTX0qsLqeeiTCJg5ipOT0E/YM8.
    ```
    
- The user must verify the fingerprint before trusting the connection.
- If accepted, the server’s key is added to `~/.ssh/known_hosts` to prevent future warnings.

#### **2. Authenticating the Client**

- Instead of passwords, SSH can use public-private key pairs for authentication.
- **Key Pair Generation** (default: Ed25519):
    
    ```bash
    ssh-keygen -t ed25519
    ```
    
- Creates:
    - **Public Key (`id_ed25519.pub`)** → Shared with the server.
    - **Private Key (`id_ed25519`)** → Kept secret and secured.

#### **3. Key Security**

- **Never share the private key!** It grants access to the associated account.
- **Passphrase (optional)** adds extra encryption to the private key.
- If using **RSA keys**, they are longer but still widely used.




### **SSH Private Keys & Authentication**

- **Keep your private key secret**—it's like a password. If stolen, an attacker can log in unless it's encrypted with a **passphrase**.
- The **passphrase** only decrypts the key; it is never sent to the server.
- **John the Ripper** can crack weak passphrases, so use a strong one.

### **Generating & Using SSH Keys**

1. Generate keys on your machine:
    
    ```bash
    ssh-keygen -t ed25519
    ```
    
2. Copy the **public key** to the target:When generating an SSH key to log in to a remote machine, you should generate the keys on your machine and then copy the public key over, as this means the private key never exists on the target machine using `ssh-copy-id`.
    
    ```bash
    ssh-copy-id user@host
    ```
    
3. Set correct **permissions** (`chmod 600 private_key`) or SSH may ignore the key.
4. Connect using your private key:
    
    ```bash
    ssh -i private_key user@host
    ```
    

### **Authorized Keys on Remote Host**

- **`~/.ssh/authorized_keys`** stores trusted public keys.
- Many Linux systems enable **key authentication** by default.
- For security, root access should only allow **key-based login**, not passwords.
- **Keys Trusted by the Remote Host**

The `~/.ssh` folder is the default place to store these keys for OpenSSH. The `authorized_keys` (note the US English spelling) file in this directory holds public keys that are allowed access to the server if key authentication is enabled. By default on many Linux distributions, key authentication is enabled as it is more secure than using a password to authenticate. Only key authentication should be accepted if you want to allow SSH access for the root user.
### **Using SSH Keys for a Better Shell**

In **CTFs, penetration testing, and red teaming**, a basic reverse shell is often unstable—it can break when you press **Ctrl+C**, and it lacks features like **tab completion**.

A better way is to use **SSH keys** to get full access. If you can write your **public key** to a user's `~/.ssh/authorized_keys` file, you can log in via SSH as that user. This gives:  
✅ A stable, fully interactive shell  
✅ No need to restart the reverse shell  
✅ Persistent access (a backdoor)

⚠ **Limitations:**

- The user must have SSH login enabled.
- **`www-data` (web server user) usually can’t log in via SSH.**
- Root or regular users are more likely to work.

### **Digital Signatures & Certificates**

In the **analogue world**, signatures confirm identity and agreement. In the **digital world**, **digital signatures** do the same using **asymmetric cryptography**.

### **Digital Signature:**

- Uses a **private key** to sign a document.
- Verified with the **public key** to ensure authenticity and integrity.
- More secure than just an image of a signature.

### **TLS Certificates & Trust:**

- Websites use **TLS certificates** for **HTTPS security**.
- Issued by **Certificate Authorities (CAs)** and verified through a **chain of trust**.
- Browsers trust a certificate if a known CA signs it.
- Free TLS certificates are available via **Let’s Encrypt**.


## GPG
- GPG stands for GNU Privacy Guard. It is a free and open-source encryption software that uses public-key cryptography. GPG can be used to encrypt files and messages, and to sign files and messages. Encryption makes it so that only the intended recipient can decrypt the file or message while signing makes it so that the recipient can verify that the file or message was sent by the person it claims to be from.

## **GPG and PGP**

1. **Purpose of GPG**:
    
    - **GPG (GNU Privacy Guard)** is an open-source implementation of the **OpenPGP** standard, which is used for encrypting files and emails, as well as signing them to ensure authenticity and integrity.
    - It is used to **protect the confidentiality of email messages** and **confirm their integrity**.
2. **Generating GPG Keys**:
    
    - When creating a GPG key, you can select the **cryptographic algorithm** (e.g., RSA, ECC) and whether you want to **sign** only or **sign and encrypt**.
    - You will also set an **expiry date** for the key, which determines how long it remains valid.
    - You must provide **identifying information** such as your **name**, **email address**, and optionally, a **comment** about the key's purpose.
3. **Key Types**:
    
    - **RSA and RSA**: Used for signing and encrypting.
    - **DSA and Elgamal**: For signing and encryption, but Elgamal is often less common.
    - **ECC (Elliptic Curve Cryptography)**: Offers stronger security with shorter key lengths.
    - **Curve 25519**: A default ECC choice, preferred for its security and performance.
4. **Key Expiry**:
    
    - You can set a key to **never expire** or specify an expiration period (e.g., **in days, weeks, months, or years**). It's recommended to set an expiration date for security purposes.
5. **User ID**:
    
    - A **User ID** is associated with your GPG key and contains information like your **name** and **email address**. This helps identify who owns the key.
6. **Protecting with Passphrase**:
    
    - GPG private keys can be **passphrase-protected**, similar to SSH keys, to prevent unauthorized access. A passphrase adds a layer of security to your key.
    - If the key is **passphrase-protected**, you can use tools like **John the Ripper** with **gpg2john** to attempt cracking the passphrase (though this is only for ethical testing or when you're authorized).
7. **GPG in CTFs**:
    
    - In **Capture The Flag (CTF)** challenges, GPG is often used to **decrypt files** or messages. You might need to use your private key to access encrypted content.
- Now that you have your GPG key pair, you can share the public key with your contacts. Whenever your contacts want to communicate securely, they encrypt their messages to you using your public key. To decrypt the message, you will have to use your private key. Due to the importance of the GPG keys, it is vital that you keep a backup copy in a secure location.

- Let’s say you got a new computer. All you need to do is import your key, and you can start decrypting your received messages again:

- You would use `gpg --import backup.key` to import your key from backup.key
- To decrypt your messages, you need to issue `gpg --decrypt confidential_message.gpg`


## Terms
- **Cryptography** is the science of securing communication and data using codes and ciphers.
- **Cryptanalysis** is the study of methods to break or bypass cryptographic security systems without knowing the key.
- **Brute-Force Attack** is an attack method that involves trying every possible key or password to decrypt a message.
- **Dictionary Attack** is an attack method where the attacker tries dictionary words or combinations of them.
