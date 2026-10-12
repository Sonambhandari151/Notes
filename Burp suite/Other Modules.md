
The **Decoder module** in Burp Suite is a powerful tool for manipulating data by encoding, decoding, and hashing. Here's a breakdown of how it works:

### **1. Purpose of the Decoder Module**

- Converts intercepted data from an encoded or obfuscated format back into readable text.
    
- Encodes data to prepare it for transmission to a target system.
    
- Computes **hash values** (e.g., MD5, SHA-256) for integrity checks.
    
- Features **Smart Decode**, which automatically attempts to decode data recursively.
    

### **2. How to Use the Decoder Module**

- **Accessing Decoder:** Navigate to the _Decoder_ tab in Burp Suite.
    
- **Input Workspace:** Enter or paste data for encoding/decoding.
    
- **Right-Click Option:** Data from other Burp Suite tools can be sent to Decoder.
    
- **Text vs. Hex Mode:** Choose to interpret the input as **plain text** or **hexadecimal byte values**.
    
- **Transformation Options:** Use dropdown menus to:
    
    - Encode (e.g., URL encoding, Base64)
        
    - Decode (reverse encoding)
        
    - Hash (generate cryptographic hashes)
        
- **Smart Decode:** Automatically tries multiple decoding techniques until the original plaintext is restored.
    

---
