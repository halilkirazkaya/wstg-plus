## WSTG-SESS-02 — Testing for Cookies Attributes

Session management often relies on HTTP cookies to maintain state, making the security attributes of these cookies a primary target for attackers. By omitting or misconfiguring attributes such as `HttpOnly`, `Secure`, and `SameSite`, applications expose session tokens to theft via Cross-Site Scripting (XSS), interception over unencrypted channels, or Cross-Site Request Forgery (CSRF) attacks. Pentesters examine these flags to determine if an attacker can exfiltrate session identifiers from the browser's document object model or force the browser to send them in cross-origin requests. Proper attribute configuration is a fundamental defense-in-depth measure to ensure that sensitive tokens remain confined to authorized, secure contexts.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analysis of Cookie Flag Implementation
| Attribute | Present | Missing |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Is the `HttpOnly` flag applied to sensitive session cookies?
- [ ] Yes — applied to **all** sensitive session cookies *(Most secure)*  
- [ ] Yes — applied only to some session cookies  
- [ ] No — flag is **not applied**, allowing access via client-side scripts *(Critical)*  

### Is the `Secure` flag enforced for cookies transmitted over HTTPS?
- [ ] Yes — applied to **all** cookies transmitted over TLS  
- [ ] No — flag is **not applied**, allowing cookies to be sent over plaintext HTTP  

### Is the `SameSite` attribute used to mitigate CSRF risks?
- [ ] Yes — set to `Strict` or `Lax` for session cookies  
- [ ] Yes — set to `None` but with the `Secure` flag present  
- [ ] No — attribute is **missing** or set to `None` **without** `Secure`  

### Are the `Domain` and `Path` attributes restricted to the minimum necessary scope?
- [ ] Yes — scoped to the **specific** subdomain and application path  
- [ ] No — scope is too **broad** (e.g., set to a parent domain or root `/`), increasing the attack surface  

### Are sensitive session cookies expired properly via the `Expires` or `Max-Age` attributes?
- [ ] Yes — appropriate expiration dates are set  
- [ ] No — cookies are persistent with excessively **long** lifetimes  
- [ ] No — cookies are session-only but **lack** server-side timeout enforcement  

---