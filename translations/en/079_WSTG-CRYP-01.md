## WSTG-CRYP-01 — Testing for Weak Transport Layer Security

Testing for weak transport layer security involves analyzing the implementation and configuration of SSL/TLS to ensure data confidentiality and integrity during transit. Attackers target misconfigurations such as outdated protocols (SSLv2, TLS 1.0), weak cipher suites (NULL, RC4, or anonymous), and invalid or weakly signed certificates to perform Man-in-the-Middle (MitM) attacks. This test typically targets the application's web server or load balancer endpoints where encrypted communication is established. Successful exploitation allows an adversary to intercept sensitive data, hijack user sessions, or downgrade connections to insecure states through protocol downgrade or traffic decryption.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium* |

> *Severity is High if sensitive data (PII, credentials, session tokens) is transmitted; Medium for general informational traffic.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Are outdated or insecure TLS/SSL protocols supported?
- [ ] No — only TLS 1.2 and TLS 1.3 are **enabled**  
- [ ] Yes — TLS 1.0 or TLS 1.1 are **enabled**  
- [ ] Yes — SSLv2 or SSLv3 are **enabled** *(Critical)*  

### Are weak or insecure cipher suites accepted by the server?
- [ ] No — only strong, modern ciphers (e.g., AES-GCM, ChaCha20) are **supported**  
- [ ] Yes — ciphers with weak encryption (e.g., 3DES, RC4) or low bit-length are **supported**  
- [ ] Yes — NULL, anonymous (ADH), or export ciphers are **supported** *(Critical)*  

### Is the server certificate valid and trusted?
- [ ] Yes — certificate is valid, matches the domain, and is signed by a **trusted CA**  
- [ ] No — certificate is **expired** or uses a **weak signature algorithm** (e.g., SHA-1)  
- [ ] No — certificate is **self-signed** or domain name **mismatch** occurs  

### Does the server support Secure Renegotiation and prevent Downgrade attacks?
- [ ] Yes — Secure Renegotiation is **supported** and TLS Fallback SCSV is **enabled**  
- [ ] No — Secure Renegotiation is **not supported**, potentially allowing MitM injection  
- [ ] No — Server is vulnerable to **POODLE** or **ROBOT** attacks  

### Is HTTP Strict Transport Security (HSTS) properly implemented?

| Header | Present | Missing |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Yes — HSTS header is present with a long `max-age` and `includeSubDomains` **enabled**  
- [ ] Yes — HSTS header is present but missing `includeSubDomains` or has a **short max-age**  
- [ ] No — HSTS header is **not present**  

---