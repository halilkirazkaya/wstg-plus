## WSTG-CRYP-04 — Testing for Weak Cryptographic Primitives

Weak cryptographic primitives involve the use of outdated or insecure algorithms, such as MD5, SHA-1, DES, or RC4, which are susceptible to collision attacks, frequency analysis, or brute-force decryption. These weaknesses typically occur in password hashing, data encryption at rest, session token generation, or digital signature verification within the application backend. From an attacker's perspective, identifying these primitives allows for the compromise of data integrity and confidentiality, such as cracking intercepted hashes to retrieve cleartext passwords or forging authentication tokens. Modern applications should instead utilize industry-standard, collision-resistant, and high-entropy primitives like Argon2, Bcrypt, or AES-GCM to ensure robust protection against cryptanalysis.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Are deprecated hashing algorithms such as MD5 or SHA-1 used for sensitive data storage?
- [ ] No — application uses modern collision-resistant algorithms such as Argon2 or Bcrypt  
- [ ] Yes — deprecated algorithms are used but **only** for non-security-sensitive integrity checks  
- [ ] Yes — deprecated algorithms **are used** for passwords or sensitive tokens *(High)*  

### Are weak symmetric encryption ciphers like DES, 3DES, or RC4 currently employed?
- [ ] No — AES (128-bit or higher) in a secure mode like GCM or CBC is used  
- [ ] Yes — legacy ciphers are **enabled** for backward compatibility but are **not** the default  
- [ ] Yes — legacy ciphers are the primary method for protecting data at rest or in transit  

### Does the application implement "roll-your-own" or non-standard cryptographic logic?
- [ ] No — application strictly adheres to standard, peer-reviewed cryptographic libraries  
- [ ] Yes — custom wrappers exist but the underlying primitives **are** industry standard  
- [ ] Yes — proprietary cryptographic algorithms or custom logic **is applied** to sensitive data *(Critical)*  

### Are cryptographic salts and initialization vectors (IVs) managed according to best practices?
- [ ] Yes — salts are unique per-user and IVs are unpredictable for every operation  
- [ ] Yes — salts are used but are **not** unique across all records  
- [ ] No — salts are static, hardcoded, or absent, and IVs **are reused** across multiple sessions  

### Is the bit-length of asymmetric keys (RSA, Diffie-Hellman) sufficient for modern standards?
- [ ] Yes — RSA/DH keys are 2048 bits or higher, or Elliptic Curve (ECC) is used  
- [ ] No — keys are less than 2048 bits but bypass **is not** trivial  
- [ ] No — keys are 1024 bits or smaller, making them **vulnerable** to factorization or computational attacks  

---