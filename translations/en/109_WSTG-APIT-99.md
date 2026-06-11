## WSTG-APIT-99 — Testing GraphQL

GraphQL is a query language for APIs that allows clients to request specific data structures, but misconfigurations often lead to significant security risks including information disclosure and resource exhaustion. Vulnerabilities typically arise from enabled introspection, lack of query depth/complexity limits, and broken object-level authorization (BOLA) within the resolver functions. Attackers exploit these endpoints by using introspection to map the entire schema, leveraging circular fragments to trigger Denial of Service (DoS), or bypassing authorization by accessing unauthorized fields through nested queries. Testing focuses on the `/graphql`, `/v1/graphql`, or `/graphiql` endpoints to ensure that the implementation enforces strict access controls and resource management.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical* |

> *Severity becomes Critical if broken object-level authorization (BOLA) allows unauthorized access to PII or administrative mutations.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Tools:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### Is the GraphQL introspection system enabled?
- [ ] No — introspection is **disabled** and the schema cannot be mapped  
- [ ] Yes — introspection is **enabled** but requires administrative authentication  
- [ ] Yes — introspection is **enabled** and publicly accessible *(High)*  

### Are resource limits (depth and complexity) enforced on queries?
- [ ] Yes — strict query depth and complexity limits are **applied** and enforced  
- [ ] Yes — limits are **applied** but can be bypassed using aliases or fragments  
- [ ] No — no limits are **applied**, allowing for recursive queries and DoS  

### Is Broken Object-Level Authorization (BOLA) present in resolvers?
- [ ] No — authorization is **applied** at the field and object level for every query  
- [ ] Yes — authorization is **applied** to some fields but sensitive objects are exposed  
- [ ] Yes — authorization is **not applied**, allowing access to any object via ID manipulation  

### Are GraphQL mutations properly protected against unauthorized use?
- [ ] No — mutations are **not** present in the schema  
- [ ] Yes — mutations require valid authentication and strict authorization checks  
- [ ] Yes — mutations are **possible** for unauthenticated users or lack authorization  

### Do error messages leak sensitive implementation or schema details?
- [ ] No — error messages are generic and do **not** reveal internal logic  
- [ ] Yes — error messages reveal underlying database types or suggestions via "Did you mean...?"  
- [ ] Yes — full stack traces and sensitive configuration data are **revealed** in responses  

---