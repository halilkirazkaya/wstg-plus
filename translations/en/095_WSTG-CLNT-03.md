## WSTG-CLNT-03 — HTML Injection

HTML Injection occurs when an application fails to sanitize user-supplied input before rendering it within the browser, allowing an attacker to inject arbitrary HTML tags into the web page's Document Object Model (DOM). While similar to Cross-Site Scripting (XSS), the primary objective of HTML Injection is to manipulate the visual presentation of the page or to facilitate phishing attacks by injecting malicious forms and deceptive content. Attackers target parameters that are reflected in the UI, such as search terms, user profile fields, or error messages, to trick victims into disclosing sensitive credentials or interacting with unauthorized third-party links. This vulnerability represents a significant risk to the integrity of the application's user interface and can be a precursor to more complex social engineering or session-related attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Are user-controlled inputs reflected in the DOM without proper HTML encoding?
- [ ] No — all reflected input is strictly HTML-encoded before being rendered  
- [ ] Yes — some characters **are** encoded but bypasses for certain tags **are possible**  
- [ ] Yes — input is reflected raw, and HTML injection **is possible**  

### Can structural HTML elements be injected to alter the page layout?
- [ ] No — the application filters or strips structural tags like `<div>`, `<table>`, or `<iframe>`  
- [ ] Yes — structural elements **can** be injected but do **not** significantly impact the UI  
- [ ] Yes — significant UI defacement **is possible** via injected tags  

### Is it possible to inject functional phishing elements like forms or malicious links?
- [ ] No — `<form>`, `<input>`, and `<a>` tags are effectively blocked or sanitized  
- [ ] Yes — malicious links **can** be injected to redirect users to external sites  
- [ ] Yes — functional `<form>` elements **can** be injected to harvest credentials *(Medium)*  

### Do security headers or Content Security Policies (CSP) mitigate the impact of injection?
- [ ] Yes — a restrictive CSP is in place and `form-action` or `frame-src` **is applied**  
- [ ] No — security headers are present but the CSP **is disabled** or contains unsafe-inline/wildcards  
- [ ] No — no CSP or relevant security headers **are enabled**  

---