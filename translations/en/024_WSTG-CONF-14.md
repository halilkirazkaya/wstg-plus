## WSTG-CONF-14 — Test Other HTTP Security Header Misconfigurations

Testing for HTTP security header misconfigurations involves verifying the presence and correct implementation of defensive headers that provide essential protection against common web-based attack vectors. While these headers do not fix underlying code vulnerabilities, their absence significantly increases the risk of man-in-the-middle (MITM) attacks, MIME-sniffing exploitation, and unintended cross-origin data leakage via referrers. From an attacker's perspective, missing security headers are high-visibility indicators of a poorly hardened environment, often facilitating more complex exploitation chains like session hijacking or information exfiltration. These configurations are typically evaluated at the server level and should be consistently applied across all response types, including API endpoints and static resources.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Security Header Presence Audit
| Header | Present | Missing | Recommended Configuration |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` or `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Feature-specific, e.g., `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### How are security headers enforced across the application scope?
- [ ] Yes — headers are globally applied via a central load balancer or reverse proxy and **cannot** be overridden  
- [ ] Yes — headers are applied to main document responses but are **missing** on API responses or static assets  
- [ ] No — headers are applied inconsistently or are **missing** across the entire application domain  

### Is the Strict-Transport-Security (HSTS) header correctly configured?
- [ ] Yes — `max-age` is set to a long duration (e.g., 2 years) and `includeSubDomains` is **enabled**  
- [ ] Yes — `max-age` is set but `includeSubDomains` is **disabled**, leaving subdomains vulnerable  
- [ ] No — HSTS header is **not present** or `max-age` is set to 0, allowing protocol downgrade attacks  

### Does the Referrer-Policy prevent the leakage of sensitive URL parameters?
- [ ] Yes — policy is set to `no-referrer` or `same-origin` *(Most secure)*  
- [ ] Yes — policy is set to `strict-origin-when-cross-origin`  
- [ ] No — policy is **not set** or is set to `unsafe-url`, potentially leaking tokens or sensitive PII in referer headers  

### Is the X-Content-Type-Options header preventing MIME-sniffing?
- [ ] Yes — `nosniff` directive is **present** and correctly implemented  
- [ ] No — header is **missing**, potentially allowing a browser to execute non-executable files (e.g., images) as scripts  

---