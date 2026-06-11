## WSTG-CLNT-11 — Test Web Messaging

Web Messaging, or `postMessage`, enables cross-origin communication between window objects, such as a parent page and a spawned popup or an embedded iframe. This mechanism is critical for modern web functionality but introduces significant risk if the receiving application fails to verify the sender's identity or improperly handles the message payload. Attackers exploit these vulnerabilities by hosting a malicious site that embeds the target application and sends crafted messages to trigger unintended actions, exfiltrate sensitive data, or execute DOM-based Cross-Site Scripting (XSS). Successful exploitation occurs when message listeners do not strictly validate the `origin` property or pass `message.data` directly into dangerous execution sinks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### Does the application utilize the `postMessage` API for cross-document communication?
- [ ] No — `postMessage` listeners are **not** present in the client-side code  
- [ ] Yes — `postMessage` is used for internal component communication  
- [ ] Yes — `postMessage` is used to communicate with third-party domains or widgets  

### Is the origin of incoming messages strictly validated?
- [ ] Yes — origin is checked against a strict whitelist using `===` or identical comparison *(Most secure)*  
- [ ] Yes — origin is validated using a regex, but the logic **is** susceptible to bypass (e.g., `^https://trusted.com`)  
- [ ] No — the `origin` property is **not** checked, allowing any domain to send messages  
- [ ] No — a wildcard `*` is used in the `targetOrigin` of the sender, potentially leaking data to malicious listeners  

### Is the message payload sanitized before being processed by the application?
- [ ] Yes — data is treated as plain text and no dangerous sinks are used  
- [ ] Yes — data is parsed (e.g., `JSON.parse`) and validated before use, and bypass **not possible**  
- [ ] No — message data is passed directly into dangerous sinks like `innerHTML`, `eval()`, or `setTimeout()`  
- [ ] No — message data is used to navigate the window or change the `location.href` without validation  

### Can an attacker trigger unauthorized actions or exfiltrate data via a malicious message?
- [ ] No — even with a spoofed message, no sensitive actions or data can be accessed  
- [ ] Yes — a crafted message **can** trigger sensitive state changes (e.g., password change, profile update)  
- [ ] Yes — a crafted message **can** result in DOM-based XSS or exfiltration of CSRF tokens/session data  

---