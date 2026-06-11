## WSTG-BUSL-04 — Test for Process Timing

Process timing attacks involve measuring the time an application takes to process specific requests to infer sensitive information about internal states or data existence. By analyzing millisecond-level discrepancies in response times—often caused by early-exit conditional logic, database lookups, or non-constant-time cryptographic comparisons—attackers can perform side-channel analysis to enumerate valid usernames, verify password fragments, or confirm the existence of specific records. These vulnerabilities typically occur at authentication endpoints, password reset forms, and resource-heavy API filters where backend logic branches based on input validity. From an attacker's perspective, these timing differences serve as a "boolean oracle" that reveals truths about the backend data even when the application returns identical, generic error messages to the user.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Tools:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### Does the application exhibit varying response times for valid vs. invalid resource requests?
- [ ] No — response times are identical regardless of input validity  
- [ ] Yes — negligible timing differences exist but are **not** statistically significant  
- [ ] Yes — consistent timing differences are **observable** and provide a reliable oracle  

### Are constant-time comparison functions or artificial delays implemented to normalize response times?
- [ ] Yes — constant-time algorithms are **applied** to all sensitive comparison operations *(Most secure)*  
- [ ] Yes — artificial delays or jitter are **enabled**, but the underlying logic remains non-constant-time  
- [ ] No — no timing mitigations are **applied**, allowing direct measurement of logic branches  

### Can an attacker use statistical analysis to isolate the timing signal from network noise?
- [ ] No — network jitter and application noise make timing analysis **not possible**  
- [ ] Yes — with a sufficient sample size, the timing signal **can** be isolated from noise  
- [ ] Yes — the timing delta is large enough that statistical normalization is **not** required  

### Is account enumeration possible via the login or password reset functionality?
- [ ] No — account existence cannot be determined through timing  
- [ ] Yes — timing discrepancies allow a remote attacker to **enumerate** valid usernames or emails  

### Do resource-intensive operations (e.g., file processing, complex searches) leak data existence via timing?
- [ ] No — resource consumption is uniform regardless of whether a record is found  
- [ ] Yes — existence of sensitive records **can** be inferred by measuring the duration of backend processing  

---