## WSTG-CONF-11 — Test Cloud Storage

Testing for cloud storage misconfigurations involves identifying and auditing publicly accessible object storage services such as Amazon S3, Azure Blobs, or Google Cloud Storage. These resources often contain sensitive data including backups, source code, PII, or configuration files that are exposed due to overly permissive Access Control Lists (ACLs) or Identity and Access Management (IAM) policies. Attackers enumerate bucket names through reconnaissance of JavaScript files, HTML source, and brute-force techniques to find entry points for data exfiltration or unauthorized file modification. Exploitation can lead to significant impact, including full data breaches or the ability to serve malicious files from a trusted domain.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical* |

> *Severity becomes Critical if PII, credentials, or production backups are accessible, or if unauthenticated write access is enabled.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Are cloud storage endpoints (buckets/containers) identified and enumerated?
- [ ] No — no cloud storage endpoints are used or discoverable  
- [ ] Yes — storage endpoints are identified via code analysis or traffic monitoring  
- [ ] Yes — storage endpoints are discovered via naming convention brute-force  

### Can unauthenticated users list the contents of the cloud storage?
- [ ] No — bucket listing is **disabled** and returns 403 Forbidden or 404 Not Found  
- [ ] Yes — listing is **enabled** but no sensitive files are present  
- [ ] Yes — listing is **enabled** and sensitive file names/metadata are visible  

### Is the downloading of sensitive files from identified buckets possible?
- [ ] No — files are protected by IAM policies or valid authentication is required  
- [ ] Yes — files are accessible but require a signed URL that **cannot** be easily guessed  
- [ ] Yes — sensitive files (backups, `.env`, PII) are **publicly accessible** for download *(Critical)*  

### Is unauthenticated file upload or modification permitted?
- [ ] No — unauthenticated uploads or modifications are **not possible**  
- [ ] Yes — authenticated upload is required but bypass **is possible** via weak policy conditions  
- [ ] Yes — unauthenticated writing, overwriting, or deleting of files is **possible** *(Critical)*  

### Are Cross-Origin Resource Sharing (CORS) policies on the storage endpoint restrictive?
- [ ] Yes — CORS is **disabled** or restricted to specific trusted origins  
- [ ] No — CORS is **enabled** with a wildcard `*` origin, allowing unauthorized web-based access  

---