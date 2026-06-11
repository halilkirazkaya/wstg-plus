## WSTG-CRYP-02 — Testing for Padding Oracle

Padding Oracle vulnerabilities occur when an application's decryption process leaks information about whether the padding of a decrypted ciphertext is valid or invalid. By observing these side-channel responses—such as distinct HTTP status codes, specific error messages, or response timing variations—an attacker can decrypt data without knowing the encryption key and potentially re-encrypt arbitrary plaintext. This vulnerability typically manifests in encrypted session cookies, authentication tokens, or URL parameters using block ciphers in Cipher Block Chaining (CBC) mode with PKCS#7 padding. Exploitation allows for complete loss of confidentiality and can lead to integrity compromise through the construction of forged encrypted blocks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical* |

> *Severity is Critical if the vulnerability resides in session management, identity tokens, or PII encryption.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Is a block cipher used in CBC mode for sensitive data?
- [ ] No — application uses authenticated encryption (AES-GCM/ChaCha20-Poly1305)  
- [ ] Yes — application uses block ciphers but padding **is not** required (e.g., Stream ciphers or CTR mode)  
- [ ] Yes — application uses CBC mode and padding **is applied**  

### Does the server disclose padding validity through side channels?
- [ ] No — server returns uniform error messages and consistent response times  
- [ ] Yes — server returns different HTTP status codes (e.g., 200 vs 500) for padding errors  
- [ ] Yes — server returns specific error messages (e.g., "Invalid Padding", "Decryption failed")  
- [ ] Yes — server exhibits measurable timing differences during decryption failure  

### Is automated decryption of ciphertext possible?
- [ ] No — side channels are **not** stable enough for exploitation  
- [ ] Yes — ciphertext **can** be partially decrypted (last few blocks)  
- [ ] Yes — full plaintext recovery **is possible** using tools like `PadBuster`  

### Can the attacker perform bit-flipping or forge valid ciphertexts?
- [ ] No — integrity checks (HMAC) are verified **before** decryption  
- [ ] Yes — no integrity check is present, but payload construction **is not** feasible  
- [ ] Yes — arbitrary ciphertext construction **is possible**, allowing for privilege escalation or data injection  

---