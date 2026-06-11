## WSTG-BUSL-03 — Test Integrity Checks

Integrity check testing focuses on verifying that the application ensures data remains unaltered and trustworthy as it moves through various business processes or between the client and server. Attackers target this logic by tampering with critical parameters—such as product prices, quantities, user privileges, or transaction states—within HTTP requests, hidden fields, or cookies. If the application lacks server-side verification or robust integrity controls like Hashing or HMACs, an adversary can manipulate these values to commit fraud, escalate their own authority, or bypass intended business constraints. This test is vital for any application handling financial transactions, sensitive state transitions, or multi-step workflows where the output of one stage serves as the trusted input for the next.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the integrity failure allows for financial theft, unauthorized privilege escalation, or modification of persistent records.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Are sensitive business parameters exposed to client-side tampering?
- [ ] No — sensitive values are managed exclusively on the server-side  
- [ ] Yes — values like price, quantity, or status flags **are** exposed but encrypted  
- [ ] Yes — values **are** exposed in plain text within hidden fields, URL parameters, or cookies  

### Is an integrity mechanism (e.g., HMAC, Checksum) applied to client-controlled parameters?
- [ ] Yes — a robust integrity check **is applied** and verified on every request  
- [ ] Yes — an integrity check **is applied** but uses a weak/predictable algorithm or hardcoded secret  
- [ ] No — no integrity checks **are applied** to sensitive parameters  

### Can the integrity check be bypassed or recalculated by an attacker?
- [ ] No — signature verification is strictly enforced and bypass **not possible**  
- [ ] Yes — signature verification can be bypassed by omitting the signature parameter  
- [ ] Yes — the signature **can** be recalculated because the secret key or logic is exposed/weak  

### Does the application perform backend re-validation of data against a trusted source?
- [ ] Yes — the server re-validates all values against the database and bypass **not possible**  
- [ ] Yes — the server performs partial validation but bypass **is possible** through specific edge cases  
- [ ] No — the server trusts client-provided data without backend verification *(Critical)*  

### Is it possible to tamper with the state of a multi-step process?
- [ ] No — session state is managed server-side and cannot be manipulated  
- [ ] Yes — state information is passed via the client and tampering **is possible** to skip steps or repeat actions  

---