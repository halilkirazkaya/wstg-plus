## WSTG-INPV-17 — Testing for Host Header Injection

Host Header Injection occurs when an application trusts the user-supplied `Host` header and incorporates it into server-side logic, such as link generation or redirects, without sufficient validation. Attackers manipulate this header to facilitate web cache poisoning, facilitate password reset poisoning by redirecting sensitive tokens to an external domain, or bypass security controls that rely on header-based routing. Successful exploitation allows an attacker to exfiltrate session data, perform account takeovers, or redirect unsuspecting users to malicious infrastructure. This vulnerability is most commonly found in load-balanced environments or applications that dynamically generate absolute URLs based on the incoming request context.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the Host header is used to generate sensitive links in emails, such as password resets.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Tools:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### Does the application reflect the `Host` header value in links, redirects, or scripts?
- [ ] No — application uses hardcoded base URLs or strictly validated configuration  
- [ ] Yes — `Host` header is reflected but strictly validated against a whitelist  
- [ ] Yes — `Host` header is reflected and bypass **is possible** via malformed values (e.g., `expected.com:attacker.com`)  
- [ ] Yes — `Host` header is reflected **without** any validation  

### Are alternative host-related headers processed by the application or infrastructure?
- [ ] No — headers like `X-Forwarded-Host`, `X-Host`, or `Forwarded` are ignored  
- [ ] Yes — alternative headers are processed but do **not** override internal logic  
- [ ] Yes — alternative headers **can** be used to override the primary `Host` header value  

### Can the `Host` header be manipulated to poison system-generated emails (e.g., password resets)?
- [ ] No — email links use a static, trusted domain from the server configuration  
- [ ] Yes — `Host` header influences email links but validation **cannot** be bypassed  
- [ ] Yes — `Host` header **can** be manipulated to redirect password reset tokens to an attacker-controlled domain *(Critical)*  

### Is the application susceptible to Web Cache Poisoning via the `Host` header?
- [ ] No — no caching mechanism is present or the `Host` header is part of the cache key  
- [ ] Yes — a cache is present but it strictly validates or ignores the `Host` header for unkeyed inputs  
- [ ] Yes — a cache exists and **can** be poisoned by an injected `Host` header to serve malicious content to other users  

### Does the server accept arbitrary `Host` headers for virtual host routing?
- [ ] No — server returns an error or a default page for unrecognized `Host` headers  
- [ ] Yes — server accepts any `Host` header, which **is** useful for reconnaissance or internal service enumeration  

---