## WSTG-BUSL-06 — Testing for the Circumvention of Work Flows

Workflow circumvention involves manipulating an application's logic to bypass intended sequences or prerequisites within multi-step processes. Attackers identify sensitive endpoints—such as payment confirmations, administrative approvals, or MFA completion screens—and attempt to access them directly, skipping mandatory intermediate steps. This vulnerability occurs when server-side logic fails to maintain a robust state machine, relying instead on the assumption that a user follows the UI-driven path. Successful exploitation can lead to critical business logic failures, including unauthorized transactions, privilege escalation, and the bypass of identity verification mechanisms.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the bypass allows for unauthorized financial transactions, privilege escalation, or administrative access.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Does the application contain multi-stage business processes?
- [ ] No — application consists only of single-request actions  
- [ ] Yes — multi-stage processes (e.g., shopping carts, wizard forms, registration) **are** present  

### Is the sequence of steps strictly enforced by the server?
- [ ] Yes — server-side state tracking ensures steps **cannot** be skipped  
- [ ] Yes — client-side controls suggest a sequence, but server **does not** validate progress  
- [ ] No — the application **does not** enforce any specific order of operations  

### Can an attacker reach the final stage of a process by directly requesting the terminal URL or endpoint?
- [ ] No — direct access to terminal steps is **not possible** without prior completion  
- [ ] Yes — terminal steps (e.g., `/api/v1/checkout/complete`) **can** be reached by direct request  

### Does the application verify that prerequisite data from previous steps is present and valid?
- [ ] Yes — server validates all previous step data before proceeding  
- [ ] No — server **is** vulnerable to "skipping" steps where data is expected but not verified  

### Can security controls like MFA or identity verification be bypassed by manipulating the workflow?
- [ ] No — critical controls **cannot** be circumvented via flow manipulation  
- [ ] Yes — bypass of MFA or identity checks **is possible** by jumping to the post-verification step  

---