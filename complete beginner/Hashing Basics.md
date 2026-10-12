# Hashing Basics

- Hashing is used for data integrity and password authentication, storage (not password management).
- Insecure password practices:
    1. storing pass in plaintext.
      `wc -l rockyou.txt : line count`
      `head rockyou.txt: by default show 10 lines.`

	1. using a deprecated encryption. EG.
	  - **MD5** – Weak hash function, prone to collisions.
     - **SHA-1** – Vulnerable to collision attacks.
     - **DES (Data Encryption Standard)** – Uses a short 56-bit key, easily brute-forced.
     - **RC4** – Weak stream cipher, prone to key recovery attacks.
     - **TLS 1.0 / TLS 1.1** – Outdated encryption protocols, replaced by TLS 1.2 and TLS 1.3.
     - for security use **AES , SHA-256+, TLS 1.2+**
    1.  insecure hashing algorithm.
- Salts are added to either the start or end of the pass, making every password unique.
- Hash functions like **Bcrypt & Scrypt** handles this automatically.
- Salts don't need to be pvt.

## Good security Practice
1. select secure hashing function, such as Argon2, Scrypt, Bcrypt, PBKDF2.
2. Add a unique salt.
3. Concatenate password with salt.
4. Calculate hash value of combined password and salt.
5. Store.


### **Why Not Encrypt Passwords?**

- Encryption requires a **key** to **decrypt** the passwords.
- If an attacker gains access to the encryption key, they can decrypt **all** stored passwords instantly.
- This makes encryption a **single point of failure** for password security.
==Hashing is irreversible , so it's a better approach.==
### **When is Encryption Used?**

- Encryption is good for **data that needs to be recovered**, like files or messages.
- Hashing is better for **authentication** since the system only needs to verify, not decrypt, passwords.

## Recognising Password Hashes
https://pypi.org/project/hashID/ : Automated hash recognition.(reliable for hashes that have a prefix)
- Web application databse : more likely MD5 than NTLM.

## Linux passwords
- Pass hashes store in /etc/shadow
- Each line contain 9 fields , separated by **(:)** , FIrst- login name, 2nd - encrypted password(if started with ! - its locked).
- In linux, Passwords are hashed not encrypted.
- Encrytped password field contains hashed passphrase with 4 components.
     **`$prefix$options$salt$hash` : prefix(algo id), options(parameters), salt , hash**
![[prefix.png]]
## Windows Passwords
- Hashed usinf NTLM(variant of MD4).
- STored in SAM ( Security Accounts Manager).
- Splitted into NT and LM hashes.d
- **Minikatz:**  Used to extract **passwords, hashes, Kerberos tickets**, and perform attacks like **Pass-the-Hash, Pass-the-Ticket, and Golden Ticket** for **privilege escalation** and **lateral movement** in Windows systems.
- A great place to find more hash formats and password prefixes is the [Hashcat Example Hashes](https://hashcat.net/wiki/doku.php?id=example_hashes) page.
- Hashcat & John the Ripper: password cracking tools
       - `hashcat -m <hash_type> -a <attack_mode> hashfile wordlist`
          - `-m <hash_type>` specifies the hash-type in numeric format. For example, `-m 1000` is for NTLM.
           - `-a <attack_mode>` specifies the attack-mode. For example, `-a 0` is for straight, i.e., trying one password from the wordlist after the other.
          - `hashfile` is the file containing the hash you want to crack.
         - `wordlist` is the security word list you want to use in your attack.
## HMAC
HMAC works by hashing the message **twice** with a **secret key** to ensure **authenticity** and **integrity**.

### **How it works:**

1. **Prepare the key** → Adjust the key to match the block size.
2. **Inner Hash** → XOR the key with `ipad` (inner padding) and hash it with the message.
3. **Outer Hash** → XOR the key with `opad` (outer padding), then hash the result of Step 2.

### **Formula Breakdown:**

HMAC(K,M)=H((K⊕opad)∣∣H((K⊕ipad)∣∣M))HMAC(K, M) = H((K \oplus opad) || H((K \oplus ipad) || M))

- **KK** → Secret key
- **MM** → Message
- **HH** → Hash function (e.g., SHA-256, MD5)
- **⊕\oplus** → XOR operation
- **ipad,opadipad, opad** → Fixed padding values

- **`ipad` (Inner Padding)** → A byte pattern of **0x36**, repeated to match the block size of the hash function.
- **`opad` (Outer Padding)** → A byte pattern of **0x5C**, repeated to match the block size.![[Pasted image 20250225124958.png]]