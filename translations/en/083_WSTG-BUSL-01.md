## WSTG-BUSL-01 — Test Business Logic Data Validation

Business logic data validation testing focuses on the application's ability to identify and reject data that is syntactically correct but logically invalid within the context of the business domain. Attackers exploit these flaws by submitting unexpected values—such as negative quantities in a shopping cart, dates in the past for future-dated events, or extremely large integers—to bypass constraints and manipulate the application's state. These vulnerabilities often reside in multi-step workflows, checkout processes, and account management modules where the server-side logic fails to verify the contextual integrity of user-supplied data. Successful exploitation can lead to financial loss, integrity compromise, and unauthorized access to restricted features or data.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Does the application enforce logical range limits on numeric inputs?
- [ ] Yes — application correctly rejects negative, zero, or excessively large values where inappropriate *(Most secure)*  
- [ ] Yes — application rejects some invalid ranges but **is** vulnerable to integer overflow or boundary bypass  
- [ ] No — application accepts any numeric value regardless of the business context *(Critical)*  

### Are server-side validation checks consistent across all layers of the application?
- [ ] Yes — validation is applied consistently at both the API/service and database layers  
- [ ] Yes — validation **is applied** at the UI/Frontend but bypass via direct API request **is possible**  
- [ ] No — validation is **not applied** or is inconsistent, allowing for logical data corruption  

### Can the business logic be subverted by manipulating data in multi-step workflows?
- [ ] No — state is maintained securely on the server and data from previous steps **cannot** be tampered with  
- [ ] Yes — validation occurs in early steps but final submission **does not** re-verify the integrity of the data  
- [ ] Yes — workflow bypass **is possible** by skipping steps or submitting out-of-order data  

### Does the application properly handle "edge case" data that bypasses business constraints?
- [ ] Yes — constraints are robust and handle unusual but syntactically valid inputs (e.g., leap years, time zone shifts)  
- [ ] Yes — some edge cases are handled but specific combinations **can** lead to unexpected behavior  
- [ ] No — edge cases directly lead to logic failures, such as free purchases or unauthorized status changes  

---