## WSTG-ATHN-06 — Testing for Browser Cache Weakness

Browser cache weakness occurs when sensitive information is stored in the local browser cache and can be retrieved by an unauthorized user with access to the same physical machine. This lack of proper cache control headers allows potentially sensitive data such as personal information, account details, or session identifiers to persist after the user has logged out or closed the session. Attackers or subsequent users on shared or public terminals can exploit this by navigating through browser history, using the "Back" button, or inspecting local cache files to exfiltrate cached responses. Ensuring the implementation of strict directives like `Cache-Control: no-store` and `Pragma: no-cache` is critical for any application handling authenticated content to ensure sensitive data is never written to the disk.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity becomes Medium if the application is frequently accessed from shared/public kiosks or contains highly sensitive PII/PHI data.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Tools:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Are appropriate cache-control headers present on authenticated or sensitive pages?
- [ ] Yes — `Cache-Control: no-store, no-cache` and `Pragma: no-cache` are **present** and **correctly configured**  
- [ ] Yes — some headers are present but `no-store` is **missing**, allowing disk caching  
- [ ] No — no cache-related headers are **applied**, relying on default browser behavior  

### Does the application use the `private` directive for user-specific data?
- [ ] Yes — `Cache-Control: private` is **enabled** for all personalized content to prevent proxy caching  
- [ ] No — content is marked as `public` or lacks the `private` directive, making it **cachable** by intermediate proxies  

### Is sensitive information accessible via the browser "Back" button after a successful logout?
- [ ] No — the browser prompts for re-authentication or the page is **not loaded** from the local cache  
- [ ] Yes — sensitive information is **still visible** via the back button after the session has been terminated  

### Are sensitive non-HTML files (e.g., PDFs, CSV exports, JSON API responses) protected from caching?
- [ ] No — no sensitive non-HTML files are generated or handled by the application  
- [ ] Yes — strict cache-control headers are **applied** to all sensitive file downloads and API endpoints  
- [ ] No — sensitive downloads or API responses are **stored** in the local browser cache or temporary directories  

### Does the application use `Expires: 0` or a date in the past to prevent stale data usage?
- [ ] Yes — `Expires` header is set to `0` or a historical date, **ensuring** the browser treats content as immediately stale  
- [ ] No — `Expires` header is **missing** or set to a future date for sensitive pages  

---