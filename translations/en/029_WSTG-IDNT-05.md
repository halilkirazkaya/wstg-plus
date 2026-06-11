## WSTG-IDNT-05 — Testing for Weak or Unenforced Username Policy

Testing for weak or unenforced username policies focuses on the rules governing account identifier creation and their susceptibility to enumeration or spoofing. Weak policies often permit predictable sequences, common dictionary words, or identifiers that mirror internal employee IDs, which significantly reduces the search space for brute-force and credential stuffing attacks. From an attacker's perspective, identifying these patterns is the first step in large-scale account discovery, often achieved by analyzing registration error messages, password reset behavior, or metadata in public-facing profiles. Ensuring a robust policy prevents attackers from systematically mapping the user base and facilitates defense against targeted phishing and automated authentication bypass attempts.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Are usernames based on highly predictable or sequential patterns?
- [ ] No — usernames are random, user-defined, or high-entropy strings  
- [ ] Yes — usernames follow a predictable format (e.g., `user1001`, `user1002`) but authentication is robust  
- [ ] Yes — usernames are **strictly sequential** or follow a known corporate format, facilitating easy enumeration  

### Does the application enforce minimum length and complexity requirements for usernames?
- [ ] Yes — strict length and character set policies **are enforced** during registration  
- [ ] Yes — policies exist but allow extremely short (e.g., 1-2 characters) or overly simple usernames  
- [ ] No — **no policy** is enforced regarding username length or character types  

### Can valid usernames be enumerated through application responses?
- [ ] No — application returns generic error messages and exhibits consistent timing for all attempts  
- [ ] Yes — application returns distinct error messages (e.g., "Username already taken") but rate limiting **is applied**  
- [ ] Yes — application **leaks** valid usernames through registration errors, password resets, or timing differences  

### Does the policy prevent the registration of common or reserved administrative usernames?
- [ ] Yes — application blocks common names (e.g., `admin`, `root`, `support`) and system-reserved terms  
- [ ] No — users **can** register accounts with sensitive or administrative names  

### Is the username policy consistent across all entry points (API, Mobile, Web)?
- [ ] Yes — policies **are applied** consistently across all interfaces and versions  
- [ ] No — legacy endpoints or specific API versions **do not** enforce the same username constraints  

---