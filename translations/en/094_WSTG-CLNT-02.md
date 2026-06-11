## WSTG-CLNT-02 — Testing for JavaScript Execution

JavaScript execution testing focuses on identifying instances where an application improperly handles user-supplied data, leading to the execution of unauthorized scripts within the victim's browser context. This vulnerability typically results in Cross-Site Scripting (XSS), which allows attackers to exfiltrate session cookies, manipulate the Document Object Model (DOM), or perform actions on behalf of the user. Penetration testers examine sinks such as `innerHTML`, `document.write()`, and `eval()` where unsanitized data from sources like URL fragments, `localStorage`, or `window.name` may be processed. Successful exploitation compromises the integrity and confidentiality of the user's session and can serve as a pivot for more complex client-side attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Tools:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### Does the application process user-supplied data in dangerous JavaScript sinks?
- [ ] No — all data is sanitized or used in safe sinks like `textContent`  
- [ ] Yes — data is processed in dangerous sinks but sanitization/encoding **is applied**  
- [ ] Yes — data is processed in dangerous sinks and sanitization **is not applied**  

### Is a Content Security Policy (CSP) present to mitigate script execution?
- [ ] Yes — a restrictive CSP is **enabled** and bypass **is not possible**  
- [ ] Yes — a CSP is **enabled** but bypass **is possible** via `unsafe-inline` or insecure CDNs  
- [ ] No — no CSP is **enabled**  

### Can JavaScript be executed via URL parameters or fragments (DOM-based XSS)?
- [ ] No — input is properly encoded and execution **is not possible**  
- [ ] Yes — input is reflected in the DOM but execution **is prevented** by modern framework protections  
- [ ] Yes — input is directly executed in the browser, and exploitation **is possible**  

### Are event handlers or URI schemes vulnerable to script injection?
- [ ] No — input is sanitized to remove event attributes and `javascript:` schemes  
- [ ] Yes — event handlers **can** be injected but filters limit payload complexity  
- [ ] Yes — arbitrary JavaScript execution via `onerror`, `onload`, or `href` attributes **is possible**  

---