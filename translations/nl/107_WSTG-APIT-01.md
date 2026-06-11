## WSTG-APIT-01 — API Reconnaissance

API Reconnaissance is het systematische proces van het identificeren en in kaart brengen van het API-aanvalsoppervlak om endpoints, ondersteunde methoden en onderliggende datastructuren te onthullen. Aanvallers voeren deze fase uit om ongedocumenteerde (shadow) API's, verouderde versies met legacy-kwetsbaarheden en blootgestelde documentatiebestanden zoals Swagger- of OpenAPI-definities die interne bedrijfslogica onthullen, te ontdekken. Door het analyseren van voorspelbare URL-patronen, client-side JavaScript-bestanden en resultaten van directory brute-force, kan een tegenstander een uitgebreide kaart van de API opbouwen om doelwitten voor Broken Object Level Authorization (BOLA) of Mass Assignment-aanvallen te identificeren. Deze verkenning richt zich doorgaans op algemene paden zoals `/api/v1/`, `/swagger.json` of `/graphql` om een roadmap te krijgen van de backend-functionaliteit van de applicatie.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Informatief / Laag |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Tools:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Zijn API-documentatiebestanden (Swagger, OpenAPI, WSDL) publiek toegankelijk?
- [ ] Nee — API-documentatie is **niet** toegankelijk of is beperkt door authenticatie  
- [ ] Ja — documentatie is toegankelijk maar vereist geldige inloggegevens  
- [ ] Ja — API-documentatie (bijv. `/swagger-ui.html`) is **publiek toegankelijk** zonder authenticatie  

### Kunnen ongedocumenteerde of "shadow" API-endpoints worden ontdekt via fuzzing?
- [ ] Nee — fuzzing van directories en endpoints leverde geen ongedocumenteerde paden op  
- [ ] Ja — ongedocumenteerde endpoints zijn gevonden maar kunnen **niet** worden geopend zonder autorisatie  
- [ ] Ja — ongedocumenteerde endpoints zijn gevonden en **kunnen** worden geopend zonder geldige autorisatie  

### Onthult de applicatie meerdere API-versies (bijv. /v1/, /v2/, /beta/)?
- [ ] Nee — alleen de huidige, geharde API-versie is toegankelijk  
- [ ] Ja — legacy-versies bestaan maar implementeren **dezelfde** beveiligingscontroles als de huidige versie  
- [ ] Ja — legacy-versies (bijv. `/v1/`) zijn toegankelijk en **missen** de beveiligingscontroles van nieuwere versies  

### Lekken client-side bronnen (JavaScript/mobiele apps) API-endpointstructuren of sleutels?
- [ ] Nee — client-side code bevat **geen** hardcoded API-paden of gevoelige sleutels  
- [ ] Ja — client-side code bevat API-endpoint mappings maar **geen** gevoelige sleutels  
- [ ] Ja — API-sleutels, secrets of interne endpoint-URL's **zijn** hardcoded in client-side bronnen  

### Zijn verborgen parameters of headers te ontdekken op bekende endpoints?
- [ ] Nee — parameter fuzzing (bijv. via `Arjun`) heeft **geen** verborgen invoer onthuld  
- [ ] Ja — verborgen parameters (bijv. `debug=true`, `admin=1`) zijn ontdekt maar zijn **niet** functioneel  
- [ ] Ja — verborgen parameters zijn ontdekt en **kunnen** worden gebruikt om het gedrag van de applicatie te wijzigen  

---