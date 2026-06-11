## WSTG-INPV-10 — IMAP SMTP Injection

IMAP and SMTP injection vulnerabilities arise when a web application improperly filters user-supplied data before incorporating it into commands sent to a mail server. By injecting Carriage Return and Line Feed (CRLF) characters, an attacker can terminate the intended command and append arbitrary mail instructions, such as adding additional recipients (CC/BCC) or modifying the message body. These flaws are most frequently found in web-to-mail gateways, contact forms, and email clients that communicate directly with mail servers via protocols like IMAP4 or SMTP. Successful exploitation allows for the unauthorized relaying of spam, the exfiltration of sensitive email contents, and in some cases, the total compromise of the mail server's integrity.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Does the application process user-supplied input to construct IMAP or SMTP commands?
- [ ] No — application does **not** interact with mail servers via user input  
- [ ] Yes — application interacts with mail servers but uses a secure API or library  
- [ ] Yes — application constructs raw mail commands using user-controlled parameters  

### Is input validated to prevent the injection of Carriage Return (`\r`) and Line Feed (`\n`) sequences?
- [ ] Yes — CRLF characters are **strictly** filtered, encoded, or rejected  
- [ ] Yes — input is validated against a strict allowlist of characters  
- [ ] No — CRLF characters are **not** filtered, allowing command termination and injection  

### Can an attacker inject additional mail headers such as `Bcc:` or `Cc:`?
- [ ] No — header modification is **not possible** due to strict input constraints  
- [ ] Yes — injection of additional headers **is possible** via CRLF sequences  
- [ ] Yes — mail relaying to external domains **is enabled** and exploitable *(Critical)*  

### Does the application allow the manipulation of IMAP commands to access unauthorized mailboxes?
- [ ] No — application does **not** use IMAP or strictly limits mailbox access to the authenticated user  
- [ ] Yes — IMAP command injection **is possible** but limited to metadata enumeration  
- [ ] Yes — IMAP command injection allows for the reading or deletion of other users' emails  

### Is the backend mail server vulnerable to command pipelining?
- [ ] No — mail server rejects batched commands or multiple commands in a single session  
- [ ] Yes — mail server processes multiple commands injected via a single input field  

---