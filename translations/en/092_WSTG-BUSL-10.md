## WSTG-BUSL-10 — Test Payment Functionality

Testing payment functionality involves identifying business logic flaws that allow an attacker to bypass payment gateways, manipulate order totals, or spoof successful transaction signals. These vulnerabilities typically reside in the communication between the application, the client browser, and third-party payment processors such as Stripe, PayPal, or internal ledger systems. An attacker may attempt to modify price parameters in transit, submit negative quantities to reduce the total cost, or replay successful transaction callbacks to fulfill orders without actual payment. Successful exploitation leads to direct financial loss, inventory discrepancy, and potential compromise of the application's e-commerce integrity.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Is the transaction amount validated on the server side prior to processing?
- [ ] Yes — amount is strictly validated against the server-side product database *(Most secure)*  
- [ ] Yes — amount is validated but logic bypass **is possible** via parameter pollution or currency switching  
- [ ] No — transaction amount **can** be modified in the client-side request and is honored by the backend  

### Can the quantity of items be manipulated to result in a zero or negative total?
- [ ] No — negative or zero quantities are rejected by the application validation logic  
- [ ] Yes — negative quantities are accepted in the cart but the total is corrected at checkout  
- [ ] Yes — negative quantities **can** be used to reduce the overall order total or obtain a credit *(Critical)*  

### Does the application securely handle payment gateway callbacks or webhooks?
- [ ] Yes — callbacks require a valid cryptographic signature that **cannot** be spoofed  
- [ ] Yes — signatures are checked but the secret is weak or the verification **can** be bypassed  
- [ ] No — application relies on unauthenticated or unverified callbacks to confirm payment status  

### Is the application vulnerable to race conditions during the checkout or payment phase?
- [ ] No — transactional integrity and database locking mechanisms are **enabled**  
- [ ] Yes — race conditions **are possible** but do not impact the final price or fulfillment  
- [ ] Yes — concurrent requests **can** lead to multiple fulfillments for a single payment or "double-spending" of gift cards  

### Are discount codes or loyalty points subject to manipulation or reuse?
- [ ] No — codes are validated for one-time use and logic constraints are **applied**  
- [ ] Yes — codes can be reused or applied to ineligible items, but impact is limited  
- [ ] Yes — attackers **can** apply multiple conflicting codes or bypass expiration and usage limits  

---