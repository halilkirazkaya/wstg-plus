## WSTG-CONF-12 — Content Security Policy (CSP)

Content Security Policy (CSP) is a critical defense-in-depth mechanism implemented via HTTP response headers to mitigate risks such as Cross-Site Scripting (XSS), clickjacking, and data injection attacks. By defining which dynamic resources are allowed to load, a well-configured CSP restricts an attacker's ability to execute unauthorized scripts or exfiltrate sensitive data to external domains. Pentesters evaluate the policy for overly permissive directives, the use of unsafe keywords like `'unsafe-inline'`, and reliance on insecure CDNs that might facilitate bypasses. A weak or missing CSP does not create a vulnerability directly but significantly increases the impact of successful injection attacks by failing to provide a secondary layer of protection.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Tools:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Is a Content Security Policy (CSP) header present in the application responses?
- [ ] Yes — `Content-Security-Policy` header is **present** and **enforced**  
- [ ] Yes — `Content-Security-Policy-Report-Only` is **present** for testing  
- [ ] No — no CSP header is **present** in the responses  

### Are the `script-src` or `default-src` directives properly restricted?
- [ ] Yes — directives use strict whitelisting, nonces, or hashes and bypass **not possible**  
- [ ] Yes — directives are **properly configured** but whitelist includes known bypassable CDNs  
- [ ] No — directives use wildcards (`*`) or `data:` schemes, making bypass **possible**  

### Does the policy allow the execution of unsafe inline scripts or eval?
- [ ] No — `'unsafe-inline'` and `'unsafe-eval'` are **not present**  
- [ ] Yes — `'unsafe-inline'` is **present** but protected by a `nonce` or `hash`  
- [ ] Yes — `'unsafe-inline'` or `'unsafe-eval'` are **enabled** without additional protections *(Critical)*  

### Is the application protected against clickjacking via the `frame-ancestors` directive?
- [ ] Yes — `frame-ancestors` is set to `'none'` or `'self'`  
- [ ] Yes — `frame-ancestors` is **enabled** but allows specific trusted third-party domains  
- [ ] No — `frame-ancestors` is **missing**, relying on legacy `X-Frame-Options` or no protection  

### Is there a mechanism to report CSP violations?
- [ ] Yes — `report-uri` or `report-to` is **configured** and active  
- [ ] No — violation reporting is **disabled** or **not configured**  

---