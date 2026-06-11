## WSTG-INPV-07 — XML Injection

XML Injection occurs when an application incorporates user-supplied data into an XML document or stream without proper validation, sanitization, or escaping of XML meta-characters. By injecting specific XML tags or structural elements, an attacker can manipulate the document's logic, bypass authentication mechanisms, or interfere with the integrity of the data processed by the backend. This vulnerability is commonly found in SOAP-based web services, REST APIs that accept XML, SAML assertions, and applications that generate XML configuration files or reports dynamically. From an attacker's perspective, the goal is to break out of the intended data context to alter the document structure, potentially leading to unauthorized privilege escalation or the execution of unintended business logic.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Tools:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### Does the application accept and process XML-formatted input from the user?
- [ ] No — application does **not** process XML input  
- [ ] Yes — XML input is processed via APIs (SOAP/REST)  
- [ ] Yes — XML is processed via file uploads (SVG, DOCX, etc.)  

### Are XML meta-characters (e.g., `<`, `>`, `&`, `'`, `"`) properly escaped or sanitized?
- [ ] Yes — all meta-characters are **properly** escaped or sanitized *(Most secure)*  
- [ ] Yes — some filtering is applied but bypass **is possible** using encoding  
- [ ] No — raw input is embedded directly into XML structures **without** sanitization  

### Is server-side schema validation (XSD/DTD) enforced for all XML inputs?
- [ ] Yes — strict schema validation is **enabled** and prevents structural changes  
- [ ] Yes — validation is **enabled** but does **not** prevent tag injection within data fields  
- [ ] No — schema validation is **disabled** or not implemented  

### Can an attacker inject new XML tags to alter the document structure or logic?
- [ ] No — structure remains intact regardless of input  
- [ ] Yes — tags can be injected but **cannot** alter business logic  
- [ ] Yes — tag injection **is possible** and allows for logic bypass or data modification *(Critical)*  

### Can the XML parser be manipulated using CDATA sections or XML comments?
- [ ] No — parser correctly handles or rejects CDATA/comment markers  
- [ ] Yes — injection of `<![CDATA[...]]>` or `<!-- ... -->` **is possible** to hide or bypass filters  

---