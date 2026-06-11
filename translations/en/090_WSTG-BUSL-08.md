## WSTG-BUSL-08 — Test Upload of Unexpected File Types

Uploading unexpected file types allows attackers to bypass business logic constraints by submitting files that the application does not intend to process, potentially leading to remote code execution, cross-site scripting, or denial of service. Attackers probe these vulnerabilities by modifying file extensions, MIME types, and magic bytes to trick the server-side validation logic into accepting malicious content. This typically occurs in profile picture uploads, document management systems, or support ticket attachments where insufficient validation exists on the back-end. Successful exploitation can compromise the host server if the uploaded file is executed or lead to client-side attacks if the file is served back to other users with an incorrect Content-Type.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Does the application restrict file uploads to a specific set of extensions?
- [ ] Yes — a strict whitelist of extensions is enforced and bypass **not possible**  
- [ ] Yes — extension whitelist is present but bypass **is possible** via double extensions or null bytes  
- [ ] No — any file extension **can** be uploaded  

### Is the file content validated beyond the file extension?
- [ ] Yes — server-side logic verifies Magic Bytes and performs deep content inspection  
- [ ] Yes — server-side logic verifies the MIME type via the `Content-Type` header only  
- [ ] No — no content validation **is applied** after the extension check  

### Can an attacker execute code by uploading a web shell or script?
- [ ] No — uploaded files are stored in a non-executable directory or a storage bucket (S3/Azure Blob)  
- [ ] Yes — files are stored in a web-accessible directory but execution **is disabled** via server configuration  
- [ ] Yes — uploaded malicious scripts **can** be executed directly via a predictable URL path *(Critical)*  

### Are client-side attacks like XSS possible via uploaded file types?
- [ ] No — files are served with `Content-Disposition: attachment` and `X-Content-Type-Options: nosniff`  
- [ ] Yes — SVG or HTML files **can** be uploaded and rendered in the browser, leading to XSS  
- [ ] Yes — image metadata (EXIF) **is processed** and reflected in the UI without sanitization  

---