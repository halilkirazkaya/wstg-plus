## WSTG-SESS-10 — Testing JSON Web Tokens

JSON Web Tokens (JWTs) are commonly used for stateless session management and identity propagation, but their security relies entirely on the correct implementation of signing algorithms and strict claim verification. Attackers frequently attempt to bypass authentication by exploiting "none" algorithm flaws, brute-forcing weak HS256 secret keys, or performing algorithm-switching attacks where an asymmetric public key is used as a symmetric HMAC secret. These vulnerabilities typically manifest in session cookies or Authorization headers, potentially allowing an attacker to escalate privileges or impersonate any user by modifying claims such as `role` or `user_id` without invalidating the token's cryptographic integrity.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Tools:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### Is the "none" algorithm accepted by the server?
- [ ] No — server **rejects** tokens using `alg: none` in the header  
- [ ] Yes — server **accepts** tokens with `alg: none` and an empty signature *(Critical)*  

### How is the JWT signature verified and protected against brute-force?
- [ ] Yes — strong asymmetric keys (RS256/ES256) or high-entropy HMAC secrets are used  
- [ ] Yes — HS256 is used but the secret **can** be brute-forced due to low entropy or default values  
- [ ] No — signature verification **is not applied** or **can** be bypassed via algorithm switching (RS256 to HS256)  

### Are standard claims (exp, nbf, iat) strictly enforced by the backend?
- [ ] Yes — `exp` (expiration) is present and **cannot** be bypassed  
- [ ] Yes — `exp` is present but the server **does not** enforce it during validation  
- [ ] No — expiration claims are **missing** or ignored, allowing for indefinite session replay  

### Does the implementation prevent header-based key injection (kid, jku, x5u)?
- [ ] Yes — headers are sanitized and only trusted internal keys/sources are **allowed**  
- [ ] No — the `kid` (Key ID) header is vulnerable to directory traversal or SQL injection to point to a known file  
- [ ] No — the `jku` or `x5u` headers **can** be pointed to attacker-controlled URLs to load malicious keys  

---