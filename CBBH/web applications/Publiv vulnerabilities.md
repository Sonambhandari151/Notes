
### 🔸 1. **Back-End Component Vulnerabilities**

- These vulnerabilities are often exploitable **remotely**, without needing local access.
    
- Caused by **coding mistakes** during development.
    
- Can range from:
    
    - Simple misconfigurations
        
    - To complex logic flaws
        
- They are commonly targeted in **external penetration testing**.
    

---

### 🔸 2. **Public CVE (Common Vulnerabilities and Exposures)**

- **CVE** is a unique identifier assigned to a publicly known vulnerability.
    
- **Discovered by experts** who report issues in open-source/proprietary apps.
    
- Once confirmed, these are:
    
    - **Patched**
        
    - Given a **CVE ID**
        
    - Rated using the **CVSS scoring system**
        

#### 🔹 Example Process:

1. Identify app version (from `version.php`, repo, or source code)
    
2. Search CVEs or exploits in:
    
    - [Exploit-DB](https://www.exploit-db.com)
        
    - [Rapid7 DB](https://www.rapid7.com/db/)
        
    - [Vulnerability Lab](https://www.vulnerability-lab.com/)
        
3. Look for:
    
    - **High CVSS score** (8–10)
        
    - **Remote Code Execution (RCE)** exploits
        
4. Also check for **plugins or 3rd party component vulnerabilities**
    

---

### 🔸 3. **CVSS – Common Vulnerability Scoring System**

- An **open industry standard** used to measure the **severity** of vulnerabilities.
    
- Helps in:
    
    - Risk assessment
        
    - Prioritizing patches and mitigation
        

#### 📐 CVSS Metrics:

1. **Base** – inherent characteristics (score 0–10)
    
2. **Temporal** – changes over time (e.g., exploit availability)
    
3. **Environmental** – impact on your system/network specifically
    

---

### 🔹 CVSS Score Ranges
**CVSS V2.0**

| **Severity** | **S**Score Range** |
| ------------ | ------------------ |
| Low          | 0.0–3.9            |
| Medium       | 4.0–6.9            |
| High         | 7.0–10.0           |

---

**CVSS v3.0**

| **Severity** | **Score Range** |
| ------------ | --------------- |
| None         | 0.0             |
| Low          | 0.1–3.9         |
| Medium       | 4.0–6.9         |
| High         | 7.0–8.9         |
| **Critical** | **9.0–10.0**    |

---

### 🧮 CVSS Calculators

- **NVD** provides only **Base scores**.
    
- Use online **CVSS v2/v3 calculators** to customize based on:
    
    - Environment
        
    - Exploit maturity
        
    - System impact
        

---

### 🔸 4. **Back-End Server Vulnerabilities**

- Includes vulnerabilities in:
    
    - Web servers (e.g., Apache, NGINX, IIS)
        
    - OS-level services
        
    - Databases
        

#### ⚠️ Key Point:

- **Web servers are most critical**, as they are directly exposed to the internet.
#### Example:
- **Shellshock (2014)**:
    
    - Affected Apache servers using Bash
        
    - Allowed **Remote Code Execution** via HTTP headers
        

---

