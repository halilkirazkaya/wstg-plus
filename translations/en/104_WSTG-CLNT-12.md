## WSTG-CLNT-12 — Test Browser Storage

Testing browser storage involves analyzing how an application utilizes client-side mechanisms such as LocalStorage, SessionStorage, IndexedDB, and WebSQL to persist data. Insecurely stored sensitive information—including PII, authentication tokens, or session identifiers—can be compromised if an attacker achieves Cross-Site Scripting (XSS) or gains physical access to the user's device. Pentesters must determine if data is stored in cleartext, if it remains accessible after logout, and if the storage lifecycle is appropriately managed to prevent data leakage. From an attacker's perspective, these storage mechanisms are lucrative targets for exfiltrating state-maintaining information that allows for session hijacking or long-term account persistence.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if session tokens or sensitive PII are stored in LocalStorage and are accessible via XSS.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### Does the application store sensitive information in browser storage?
- [ ] No — application does **not** use browser storage or only stores non-sensitive UI state  
- [ ] Yes — sensitive data is stored but **is encrypted** using robust client-side cryptography  
- [ ] Yes — sensitive data (tokens, PII, secrets) is stored in **cleartext** *(Medium)*  

### Is stored data accessible to unauthorized scripts (XSS risk)?
- [ ] No — data is stored in HttpOnly cookies (not browser storage) or storage is **not utilized**  
- [ ] Yes — LocalStorage/SessionStorage is used, making data **completely accessible** via any XSS payload *(High)*  

### Is browser storage appropriately cleared upon user logout?
- [ ] Yes — all application-related storage is **explicitly cleared** during the logout process  
- [ ] No — storage persists after logout, but does **not** contain sensitive session data  
- [ ] No — authentication tokens or PII **persist** in storage after the session is terminated *(Medium)*  

### Does the application use IndexedDB or WebSQL for large/sensitive datasets?
- [ ] No — these storage mechanisms are **not used**  
- [ ] Yes — data is stored and **properly sanitized** before being rendered in the DOM  
- [ ] Yes — sensitive datasets are stored in **cleartext** within IndexedDB/WebSQL structures  

### Can the integrity of the stored data be manipulated to affect application logic?
- [ ] No — application validates or signs data before processing it from storage  
- [ ] Yes — modifying storage values (e.g., user roles, flags) **is possible** and results in altered application behavior  

---