## WSTG-CLNT-04 — Testing for Client-Side URL Redirect

Client-side URL redirection occurs when a web application accepts user-controlled input—typically through URL parameters or hash fragments—and uses it within a client-side redirection sink like `window.location` or `document.location.href`. This vulnerability is primarily leveraged by attackers to conduct sophisticated phishing campaigns, as the victim initially trusts the legitimate domain before being silently redirected to a malicious site. Beyond phishing, client-side redirects can often be chained to exfiltrate sensitive data such as OAuth access tokens or session identifiers if the redirection happens while those values are present in the URL or the `Referer` header. In modern Single Page Applications (SPAs), this vulnerability frequently appears in routing logic, "return to" parameters after authentication, or language-switching functionalities.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Test Status** | Not Performed |
| **Severity** | Medium* |

> *Severity becomes High if the redirect can be used to exfiltrate OAuth tokens, session IDs, or bypass security controls in an authentication flow.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Does the application use client-side scripts to handle redirects based on user input?
- [ ] No — redirection is handled exclusively server-side via `3xx` HTTP status codes  
- [ ] Yes — application uses JavaScript sinks like `window.location` to navigate based on URL parameters  
- [ ] Yes — application uses a client-side framework router (e.g., React Router, Vue Router) that processes user-supplied paths  

### Are input validation or whitelisting controls implemented for the destination URL?
- [ ] Yes — a strict **whitelist** of allowed domains/paths is enforced and bypass **not possible**  
- [ ] Yes — validation is present but relies on weak regex or blacklists, and bypass **is possible**  
- [ ] No — application accepts any arbitrary string as a redirection target  

### Can the redirect logic be bypassed using common obfuscation techniques?
- [ ] No — controls correctly handle encoded characters, protocol-relative URLs (`//`), and backslashes  
- [ ] Yes — bypass **is possible** using URL encoding or double-encoding  
- [ ] Yes — bypass **is possible** using protocol-relative URLs or `@` characters (e.g., `https://expected.com@attacker.com`)  
- [ ] Yes — bypass **is possible** using specific browser quirks or null byte injection  

### Does the redirection process expose sensitive data to the destination site?
- [ ] No — no sensitive data is present in the URL or transmitted via headers during redirect  
- [ ] Yes — sensitive data (e.g., session tokens, API keys) **is leaked** via the `Referer` header to the external site  
- [ ] Yes — sensitive data present in the URL fragment (`#`) **is accessible** to the destination site's scripts  

### Is it possible to execute JavaScript via the redirect sink (DOM-based XSS)?
- [ ] No — sink only allows navigation to `http` or `https` protocols  
- [ ] Yes — the redirect sink allows `javascript:` or `data:` URIs, making DOM-based XSS **possible**  

---