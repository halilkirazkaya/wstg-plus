## WSTG-CONF-01 — Test Network Infrastructure Configuration

Network infrastructure configuration testing involves identifying weaknesses in the underlying server and network components that support the web application. Attackers target misconfigured services, outdated protocols, and unnecessary open ports to gain a foothold, move laterally, or perform reconnaissance. Common findings include insecure TLS versions, verbose service banners, and exposed administrative interfaces that should be restricted to internal networks. By exploiting these infrastructure-level flaws, an adversary can compromise the confidentiality of data in transit or gain unauthorized access to the host operating system.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Tools:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Are there any unnecessary open ports or services exposed on the application host?
- [ ] No — only required ports (e.g., 443) are **accessible**  
- [ ] Yes — additional services are exposed but follow a **secure** configuration  
- [ ] Yes — non-essential services (e.g., FTP, Telnet, SMB) are **exposed** publicly  

### Do service banners or headers leak sensitive version information?
- [ ] No — banners are stripped or generic  
- [ ] Yes — banners reveal server software (e.g., Apache/nginx) but **not** version numbers  
- [ ] Yes — verbose banners reveal specific software versions, aiding **exploitation**  

### Is the TLS configuration following modern security best practices?
- [ ] Yes — only TLS 1.2/1.3 **enabled** with strong cipher suites  
- [ ] Yes — legacy protocols (TLS 1.0/1.1) are **enabled**  
- [ ] No — weak ciphers or insecure protocols (SSLv2/v3) are **enabled**  

### Are administrative or management interfaces restricted to authorized networks?
- [ ] Yes — interfaces (e.g., SSH, RDP, control panels) are **not accessible** from the public internet  
- [ ] No — interfaces are publicly **accessible** but require multi-factor authentication  
- [ ] No — interfaces are publicly **accessible** with single-factor authentication or default credentials  

---