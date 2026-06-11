## WSTG-INPV-11 — Code Injection

Code injection occurs when an application evaluates user-supplied code snippets within its runtime environment, typically through unsafe language-specific functions like `eval()`, `exec()`, or `system()`. This vulnerability differs from command injection because it targets the programming language's execution context (e.g., PHP, Python, Node.js) rather than the underlying operating system shell. Attackers exploit these points to execute arbitrary logic, exfiltrate environment variables, or achieve full remote code execution (RCE) on the server. Pentesters should investigate features that process mathematical expressions, server-side templates, or dynamic configuration parameters where input might be interpreted as executable code.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Test Status** | Not Performed |
| **Severity** | Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Tools:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Do any application features evaluate dynamic input via language-specific evaluation functions?
- [ ] No — dynamic evaluation functions **are not** present in the codebase  
- [ ] Yes — functions like `eval()`, `exec()`, or `include()` are used but do **not** accept user input  
- [ ] Yes — functions are used and process user-supplied input  

### Are input validation or sanitization mechanisms applied to input before evaluation?
- [ ] Yes — input is strictly validated against a **hardcoded whitelist**  
- [ ] Yes — input is sanitized via blacklisting, but bypass **is possible**  
- [ ] No — input is passed directly to the evaluation function and **no controls** are applied  

### Is the execution environment sandboxed or restricted to prevent system-level access?
- [ ] Yes — the execution occurs in a highly restricted sandbox where system calls **cannot** be made  
- [ ] Yes — some restrictions exist, but sandbox escape **is possible**  
- [ ] No — the environment allows full access to the underlying language APIs and OS functions *(Critical)*  

### Can the code injection be used to exfiltrate sensitive information?
- [ ] No — execution is blind and no out-of-band communication **is possible**  
- [ ] Yes — environment variables or local files **can** be exfiltrated via HTTP/DNS requests  
- [ ] Yes — results of the executed code are **directly reflected** in the application response  

### Does the application allow for remote file inclusion or dynamic library loading?
- [ ] No — only local, predefined code paths can be executed  
- [ ] Yes — the application **can** be forced to load and execute code from a remote or attacker-controlled source  

---