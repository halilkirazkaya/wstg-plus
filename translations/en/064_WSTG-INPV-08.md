## WSTG-INPV-08 — Testing for SSI Injection

Server-Side Includes (SSI) Injection occurs when an application fails to sanitize user input before incorporating it into HTML files processed by the server's SSI engine. Attackers exploit this by injecting SSI directives, such as `<!--#exec cmd="id" -->`, to execute arbitrary shell commands, read local files, or exfiltrate environment variables. This vulnerability is typically found in applications serving legacy file extensions like `.shtml`, `.shtm`, or `.stm`, or where the web server is explicitly configured to parse SSI within standard HTML files. Successful exploitation grants the attacker the same permissions as the web server process, often leading to a full system compromise or sensitive data exposure.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Does the web server support and process SSI directives?
- [ ] No — server **does not** support SSI or file extensions like `.shtml` are **disabled**  
- [ ] Yes — SSI **is enabled** but user input is **not** reflected in processed pages  
- [ ] Yes — SSI **is enabled** and user input **is** reflected in processed pages  

### Can arbitrary system commands be executed via the `#exec` directive?
- [ ] No — `#exec` directive is **disabled** or restricted by server configuration  
- [ ] Yes — command execution **is possible** via `#exec cmd` or `#exec cgi` payloads  

### Is the exfiltration of sensitive files or environment variables possible?
- [ ] No — directives like `#include` or `#config` are **disabled**  
- [ ] Yes — reading local files (e.g., `/etc/passwd`) **is possible** via `#include virtual`  
- [ ] Yes — server environment variables **can** be exfiltrated via `#printenv` or `#echo`  

### Are input validation or WAF controls effective against SSI payloads?
- [ ] Yes — strict input validation **is applied** and bypass **not possible**  
- [ ] Yes — validation/WAF **is applied** but bypass **is possible** using character encoding or different SSI syntax  
- [ ] No — no validation or WAF protection is present for SSI characters (`<`, `!`, `#`, `-`, `"`)  

---