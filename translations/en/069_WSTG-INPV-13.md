## WSTG-INPV-13 — Format String Injection

Format string injection occurs when a web application passes user-controlled input directly into the format string argument of a variadic function, such as C's `printf` family or low-level logging wrappers. While less common in modern managed-code web frameworks, this vulnerability remains critical in legacy CGI applications, native extensions, or edge-case logging systems interacting with low-level system libraries. Attackers exploit this by supplying format specifiers like `%x` or `%p` to leak sensitive memory addresses and data from the stack, or the `%n` specifier to perform arbitrary memory writes. Successful exploitation typically results in complete system compromise via remote code execution or, at minimum, an impactful denial-of-service condition through process crashes.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Does the application process user input through native code or legacy CGI interfaces?
- [ ] No — application is built entirely on managed code frameworks (e.g., JVM, CLR)  
- [ ] Yes — application uses JNI, native C/C++ extensions, or legacy binary CGIs where format string vulnerabilities **can** exist  

### Are user-supplied inputs passed as the primary argument to format-aware functions?
- [ ] No — inputs are passed as data arguments, and format strings are **static** and **hardcoded**  
- [ ] Yes — user input is directly concatenated into the format string argument, but sanitization **is applied**  
- [ ] Yes — user input is directly used as the format string and no sanitization **is applied**  

### Is it possible to leak memory contents using format specifiers?
- [ ] No — input is validated and specifiers like `%p`, `%x`, or `%s` are **not processed**  
- [ ] Yes — supply of `%p` or `%x` specifiers results in the reflection of stack or heap memory addresses  
- [ ] Yes — sensitive data (e.g., pointers, stack cookies) **can** be exfiltrated via format specifiers  

### Can the application be crashed or manipulated using the `%n` specifier?
- [ ] No — specifiers that permit memory writes are **disabled** or blocked by the compiler/environment  
- [ ] Yes — the application crashes (DoS) when `%s%s%s%s` or similar strings are provided  
- [ ] Yes — arbitrary memory writes via `%n` are **possible**, potentially leading to Remote Code Execution (RCE)  

### Are modern binary protections (ASLR, DEP, Stack Canaries) bypassable through this injection?
- [ ] No — the environment is hardened and memory leakage **cannot** be used to bypass protections  
- [ ] Yes — format string injection **can** be used to leak base addresses, making ASLR/DEP bypass **possible**  

---