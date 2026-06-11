## WSTG-CLNT-08 — Testing for Cross Site Flashing

Cross-Site Flashing (XSF) is a client-side vulnerability occurring when a Flash (SWF) file improperly handles user-controlled input, allowing an attacker to execute malicious ActionScript code or bridge into the browser's JavaScript environment. By manipulating parameters such as `FlashVars` or URL query strings passed to sinks like `ExternalInterface.call`, `getURL`, or `loadMovie`, an attacker can perform actions in the context of the vulnerable domain, including session hijacking and data exfiltration. This vulnerability is primarily found in legacy enterprise applications that still host SWF files, where insufficient input validation allows the Flash movie to be repurposed as a vector for Cross-Site Scripting (XSS) or to bypass Same-Origin Policy (SOP) via permissive cross-domain configurations.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Are legacy Adobe Flash (.swf) files hosted on the application?
- [ ] No — no Flash files are hosted or referenced within the application scope  
- [ ] Yes — SWF files are present but serve static content only  
- [ ] Yes — SWF files are present and accept dynamic parameters via `FlashVars` or URL strings  

### Is input passed to ActionScript sinks sanitized?
- [ ] Yes — all inputs are strictly validated and ActionScript sinks **cannot** be manipulated  
- [ ] Yes — validation is applied but bypass **is possible** via specific encoding techniques  
- [ ] No — input is passed directly to sensitive sinks like `ExternalInterface.call` or `getURL` *(Critical)*  

### Can the Flash file be used to execute arbitrary JavaScript (XSS)?
- [ ] No — `ExternalInterface` is **disabled** or the `allowScriptAccess` parameter is set to `never`  
- [ ] Yes — `allowScriptAccess` is set to `sameDomain` but the SWF is hosted on the target domain  
- [ ] Yes — `allowScriptAccess` is set to `always`, allowing XSS from any domain  

### Does the `crossdomain.xml` policy prevent unauthorized cross-origin requests?
- [ ] Yes — `crossdomain.xml` is restrictive and only allows trusted, specific origins  
- [ ] No — `crossdomain.xml` file **is missing**  
- [ ] Yes — policy is overly permissive (e.g., `<allow-access-from domain="*" />`)  

### Can the SWF file be forced to load external, attacker-controlled movies?
- [ ] No — the application **cannot** be forced to load external SWF files  
- [ ] Yes — the `loadMovie` or `loadMovieNum` functions accept unvalidated external URLs  

---