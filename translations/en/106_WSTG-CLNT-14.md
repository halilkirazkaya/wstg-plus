## WSTG-CLNT-14 — Testing for Reverse Tabnabbing

Reverse Tabnabbing is a client-side vulnerability where a page opened via `target="_blank"` retains a functional reference to the parent page through the `window.opener` object. This allows an attacker-controlled destination site to programmatically redirect the original, trusted tab to a malicious URL, typically a phishing page designed to harvest credentials. The attack exploits the user's inherent trust in the original tab, as they are unlikely to notice that the page they just left has navigated to a different domain. Modern security practices require the use of the `rel="noopener"` or `rel="noreferrer"` attributes on anchor tags, or setting the `opener` property to null in JavaScript, to prevent this cross-window communication.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity is Low in most modern browsers as they now default `target="_blank"` to `rel="noopener"`. Severity becomes Medium if the application targets legacy browsers or uses `window.open()` without setting the `opener` property to null.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Are links present that open in a new window or tab?
- [ ] No — no links use `target="_blank"` or equivalent JavaScript-based redirection  
- [ ] Yes — `target="_blank"` is used exclusively on internal, trusted links  
- [ ] Yes — `target="_blank"` is used on external links or user-supplied URLs  

### Is the `window.opener` relationship restricted on anchor tags?
- [ ] Yes — `rel="noopener"` or `rel="noreferrer"` is **consistently applied** to all links with `target="_blank"` *(Most secure)*  
- [ ] Yes — attributes are applied but are **missing** on high-risk or user-generated links  
- [ ] No — attributes are **not applied**, allowing the child page to access `window.opener`  

### Is the application vulnerable to Reverse Tabnabbing via JavaScript `window.open()`?
- [ ] No — `window.open()` is not used or explicitly sets the `opener` property to null  
- [ ] Yes — `window.open()` is used and the `opener` reference **remains accessible** to the child window  

### Can an attacker control the destination of a link that opens in a new tab?
- [ ] No — all link destinations are hardcoded and point to trusted domains  
- [ ] Yes — link destinations are user-supplied (e.g., social media profiles, bio links) and **not** sanitized for tabnabbing protections  

---