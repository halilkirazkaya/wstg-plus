## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Reflected Cross Site Scripting (XSS) occurs when an application includes untrusted data in an HTTP response without sufficient validation or encoding, causing the payload to be executed in the victim's browser context. Attackers deliver malicious payloads via social engineering, typically through crafted URLs or forms, to hijack user sessions, exfiltrate sensitive cookies, or perform unauthorized actions on behalf of the user. This vulnerability is commonly found in search parameters, error messages, and any endpoint that reflects input directly back to the user interface. Exploitation relies on the victim interacting with a malicious link, making it a non-persistent but highly effective attack vector for targeting specific administrative or authenticated users.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Are user-supplied inputs reflected in the response body?
- [ ] No — input is **never** reflected back to the user  
- [ ] Yes — input is reflected but properly encoded and bypass **not possible**  
- [ ] Yes — input is reflected **without** any encoding or sanitization  

### Is context-aware output encoding implemented?
- [ ] Yes — proper encoding (HTML, Attribute, JavaScript) is **applied** based on the specific reflection context  
- [ ] Yes — encoding is **applied** but is insufficient for the specific context (e.g., HTML encoding inside a `<script>` tag)  
- [ ] No — encoding is **not applied**  

### Can input validation or Web Application Firewall (WAF) filters be bypassed?
- [ ] No — filters effectively block all common and obfuscated XSS payloads  
- [ ] Yes — filters are present but bypass **is possible** using character encoding, non-standard tags, or polyglots  
- [ ] No — no filters or validation mechanisms are in place  

### What is the execution context of the reflected input?
- [ ] Safe — input is reflected in a non-executable location (e.g., within standard `<div>` or `<span>` tags)  
- [ ] Risky — input is reflected inside HTML attributes or within the `src`/`href` of tags  
- [ ] Critical — input is reflected directly inside `<script>` blocks, event handlers, or template literals  

### Are modern browser security headers present to mitigate XSS impact?
- [ ] Yes — `Content-Security-Policy` (CSP) is **enabled** with a restrictive script-src  
- [ ] Yes — `Content-Security-Policy` (CSP) is **enabled** but contains `unsafe-inline` or weak whitelists  
- [ ] No — no CSP or legacy `X-XSS-Protection` headers are present  

---