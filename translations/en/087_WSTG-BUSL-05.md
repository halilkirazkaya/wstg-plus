## WSTG-BUSL-05 — Test Number of Times a Function Can Be Used Limits

This test evaluates whether an application enforces business logic constraints on the frequency and total number of times a specific action or function can be executed by a single user, session, or IP address. Failure to implement these limits allows attackers to abuse high-value functions—such as sending SMS OTPs, triggering password reset emails, applying discount codes, or performing heavy data exports—leading to financial loss, resource exhaustion, or reputational damage. These vulnerabilities are typically found in transactional endpoints or communication features and are exploited through automated scripts that repeat the action far beyond intended business thresholds. From an attacker's perspective, this is a prime target for SMS pumping, mail bombing, or brute-forcing workflows that lack explicit rate-limiting or count-based throttles.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the function involves financial transactions, SMS costs, or impacts system-wide availability.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### Does the application define and enforce limits on sensitive business functions?
- [ ] Yes — limits are strictly enforced and bypass **not possible**  
- [ ] Yes — limits are enforced but are too high to prevent abuse  
- [ ] No — no limits are applied to the function execution  

### Are rate limits and usage counters enforced server-side?
- [ ] Yes — enforcement is strictly server-side and **cannot** be manipulated  
- [ ] No — enforcement relies on client-side logic (e.g., JavaScript or hidden fields)  
- [ ] No — no enforcement exists at any level  

### Can the usage limits be bypassed through header or session manipulation?
- [ ] No — bypass via `X-Forwarded-For`, `Origin`, or session rotation is **not possible**  
- [ ] Yes — limits **can** be bypassed by spoofing IP headers or clearing cookies  
- [ ] Yes — limits **can** be bypassed by creating multiple accounts or sessions  

### Does exceeding the limit trigger an appropriate defensive response?
- [ ] Yes — application returns `429 Too Many Requests` and logs the event *(Most secure)*  
- [ ] Yes — application returns an error but does **not** log the potential abuse  
- [ ] No — application continues to process requests but ignores the output  
- [ ] No — application provides no response or crashes under load  

### Is the impact of abusing the function significant to the business?
- [ ] No — function abuse has negligible cost or resource impact  
- [ ] Yes — abuse **can** lead to minor resource consumption or user annoyance  
- [ ] Yes — abuse **can** lead to significant financial costs (e.g., SMS fees) or service denial *(Critical)*  

---