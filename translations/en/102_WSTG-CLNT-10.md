## WSTG-CLNT-10 — Testing WebSockets

WebSocket testing focuses on the full-duplex communication channel established between the client and server, which bypasses traditional HTTP request-response patterns. Pentesters evaluate the security of the handshake process, the presence of Cross-Site WebSocket Hijacking (CSWSH) protections, and the rigor of input validation applied to message payloads. Exploitation typically involves hijacking an active session to exfiltrate data in real-time or injecting malicious payloads that trigger server-side vulnerabilities like Command Injection or SQLi through the persistent socket. Since many traditional Web Application Firewalls (WAFs) do not inspect WebSocket traffic effectively, this protocol often provides a stealthy vector for bypassing perimeter defenses.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Tools:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### Is the WebSocket connection established over a secure channel?
- [ ] Yes — `wss://` (WebSocket Secure) is enforced for all connections  
- [ ] No — `ws://` is used, allowing for person-in-the-middle (PITM) eavesdropping  

### Is the application protected against Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] No — the server strictly validates the `Origin` header during the handshake  
- [ ] Yes — `Origin` header validation is weak or **can** be bypassed via null or spoofed origins  
- [ ] Yes — no `Origin` header validation is performed, allowing cross-site attackers to initiate a connection *(Critical)*  

### Is input validation and sanitization applied to WebSocket message payloads?
- [ ] Yes — all incoming messages are sanitized and validated against a schema  
- [ ] Yes — some validation exists but bypass **is possible** using encoded or non-standard payloads  
- [ ] No — messages are processed directly, allowing for injection (XSS, SQLi, RCE) or logic manipulation  

### Are authentication and authorization checks performed on every WebSocket message?
- [ ] Yes — the session is verified for every message or the protocol is stateless with tokens  
- [ ] Yes — authentication happens at the handshake, but authorization for specific actions **is not** enforced per message  
- [ ] No — authentication is only checked at the handshake, allowing session hijacking to grant full access  

### Does the server implement rate limiting or resource constraints on the WebSocket?
- [ ] Yes — rate limiting and maximum message sizes are **enabled** and enforced  
- [ ] No — no limits exist, allowing for Denial of Service (DoS) via message flooding or large payload sizes  

---