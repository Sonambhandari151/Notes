
- **Ciphertext** - The result of encrypting a plaintext, encrypted data

- **Cipher** - A method of encrypting or decrypting data. Modern ciphers are cryptographic, but there are many non cryptographic ciphers like Caesar.

- **Plaintext** - Data before encryption, often text but not always. Could be a photograph or other file

- **Encryption** - Transforming data into ciphertext, using a cipher.

- **Encoding** - NOT a form of encryption, just a form of data representation like base64. Immediately reversible.

- **Key** - Some information that is needed to correctly decrypt the ciphertext and obtain the plaintext.

- **Passphrase** - Separate to the key, a passphrase is similar to a password and used to protect a key.

- **Asymmetric encryption** - Uses different keys to encrypt and decrypt.

- **Symmetric encryption** - Uses the same key to encrypt and decrypt

- **Brute force** - Attacking cryptography by trying every different password or every different key

- **Cryptanalysis** - Attacking cryptography by finding a weakness in the underlying maths
 - **Cryptography**: protect confidentiality, ensure integrity, ensure authenticity.
 - Standards like [PCI-DSS](https://www.pcisecuritystandards.org/documents/PCI_DSS_for_Large_Organizations_v1.pdf) state that the data should be encrypted both at rest (in storage) AND while being transmitted.

## Encryption Types
The two main categories of Encryption are symmetric and asymmetric.

**Symmetric encryption** uses the same key to encrypt and decrypt the data. Examples of Symmetric encryption are DES (Broken) and AES. These algorithms tend to be faster than asymmetric cryptography, and use smaller keys (128 or 256 bit keys are common for AES, DES keys are 56 bits long).

**Asymmetric encryption** uses a pair of keys, one to encrypt and the other in the pair to decrypt. Examples are RSA and Elliptic Curve Cryptography. Normally these keys are referred to as a public key and a private key. Data encrypted with the private key can be decrypted with the public key, and vice versa. Your private key needs to be kept private, hence the name. Asymmetric encryption tends to be slower and uses larger keys, for example RSA typically uses 2048 to 4096 bit keys.

RSA and Elliptic Curve cryptography are based around different mathematically difficult (intractable) problems, which give them their strength.


## RSA - Rivest Shamir Adleman
- RSA is based on the mathematically difficult problem of working out the factors of a large number.
- There are some excellent tools for defeating RSA challenges in CTFs, and my personal favorite is [https://github.com/Ganapati/RsaCtfTool](https://github.com/Ganapati/RsaCtfTool) which has worked very well for me. I’ve also had some success with [https://github.com/ius/rsatool](https://github.com/ius/rsatool).
- The key variables that you need to know about for RSA in CTFs are p, q, m, n, e, d, and c.
  “p” and “q” are large prime numbers, “n” is the product of p and q.
  The public key is n and e, the private key is n and d.
  “m” is used to represent the message (in plaintext) and “c” represents the ciphertext (encrypted text).
- MuirlandOracle’s blog to learn maths behind rsa: [https://muirlandoracle.co.uk/2020/01/29/rsa-encryption/](https://muirlandoracle.co.uk/2020/01/29/rsa-encryption/).


## Establishing Keys Using Asymmetric Cryptography
- A very common use of asymmetric cryptography is exchanging keys for symmetric encryption.
- The secret code represents a symmetric encryption key, the lock represents the server’s public key, and the key represents the server’s private key.
- In reality, you need a little more cryptography to verify the person you’re talking to is who they say they are, which is done using digital signatures and certificates. You can find a lot more detail on how HTTPS (one example where you need to exchange keys) really works from this excellent blog post. [https://robertheaton.com/2014/03/27/how-does-https-actually-work/](https://robertheaton.com/2014/03/27/how-does-https-actually-work/)

## Digital Signatures
- A **digital signature** verifies the authenticity of a file using **asymmetric cryptography**. You sign with your **private key**, and others verify it with your **public key**. This ensures only you could have signed it. In the UK, digital signatures hold the same legal value as physical ones.The simplest form of digital signature would be encrypting the document with your private key, and then if someone wanted to verify this signature they would decrypt it with your public key and check if the files match.

## Certificates 
- **Certificates** verify a server’s identity using **public key cryptography**. In HTTPS, a website's certificate proves it is legitimate. A **chain of trust** links certificates back to a trusted **Root CA**, which your OS or browser automatically trusts. You can get free HTTPS certificates from **Let’s Encrypt** for secure websites. 

## Encryption and SSH Authentication
- SSH authentication defaults to **usernames and passwords**, but often uses **key-based authentication** for security. This relies on **public-private key pairs**, typically **RSA** by default. Keys can be generated using `ssh-keygen`, with optional encryption via a **passphrase**. 

- ### **Important Points on SSH Private Keys**

- **Keep Private Keys Secure**: Treat them like passwords; never share them.
- **Passphrase Security**: Encrypts the key but is **not** used for authentication.
- **Brute-force Risk**: Encrypted keys can be attacked using tools like **John the Ripper**. you can attack an encrypted SSH key to attempt to find the passphrase, which highlights the importance of using a secure passphrase and keeping your private key private.
- **Key Generation Best Practice**: Generate keys on **your machine** and only copy the **public key** to the server.
- **Storage Location**: Default folder is `~/.ssh/`; authorized public keys are stored in `authorized_keys`.
- **Correct Permissions**: Private keys must be **read/write only** for the owner (`chmod 600`).
- **Using Private Key**: Connect using `ssh -i private_key user@host`.
- **Upgrading Shells**: SSH keys provide a **stable** shell, avoiding reverse shell issues. 
## Diffie Hellman Key Exchange
Alice and Bob want to talk securely. They want to establish a common key, so they can use symmetric cryptography, but they don’t want to use key exchange with asymmetric cryptography. This is where DH Key Exchange comes in.
- **Diffie-Hellman Key Exchange **

- **Purpose:** Establish a shared secret key for secure communication without directly exchanging it.
- **Process:**
    1. Alice & Bob each generate a secret (A & B).
    2. They use a common public value (C).
    3. Alice & Bob mix their secret with C to get AC & BC.
    4. They exchange AC & BC.
    5. Each combines the received value with their secret to get the same shared key (ABC).
- **Security:** Even if an attacker intercepts AC & BC, they can't easily derive ABC.

This shared key is then used for symmetric encryption. 

# PGP
PGP stands for Pretty Good Privacy. **PGP Purpose:** Ensures **confidentiality, integrity, authentication, and non-repudiation** using encryption and digital signatures.
### What is GPG?

[GnuPG or GPG](https://gnupg.org/) is an Open Source implementation of PGP from the GNU project. You may need to use GPG to decrypt files in CTFs. With PGP/GPG, private keys can be protected with passphrases in a similar way to SSH private keys. If the key is passphrase protected, you can attempt to crack this passphrase using John The Ripper and gpg2john. The key provided in this task is not protected with a passphrase.

The man page for GPG can be found online [here](https://www.gnupg.org/gph/de/manual/r1023.html).

### What about AES?

AES, sometimes called Rijndael after its creators, stands for Advanced Encryption Standard. It was a replacement for DES which had short keys and other cryptographic flaws.

AES and DES both operate on blocks of data (a block is a fixed size series of bits).

AES is complicated to explain, and doesn’t seem to come up as often. If you’d like to learn how it works, here’s an excellent video from Computerphile [https://www.youtube.com/watch?v=O4xNJsjtN6E](https://www.youtube.com/watch?v=O4xNJsjtN6E)



