## WSTG-CLNT-06 — Testing for Client-Side Resource Manipulation

Client-side resource manipulation occurs when an application allows user-controlled input to influence the source or path of resources loaded by the browser, such as scripts, stylesheets, or iframes. By manipulating URI fragments, query parameters, or other DOM sources, an attacker can coerce the application into loading malicious external content or executing unauthorized code. This vulnerability is typically found in JavaScript logic that dynamically populates the `src` or `href` attributes of HTML elements without sufficient validation against a trusted allow-list. From an attacker's perspective, this is exploited by crafting a URL that redirects resource loading to an attacker-controlled server, potentially resulting in DOM-based Cross-Site Scripting (XSS), sensitive data exfiltration, or UI redressing.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### Does the application use client-side sinks to dynamically load or embed resources?
- [ ] No — resources are loaded via static HTML or server-side logic only  
- [ ] Yes — client-side JavaScript dynamically populates resource attributes like `src`, `href`, or `data`  

### Are validation controls or allow-lists implemented for resource URI sources?
- [ ] Yes — strict allow-lists and origin validation **are applied** to all dynamic resources  
- [ ] Yes — validation is present but relies on weak blacklists or easily bypassed regex  
- [ ] No — no validation controls are applied to user-controlled resource paths  

### Is it possible to bypass URI validation using alternative protocols or encoding?
- [ ] No — bypass of resource filters is **not possible**  
- [ ] Yes — bypass **is possible** using different protocols such as `data:`, `vbscript:`, or `javascript:`  
- [ ] Yes — bypass **is possible** using encoded characters or null bytes to circumvent simple string matches  

### Can an attacker redirect resource loading to an external, untrusted domain?
- [ ] No — resource paths are restricted to the same origin or a fixed subdirectory  
- [ ] Yes — the application **can** be forced to load scripts or styles from an arbitrary external domain  

### What is the maximum impact of the resource manipulation?
- [ ] Low — manipulation only affects non-executable elements like images or non-sensitive text  
- [ ] Medium — manipulation allows for CSS injection or significant UI redressing  
- [ ] High — manipulation leads to DOM-based XSS or execution of arbitrary JavaScript *(Critical)*  

---