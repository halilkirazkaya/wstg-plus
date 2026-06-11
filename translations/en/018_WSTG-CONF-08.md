## WSTG-CONF-08 — Test RIA Cross Domain Policy

Rich Internet Application (RIA) cross-domain policies involve files like `crossdomain.xml` and `clientaccesspolicy.xml` which define how web clients—specifically Adobe Flash and Microsoft Silverlight—handle cross-origin requests. These files are critical for security because they explicitly override the Same-Origin Policy (SOP), determining which external domains are permitted to interact with the application and read its responses. Overly permissive configurations, such as using wildcards in the `domain` attribute, allow attackers to host malicious RIA objects on a third-party site that can exfiltrate sensitive data or perform actions on behalf of authenticated users. Pentesters typically locate these files at the web root to evaluate the level of trust granted to third-party origins and identify potential cross-origin data leaks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the application handles sensitive user data or session identifiers that can be exfiltrated via a permissive policy.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Are cross-domain policy files (`crossdomain.xml` or `clientaccesspolicy.xml`) present in the web root?
- [ ] No — files **cannot** be found in the root directory  
- [ ] Yes — `crossdomain.xml` (Flash) **is** present  
- [ ] Yes — `clientaccesspolicy.xml` (Silverlight) **is** present  
- [ ] Yes — both RIA policy files **are** present  

### Is the `allow-access-from` domain attribute restricted to trusted origins?
- [ ] No — files exist but contain no `allow-access-from` entries  
- [ ] Yes — specific, trusted domains are explicitly whitelisted and bypass **not possible**  
- [ ] Yes — domains are whitelisted but include untrusted third-party or sub-domains  
- [ ] Yes — a wildcard `*` **is used**, allowing access from **any** origin *(Critical)*  

### Does the policy permit insecure communication over HTTP for HTTPS resources?
- [ ] No — `secure="true"` is set (or defaulted), preventing HTTP origins from accessing HTTPS data  
- [ ] Yes — `secure="false"` is explicitly set, allowing MITM attackers to exfiltrate secure data  

### Are HTTP headers overly permissive in the cross-domain configuration?
- [ ] No — `allow-http-request-headers-from` is restricted or **not enabled**  
- [ ] Yes — specific headers are allowed for trusted domains  
- [ ] Yes — wildcard `*` is used for headers, allowing attackers to send custom headers from any domain  

### Is the `site-control` (master-policy) configuration restrictive?
- [ ] No — `permitted-cross-domain-policies` is set to `none` or `master-only`  
- [ ] Yes — the policy allows other files on the server to define their own rules (`all`)  

---