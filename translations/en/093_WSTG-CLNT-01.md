## WSTG-CLNT-01 — Testing for DOM Based Cross Site Scripting

DOM-based Cross-Site Scripting (DOM XSS) occurs when an application contains client-side JavaScript that processes data from an untrusted source in an unsafe way, usually by writing the data back to the Document Object Model (DOM) via a dangerous sink. Unlike reflected or stored XSS, the vulnerability exists entirely within the client-side code; the payload is often not sent to the server at all, as it is processed by sinks like `innerHTML`, `eval()`, or `document.write()`. Attackers exploit this by crafting URLs with malicious fragments or parameters that, when loaded by a victim's browser, execute arbitrary JavaScript in the context of the user's session. This can lead to the exfiltration of session tokens, sensitive data theft, and unauthorized actions performed on behalf of the authenticated user.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Are untrusted sources used to capture user-controlled data in JavaScript?
- [ ] No — JavaScript does **not** use `location.hash`, `location.search`, or `document.referrer`  
- [ ] Yes — untrusted sources are used but data is **not** passed to any execution sinks  
- [ ] Yes — untrusted sources are used and data flows directly into client-side logic  

### Is user-controlled data passed to dangerous DOM sinks?
- [ ] No — sinks such as `innerHTML`, `outerHTML`, `document.write()`, or `eval()` are **not** used  
- [ ] Yes — dangerous sinks are present but the data is strictly **sanitized** using a secure library like DOMPurify  
- [ ] Yes — dangerous sinks are present and data is processed with **insufficient** or custom regex-based filtering  
- [ ] Yes — dangerous sinks are present and data is passed **without** any sanitization or encoding *(Critical)*  

### Can the execution sink be reached via a URL-based payload?
- [ ] No — source-to-sink flow **cannot** be triggered via external input  
- [ ] Yes — flow is **possible** but requires complex user interaction  
- [ ] Yes — flow is **possible** and can be triggered via a simple URL fragment or query parameter  

### Are modern security headers or browser-based mitigations neutralizing the risk?
- [ ] Yes — a strict `Content-Security-Policy` (CSP) with `script-src` and `trusted-types` is **enabled** and prevents execution  
- [ ] Yes — a CSP is present but `unsafe-inline` or weak hashes make a bypass **possible**  
- [ ] No — no CSP is present to mitigate DOM-based execution  

### Does the application use client-side frameworks with built-in protections?
- [ ] Yes — framework (e.g., Angular, React) automatically encodes data and bypass is **not possible**  
- [ ] Yes — framework is used but "escape hatches" like `dangerouslySetInnerHTML` are **enabled**  
- [ ] No — the application uses vanilla JavaScript or legacy libraries (e.g., old jQuery versions) without auto-escaping  

---