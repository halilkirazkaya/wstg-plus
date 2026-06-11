## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Directory traversal and file inclusion vulnerabilities arise when an application uses user-controllable input to construct paths to files or directories without sufficient validation or sanitization. Attackers exploit these flaws by injecting sequences such as `../` to navigate outside the intended directory, potentially accessing sensitive system files, configuration data, or application source code. In more severe cases involving Local File Inclusion (LFI) or Remote File Inclusion (RFI), an attacker may achieve remote code execution by including malicious scripts or leveraging log poisoning and PHP wrappers. These vulnerabilities typically reside in parameters used for dynamic content loading, template engines, or image retrieval endpoints where the server-side logic handles file system paths.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Tools:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Do parameters accept file names or paths for server-side processing?
- [ ] No — no parameters appear to interact with the file system  
- [ ] Yes — parameters exist but use a strict allowlist of file identifiers  
- [ ] Yes — parameters accept direct file names or paths  

### Is the input sanitized to prevent directory traversal sequences?
- [ ] Yes — input is validated against a strict allowlist and traversal sequences are **not possible**  
- [ ] Yes — input is sanitized by removing `../` sequences, but recursive bypass **is possible**  
- [ ] No — no sanitization or validation **is applied** to path-related input  

### Is it possible to access files outside the restricted directory via traversal sequences?
- [ ] No — the application or OS prevents access to files outside the defined scope  
- [ ] Yes — access to files within the web root **is possible**  
- [ ] Yes — access to sensitive system files (e.g., `/etc/passwd`, `C:\Windows\win.ini`) **is possible** *(Critical)*  

### Does the server allow the inclusion of remote URLs (RFI)?
- [ ] No — remote file inclusion is **disabled** at the server/application level  
- [ ] Yes — remote files **can** be included but execution **is not possible**  
- [ ] Yes — remote files **can** be included and executed on the server *(Critical)*  

### Can filters be bypassed using encoding or special characters?
- [ ] No — filters effectively handle various encodings and null bytes  
- [ ] Yes — bypass **is possible** using URL encoding, double URL encoding, or 16-bit Unicode  
- [ ] Yes — bypass **is possible** using null byte injection (`%00`) or filesystem wrappers (e.g., `php://filter`)  

---