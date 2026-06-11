## WSTG-CLNT-09 — Testing for Clickjacking

Clickjacking, also known as UI redressing, occurs when an attacker uses transparent or opaque layers to trick a user into clicking on a button or link on another page when they were intending to click on the top-level page. This vulnerability compromises the integrity of user actions, potentially leading to unauthorized data modification, account changes, or financial transfers performed on behalf of the victim. Attackers typically exploit this by embedding the target website within an `<iframe>` on a controlled malicious site and overlaying it with deceptive content or enticing lures. It occurs most frequently on pages performing sensitive state-changing actions such as "Delete Account" buttons, password reset forms, or administrative configuration panels.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if sensitive actions (e.g., fund transfers, account deletion, or privilege escalation) can be triggered via clickjacking.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Tools:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Does the application implement server-side headers to prevent unauthorized framing?
- [ ] Yes — `X-Frame-Options` or `Content-Security-Policy` **are applied** and prevent framing *(Most secure)*  
- [ ] Yes — headers are present but **misconfigured** (e.g., `X-Frame-Options: ALLOW-FROM` which lacks broad browser support)  
- [ ] No — no framing protection headers **are applied**  

### Is the `Content-Security-Policy` (CSP) `frame-ancestors` directive implemented?
- [ ] Yes — `frame-ancestors 'none'` or `'self'` **is applied**  
- [ ] Yes — `frame-ancestors` is present but **allows** untrusted or overly broad origins  
- [ ] No — directive is **missing** or CSP is not implemented  

### Is the `X-Frame-Options` (XFO) header correctly configured for legacy browser support?
- [ ] Yes — `DENY` or `SAMEORIGIN` **is applied**  
- [ ] Yes — XFO is present but **ignored** by modern browsers because a CSP `frame-ancestors` directive exists  
- [ ] No — XFO header is **missing** or set to an insecure value  

### Can the framing protections be bypassed using known techniques?
- [ ] No — bypass **not possible** via standard techniques (double-framing, etc.)  
- [ ] Yes — protection **can** be bypassed due to weak regex or logic in the `frame-ancestors` white-list  
- [ ] Yes — protection **can** be bypassed because the application relies solely on JavaScript-based frame-busting  

### Is a sensitive action exploitable via a clickjacking proof-of-concept?
- [ ] No — framing is blocked or no sensitive actions are reachable  
- [ ] Yes — UI redressing **is possible** but requires complex user interaction  
- [ ] Yes — UI redressing **is possible** and allows for immediate state-changing actions *(Critical)*  

---