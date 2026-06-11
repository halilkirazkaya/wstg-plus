## WSTG-INPV-19 — Testing for Server-Side Request Forgery (SSRF)

Server-Side Request Forgery occurs when an attacker can influence a web application to make unauthorized requests from the server to internal or external resources. By manipulating parameters that expect URLs, hostnames, or IP addresses, an attacker can bypass network-level controls like firewalls and ACLs to probe internal network segments, access cloud metadata services (IMDS), or interact with vulnerable internal APIs and databases. This vulnerability typically manifests in features involving image fetching, PDF generation, webhooks, or file uploads via URL. From an attacker's perspective, SSRF is a powerful primitive used to pivot into isolated environments, exfiltrate sensitive configuration data, or potentially achieve remote code execution through interaction with internal services like Redis or Memcached.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Tools:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### Does the application process user-supplied URLs or hostnames?
- [ ] No — no features accept external URLs or hostnames  
- [ ] Yes — feature exists but input **is strictly validated** against an allow-list  
- [ ] Yes — feature exists and accepts user-supplied URLs  

### Are validation controls or blacklists applied to the requested destination?
- [ ] Yes — a strict **allow-list** of trusted domains/IPs is enforced  
- [ ] Yes — a **deny-list** is used (e.g., blocking `127.0.0.1`, `169.254.169.254`)  
- [ ] No — **no validation** or filtering is applied to the input  

### Is it possible to bypass filters via encoding, redirects, or DNS rebinding?
- [ ] No — filters are robust and handle redirects/DNS resolution securely  
- [ ] Yes — bypass **is possible** using URL encoding or alternative IP formats (hex, octal)  
- [ ] Yes — bypass **is possible** via 3xx HTTP redirects to internal targets  
- [ ] Yes — bypass **is possible** via DNS rebinding attacks to resolve to internal IPs  

### Can the application reach internal metadata services or local interfaces?
- [ ] No — local network and cloud metadata endpoints are **not reachable**  
- [ ] Yes — access to localhost (`127.0.0.1`) or loopback services **is possible**  
- [ ] Yes — access to cloud metadata services (e.g., AWS/GCP/Azure IMDS) **is possible** *(Critical)*  

### What is the nature of the SSRF response?
- [ ] Blind — no response or metadata is returned to the user  
- [ ] Partial — response metadata (headers, size, timing) is returned to the user  
- [ ] Full — the full response body from the internal resource **is reflected** to the user  

---