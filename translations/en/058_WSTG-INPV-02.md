## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Stored Cross Site Scripting (XSS), also known as Persistent XSS, occurs when an application receives data from a user and stores it in a persistent database, file system, or other storage medium without adequate validation or encoding. When a victim subsequently navigates to a page that retrieves and displays this unsanitized data, the malicious script executes within their browser context under the application's origin. This vulnerability is particularly dangerous because it does not require a victim to click a specially crafted link; any user viewing the affected page becomes a target, potentially leading to session hijacking, unauthorized actions, or credential harvesting. Attackers typically target comment sections, user profiles, message boards, and administrative logs where input is consistently rendered for other users or administrators.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Tools:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Are there entry points that store user input for later display to other users?
- [ ] No — application does not store user-controllable input for later rendering  
- [ ] Yes — input is stored (e.g., profiles, comments) but is **not** rendered in an HTML context  
- [ ] Yes — input is stored and rendered to other users or administrators  

### Is output encoding or sanitization applied when stored data is rendered?
- [ ] Yes — context-aware output encoding **is applied** and bypass **not possible**  *(Most secure)*  
- [ ] Yes — sanitization (e.g., DOMPurify) **is applied** but bypass **is possible** via configuration errors  
- [ ] No — data is rendered directly into the DOM **without** encoding or sanitization *(High)*  

### Can the input validation or sanitization be bypassed using alternative payloads?
- [ ] No — filters handle various encodings, non-standard tags, and event handlers securely  
- [ ] Yes — bypass **is possible** using HTML entity encoding or specific tags (e.g., `<svg>`, `<img>`)  
- [ ] Yes — bypass **is possible** via polyglot payloads or mutation-based XSS (mXSS)  

### Does a Content Security Policy (CSP) provide a secondary layer of defense?
- [ ] Yes — a strict CSP is **enabled** and prevents execution of inline scripts and unauthorized sources  
- [ ] Yes — a CSP is **enabled** but is misconfigured (e.g., `unsafe-inline` or broad `script-src`)  
- [ ] No — no CSP is **enabled** for the application  

### Is it possible to achieve "Stored XSS to RCE" or other high-impact chains (Blind XSS)?
- [ ] No — impact is limited to the client-side browser context  
- [ ] Yes — stored XSS **can** be used to target administrators (Blind XSS) in internal panels  
- [ ] Yes — stored XSS **can** be chained with other vulnerabilities (e.g., CSRF, sensitive data exfiltration)  

---