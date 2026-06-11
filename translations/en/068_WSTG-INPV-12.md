## WSTG-INPV-12 — Command Injection

Command injection occurs when an application passes unsafe user-supplied data, such as form inputs, HTTP headers, or cookies, to a system shell. This vulnerability allows an attacker to execute arbitrary operating system commands on the server, typically with the privileges of the web application process. From an attacker's perspective, this is exploited by using shell metacharacters like semicolons, pipes, or backticks to chain malicious commands to the intended execution flow. The impact is frequently catastrophic, leading to full system compromise, unauthorized data exfiltration, or lateral movement within the internal network.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Test Status** | Not Performed |
| **Severity** | Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Tools:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### Does the application invoke system-level commands or shell executables?
- [ ] No — application does **not** interact with the OS shell  
- [ ] Yes — application uses internal APIs or high-level libraries for system tasks  
- [ ] Yes — application directly invokes shell commands via system calls like `system()`, `exec()`, or `popen()`  

### Is user input validated or sanitized before being passed to system calls?
- [ ] Yes — strict allow-listing and input validation **is applied**  
- [ ] Yes — sanitization or escaping is used but bypass **is possible**  
- [ ] No — user input is passed directly to the shell **without** validation  

### Can shell metacharacters be used to manipulate the command execution flow?
- [ ] No — metacharacters are correctly filtered or escaped  
- [ ] Yes — in-band execution where command output is reflected in the response **is possible**  
- [ ] Yes — blind execution where no output is reflected **is possible**  

### Is out-of-band (OOB) exfiltration possible from the host?
- [ ] No — server has no egress or OOB signals (DNS/HTTP) fail  
- [ ] Yes — data exfiltration via DNS or HTTP-based OOB signals **is possible**  

### Does the application process run with elevated system privileges?
- [ ] No — application runs with a dedicated, low-privilege service account  
- [ ] Yes — application runs as `root`, `Administrator`, or a high-privilege user *(Critical)*  

---