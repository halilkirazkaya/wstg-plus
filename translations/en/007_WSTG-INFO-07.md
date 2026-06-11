## WSTG-INFO-07 — Map Execution Paths Through Application

Mapping execution paths involves the systematic identification of all reachable endpoints, functional flows, and decision-making branches within a web application. This process is vital for ensuring that no "dark" functionality—such as legacy code, debug interfaces, or undocumented API routes—remains hidden from security scrutiny, as these often lack modern security controls. Attackers utilize a combination of automated spidering and manual exploration to visualize the application’s structure, often discovering vulnerabilities like unauthorized administrative access or business logic flaws during the process. By correlating inputs to specific server-side responses, testers can pinpoint sensitive processing logic that requires targeted deep-dive testing to ensure robust coverage.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### Has the application been fully indexed using automated and manual crawling?
- [ ] Yes — comprehensive map of all visible and linked resources **is completed**  
- [ ] Yes — automated crawling **is completed** but manual exploration is pending  
- [ ] No — application has **not** been indexed  

### Are hidden or undocumented endpoints identified through JS analysis or directory brute-forcing?
- [ ] No — no undocumented paths discovered via side-channel analysis  
- [ ] Yes — hidden paths discovered but they **cannot** be accessed without authorization  
- [ ] Yes — undocumented endpoints **are** accessible and provide sensitive functionality *(Critical / High)*  

### Can execution paths be altered via non-standard parameters or headers?
- [ ] No — parameters like `debug`, `test`, or `admin` do **not** affect execution flow  
- [ ] Yes — parameters change output but **do not** bypass security controls  
- [ ] Yes — path-altering parameters **are applied** and allow bypassing intended logic  

### Is the flow of multi-step business logic (e.g., multi-factor auth, checkout) fully mapped?
- [ ] Yes — all states and transitions in multi-step processes **are identified**  
- [ ] No — complex state machine transitions **cannot** be fully mapped  

### Are API documentation files (Swagger/OpenAPI) or client-side maps (Source Maps) exposed?
- [ ] No — no architectural maps or documentation files are publicly exposed  
- [ ] Yes — documentation is present but **is protected** by authentication  
- [ ] Yes — Swagger UI, OpenAPI JSON, or JS Source Maps **are enabled** and publicly accessible  

---