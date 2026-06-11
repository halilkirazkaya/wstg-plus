## WSTG-INFO-04 — Enumerate Applications on Webserver

Application enumeration is the process of identifying all web applications and services hosted on a single web server or IP address. Because modern web servers often host multiple virtual hosts, legacy staging environments, or administrative interfaces on non-standard ports and paths, failing to identify these hidden assets leaves a significant portion of the attack surface untested. Attackers utilize techniques such as DNS zone transfers, virtual host brute-forcing, and reverse IP lookups to discover these forgotten or obscured entry points, which are frequently less secure than the primary production application.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational / Low |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Tools:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Are multiple IP addresses or DNS aliases associated with the target infrastructure?
- [ ] No — only a single IP and A-record exist  
- [ ] Yes — multiple IP addresses or aliases **are** identified, expanding the scope  

### Does the web server host multiple Virtual Hosts (vhosts) on the same IP?
- [ ] No — only the primary domain is served by the server  
- [ ] Yes — additional virtual hosts identified but access **is not possible** (e.g., 403 Forbidden)  
- [ ] Yes — hidden, development, or staging virtual hosts **are** identified and accessible  

### Are applications hosted on non-standard ports?
- [ ] No — only standard ports (80/443) are open  
- [ ] Yes — non-standard ports are open but do **not** host web applications  
- [ ] Yes — additional web applications **are** identified on non-standard ports (e.g., 8080, 8443, 9000)  

### Are distinct applications mapped to specific URI paths?
- [ ] No — a single application serves the entire root directory  
- [ ] Yes — multiple applications **are** discovered through directory enumeration (e.g., `/api`, `/portal`, `/backup`)  

### Do third-party reconnaissance services reveal historical or secondary applications?
- [ ] No — no significant findings from Shodan, Censys, or Wayback Machine  
- [ ] Yes — historical snapshots or service scans reveal applications that **are** currently active but unlinked  

---