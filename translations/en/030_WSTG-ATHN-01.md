## WSTG-ATHN-01 — Testing for Credentials Transported over an Encrypted Channel

Testing for credentials transported over an encrypted channel ensures that sensitive authentication data, such as usernames, passwords, and session tokens, are protected from interception during transit. Attackers positioned on the network—such as through Man-in-the-Middle (MitM) attacks on public Wi-Fi or compromised internal networks—can use packet sniffing tools to capture cleartext credentials if TLS/SSL is not properly enforced. This vulnerability typically occurs at login endpoints, password reset forms, and API authentication headers where the application fails to mandate HTTPS or improperly redirects from HTTP. Successful exploitation grants the attacker full access to user accounts, leading to total compromise of user data and potential lateral movement within the application.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Is HTTPS enforced for the entire authentication process?
- [ ] Yes — HSTS is **enabled** and all traffic is strictly forced over HTTPS  
- [ ] Yes — redirection to HTTPS occurs, but HSTS is **not enabled**  
- [ ] No — credentials **can** be submitted over unencrypted HTTP  

### Are login pages and forms served over an encrypted channel?
- [ ] Yes — the page containing the login form is delivered via HTTPS  
- [ ] No — the login page is served over HTTP, allowing for form-action hijacking or credential sniffing  

### Is the `Secure` flag applied to all sensitive session cookies?
- [ ] Yes — `Secure` flag is **applied** to all authentication and session cookies  
- [ ] Yes — `Secure` flag is **applied** only to some cookies  
- [ ] No — `Secure` flag is **not applied**, allowing cookies to be exfiltrated via unencrypted requests  

### Does the server support weak TLS versions or insecure cipher suites?
- [ ] No — only modern TLS (1.2+) and strong ciphers are **enabled**  
- [ ] Yes — legacy versions (TLS 1.0/1.1) or weak ciphers (RC4, DES, CBC) are **supported**  

### Does the application prevent credential transmission to third-party domains over unencrypted channels?
- [ ] Yes — no sensitive data is sent to external domains or all external calls are forced over HTTPS  
- [ ] No — authentication headers or credentials are sent to third-party endpoints (e.g., analytics or SSO) via HTTP  

---