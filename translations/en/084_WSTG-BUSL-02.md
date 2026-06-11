## WSTG-BUSL-02 — Test Ability to Forge Requests

Forging requests within a business logic context involves manipulating application-defined parameters to perform actions outside the intended workflow or authorization level. Attackers target multi-step processes, such as checkout systems or account registration, where the state is maintained via client-side parameters that the server fails to re-validate against a trusted backend source. By altering hidden fields, JSON values, or REST parameters like prices, quantities, or internal status flags, an attacker can bypass payment requirements, escalate privileges, or trigger unintended state changes. This vulnerability typically manifests in stateful web forms or APIs where the server implicitly trusts the client's representation of the business state during a transaction.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical* |

> *Severity becomes Critical if the forged request allows for financial loss, unauthorized administrative actions, or full account takeover.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Does the application validate transactional parameters against a server-side "source of truth"?
- [ ] Yes — all sensitive parameters (price, role, ID) are validated against the database and bypass **not possible**  
- [ ] Yes — validation is applied but can be bypassed via parameter pollution or alternative encoding  
- [ ] No — the application trusts client-side values to determine the cost or outcome of a transaction *(Critical)*  

### Can business-critical values (e.g., quantities, amounts) be modified to unauthorized values?
- [ ] No — negative values, zero values, or excessively high values are rejected by the server  
- [ ] Yes — the server accepts modified values but only within a limited range  
- [ ] Yes — negative or zero values **can** be submitted, leading to financial or logic bypasses  

### Are hidden fields or API parameters used to maintain state across multi-step processes?
- [ ] No — state is maintained securely via server-side sessions or signed tokens  
- [ ] Yes — state is passed via the client but integrity checks (HMAC/Signatures) are **enabled** and robust  
- [ ] Yes — state is passed via the client and integrity checks are **disabled** or can be stripped  

### Is it possible to forge requests that skip mandatory intermediate steps in a workflow?
- [ ] No — the server enforces a strict sequence of operations for every transaction  
- [ ] Yes — steps **can** be skipped but the final action still requires valid data from previous steps  
- [ ] Yes — the final "submit" or "complete" request **can** be forged directly to bypass authorization or payment steps  

---