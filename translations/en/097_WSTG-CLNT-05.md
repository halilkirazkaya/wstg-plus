## WSTG-CLNT-05 — Testing for CSS Injection

CSS Injection occurs when an application allows user-supplied input to influence the Cascading Style Sheets (CSS) of a web page without proper sanitization or escaping. While often perceived as a cosmetic issue, attackers can leverage CSS injection to exfiltrate sensitive data, such as CSRF tokens or session IDs, by using attribute selectors and background-image properties to trigger external requests. This vulnerability typically occurs in endpoints where user preferences (e.g., custom themes, font colors) are reflected inside `<style>` blocks or inline `style` attributes. From an attacker's perspective, successful exploitation can lead to UI redressing, phishing through unauthorized layout modification, or stealthy data exfiltration in environments where strict Content Security Policies might otherwise block JavaScript-based attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the injection point allows the exfiltration of sensitive attributes like CSRF tokens or if it can be used for UI redressing in sensitive workflows.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Does the application reflect user-controlled input within CSS contexts?
- [ ] No — input is **not** reflected in style tags, attributes, or CSS files  
- [ ] Yes — input is reflected but strictly limited to safe alphanumeric values  
- [ ] Yes — input is reflected within a `style` attribute or `<style>` block  

### Are CSS-specific metacharacters and functions properly sanitized?
- [ ] Yes — characters such as `{`, `}`, `:`, and functions like `url()` are **properly escaped**  
- [ ] Yes — sanitization is in place but bypass **is possible** via encoding or CSS comments  
- [ ] No — no sanitization is applied to CSS metacharacters  

### Is data exfiltration possible using CSS selectors?
- [ ] No — selectors **cannot** trigger external requests or leak data  
- [ ] Yes — partial data exfiltration **is possible** via attribute selectors and `background-image`  
- [ ] Yes — full exfiltration of sensitive tokens **is possible** using automated brute-force techniques  

### Does a Content Security Policy (CSP) mitigate the risk of CSS injection?
- [ ] Yes — `style-src` is restricted to 'self' and `img-src` or `connect-src` **is restricted**  
- [ ] Yes — CSP is present but uses 'unsafe-inline' or allows wildcard external domains  
- [ ] No — no CSP is implemented to prevent external resource loading via CSS  

### Can the vulnerability be used to perform UI redressing or phishing?
- [ ] No — injection scope is too limited to modify the layout significantly  
- [ ] Yes — UI modification **is possible**, allowing for the overlay of malicious elements  
- [ ] Yes — full page layout control **is possible**, facilitating highly convincing phishing attacks  

---