## WSTG-CONF-10 — Subdomain Takeover

Subdomain takeover occurs when a DNS entry (typically a CNAME, but occasionally A or MX records) points to a decommissioned or non-existent resource on a third-party cloud provider or hosting service. Attackers identify these "dangling" DNS records by looking for specific error messages from providers like AWS, GitHub, or Azure that indicate a resource is no longer claimed. By registering the abandoned resource name on the provider's platform, the attacker gains full control over the subdomain, allowing them to host malicious content, exfiltrate session cookies scoped to the base domain, and bypass Content Security Policy (CSP) or CORS protections. This vulnerability is particularly dangerous because it leverages the trust associated with the organization's legitimate domain structure to facilitate high-impact phishing and data theft.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical* |

> *Severity becomes Critical if the subdomain can be used to hijack session cookies from the base domain or bypass SSO/OAuth redirects.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Tools:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### Does the application utilize third-party hosting or cloud services via subdomains?
- [ ] No — all subdomains point to organization-controlled IP space  
- [ ] Yes — subdomains point to third-party providers (S3, GitHub Pages, Heroku, etc.)  

### Are there any "dangling" DNS records pointing to non-existent resources?
- [ ] No — all identified DNS records resolve to active, controlled resources  
- [ ] Yes — CNAME or A records exist for resources that **no longer exist** at the provider  
- [ ] Yes — DNS records resolve to NXDOMAIN or provider-specific error pages *(e.g., "NoSuchBucket")*  

### Can the identified dangling resource be successfully claimed?
- [ ] No — provider has protections against claiming abandoned names or account verification is required  
- [ ] Yes — the resource name **can** be registered on the third-party platform by any user  

### Does the base domain use "Domain" scoped cookies that could be compromised?
- [ ] No — cookies are strictly scoped to specific subdomains or use `Host-Only` flags  
- [ ] Yes — sensitive cookies (session IDs, JWTs) are scoped to the parent domain and **can** be exfiltrated via a hijacked subdomain  

### Can the subdomain be used to bypass security headers or authentication flows?
- [ ] No — no security dependencies on the identified subdomains  
- [ ] Yes — the subdomain is whitelisted in CSP `script-src` or `connect-src` directives  
- [ ] Yes — the subdomain is a valid redirect URI for OAuth or SSO configurations  

---