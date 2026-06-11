## WSTG-CRYP-03 — Testing for Sensitive Information Sent Via Unencrypted Channels

Transmitting sensitive data via unencrypted channels, such as plain HTTP, exposes information to interception and modification via Man-in-the-Middle (MITM) attacks. This vulnerability typically occurs when web applications fail to enforce Transport Layer Security (TLS) across all endpoints, particularly in legacy components, login forms, or during the initial transition from HTTP to HTTPS. From an attacker's perspective, this allows for the passive sniffing of authentication credentials, session tokens, and personally identifiable information (PII) on compromised or untrusted network segments like public Wi-Fi. Ensuring that all sensitive data is encapsulated within a robust TLS tunnel is fundamental to maintaining the confidentiality and integrity of user interactions and preventing session hijacking.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium |

> *Severity becomes High if authentication credentials, session identifiers, or PII are transmitted in cleartext.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### Does the application allow communication over unencrypted HTTP for sensitive endpoints?
- [ ] No — all application traffic is strictly forced over HTTPS *(Most secure)*  
- [ ] Yes — non-sensitive pages are accessible over HTTP but sensitive endpoints **are not**  
- [ ] Yes — sensitive endpoints (login, checkout, profile) **are** accessible over unencrypted HTTP *(High Risk)*  

### Is there a global redirection from HTTP to HTTPS?
- [ ] Yes — application performs a 301/302 redirect to HTTPS for all HTTP requests  
- [ ] Yes — redirection is only performed for specific sensitive endpoints  
- [ ] No — no redirection from HTTP to HTTPS **is applied**  

### Is HTTP Strict Transport Security (HSTS) implemented?
- [ ] Yes — HSTS is **enabled** with a long `max-age` and includes the `includeSubDomains` and `preload` directives  
- [ ] Yes — HSTS is **enabled** but lacks the `includeSubDomains` or `preload` directives  
- [ ] No — HSTS is **not enabled** on the application server  

### Are sensitive cookies protected from cleartext transmission?

| Cookie Name | Secure Flag Present | Secure Flag Missing |
|-------------|:-------------------:|:------------------:|
| `SessionID` |                     |                    |
| `AuthToken` |                     |                    |
| `CSRF-Token`|                     |                    |

### Does the application transmit sensitive data to third-party APIs via unencrypted channels?
- [ ] No — all external API calls are performed over encrypted (HTTPS) tunnels  
- [ ] Yes — some non-sensitive telemetry is sent via HTTP  
- [ ] Yes — API keys, PII, or credentials **are** sent to third parties via unencrypted HTTP  

---