## WSTG-ATHZ-05 — Testing for OAuth Weaknesses

OAuth weaknesses encompass a variety of flaws in the implementation of the OAuth 2.0 or OpenID Connect (OIDC) protocols, often resulting in complete account takeover or unauthorized data access. These vulnerabilities typically manifest during the authorization flow, specifically concerning the validation of the `redirect_uri`, the entropy and verification of the `state` parameter, and the secure handling of access or ID tokens. Attackers exploit these by intercepting authorization codes, redirecting tokens to attacker-controlled domains via open redirects, or performing Cross-Site Request Forgery (CSRF) to link a victim's session to an attacker's account. Because OAuth often serves as a primary authentication mechanism, a single misconfiguration can compromise the integrity of the entire user identity system.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Tools:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Is the `state` parameter implemented and validated to prevent CSRF?
- [ ] Yes — `state` is mandatory, unique per session, and strictly validated on the server  
- [ ] Yes — `state` is present but is static or predictable across different sessions  
- [ ] Yes — `state` is present but the application **does not** validate it upon callback  
- [ ] No — `state` parameter is **not** used in the authorization request *(Critical)*  

### Is the `redirect_uri` strictly validated against a whitelist?
- [ ] Yes — only exact matches against a pre-registered whitelist are **accepted**  
- [ ] Yes — validation is applied but bypass **is possible** via path traversal or subdomain manipulation  
- [ ] Yes — validation is applied but bypass **is possible** via parameter pollution or fragment injection  
- [ ] No — application **allows** arbitrary URLs in the `redirect_uri` parameter *(Critical)*  

### Can the `response_type` be manipulated to alter the flow?
- [ ] No — application only accepts the intended flow (e.g., `code`) and rejects others  
- [ ] Yes — switching from `code` to `token` (Implicit flow) **is possible** and leaks tokens in the URL  
- [ ] Yes — hybrid flows or unauthorized `response_type` values are **enabled** and processed  

### Are access tokens or authorization codes leaked via Referer headers or browser history?
- [ ] No — tokens/codes are handled securely and sensitive pages use appropriate `Referrer-Policy`  
- [ ] Yes — tokens/codes **are** leaked to third-party domains via the `Referer` header  
- [ ] Yes — tokens/codes **are** visible in the browser history due to insecure caching or GET requests  

### Does the application enforce the "Least Privilege" principle for OAuth scopes?
- [ ] Yes — application requests only the minimum scopes necessary for the functionality  
- [ ] No — application requests excessive or administrative scopes by default  
- [ ] No — user **cannot** review or opt-out of specific scopes during the consent phase  

---