## WSTG-ATHZ-03 — Testen op Privilege Escalation

Privilege escalation vindt plaats wanneer een aanvaller kwetsbaarheden misbruikt om toegang te krijgen tot resources of functionaliteit die gereserveerd zijn voor gebruikers met hogere autorisatieniveaus of andere identiteiten. Bij verticale escalatie probeert een standaardgebruiker toegang te krijgen tot administratieve functies, terwijl horizontale escalatie het benaderen van gegevens van een andere gebruiker met hetzelfde privilegieniveau betreft. Deze fouten manifesteren zich doorgaans in slecht geïmplementeerde access control lists (ACLs), insecure direct object references (IDOR), of parameter tampering binnen sessie- of identiteitstokens. Vanuit het perspectief van een aanvaller wordt dit vaak bereikt door het manipuleren van numerieke ID's, het wijzigen van rol-gebaseerde parameters in cookies of JWTs, of door direct verborgen API-endpoints aan te roepen die server-side autorisatiecontroles missen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Implementeert de applicatie meerdere privilegieniveaus of multi-tenancy?
- [ ] Nee — de applicatie is een systeem voor één gebruiker of één rol  
- [ ] Ja — er zijn meerdere rollen of tenants gedefinieerd die autorisatietests vereisen  

### Is horizontale privilege escalation (toegang op hetzelfde niveau) mogelijk?
- [ ] Nee — autorisatiecontroles worden toegepast op alle resource-identifiers en sessie-eigenaren  
- [ ] Ja — toegang tot gegevens van andere gebruikers is mogelijk via IDOR of parameter tampering  
- [ ] Ja — datalekken zijn mogelijk, maar het wijzigen van gegevens van andere gebruikers is niet mogelijk  

### Is verticale privilege escalation (laag naar hoog) mogelijk?
- [ ] Nee — administratieve endpoints dwingen strikt role-based access controls (RBAC) af  
- [ ] Ja — administratieve functies zijn toegankelijk voor gebruikers met weinig privileges via directe URL-toegang  
- [ ] Ja — administratieve functies zijn toegankelijk door het manipuleren van rol-gerelateerde headers, cookies of JWT-claims  

### Kunnen gebruikersrollen of machtigingen worden gewijzigd via Mass Assignment of Parameter Pollution?
- [ ] Nee — rol-gebaseerde velden zijn strikt alleen-lezen en kunnen niet door gebruikers worden gewijzigd  
- [ ] Ja — gebruikersrechten kunnen worden verhoogd door verborgen parameters (bijv. `role=admin`) op te nemen in profielupdates of registratie  

### Worden autorisatiecontroles consistent afgedwongen in alle API-versies en HTTP-methoden?
- [ ] Ja — controles worden consistent toegepast op alle versies en methoden  
- [ ] Nee — legacy API-versies (bijv. `/v1/`) of specifieke HTTP-methoden (bijv. `PUT`, `DELETE`, `PATCH`) omzeilen autorisatie  
- [ ] Nee — administratieve functies zijn verborgen in de UI, maar ingeschakeld op API-niveau voor alle gebruikers  

---