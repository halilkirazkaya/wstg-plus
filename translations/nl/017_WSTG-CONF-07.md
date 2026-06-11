## WSTG-CONF-07 — Test HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) is een beveiligingsmechanisme waarmee een webserver browsers kan informeren dat deze alleen via HTTPS mag worden benaderd, waardoor protocol downgrade-aanvallen en cookie hijacking worden voorkomen. Door een versleutelde verbinding af te dwingen, beperkt HSTS Man-in-the-Middle (MitM) aanvallen zoals SSLStrip, waarbij een aanvaller een initiële onbeveiligde HTTP-aanvraag onderschept om de overgang naar TLS te voorkomen. Vanuit het perspectief van een aanvaller biedt de afwezigheid van deze header of een korte `max-age` duur een kans om gevoelige sessietokens of inloggegevens (credentials) te onderscheppen tijdens de initiële cleartext handshake. Deze test richt zich op het verifiëren van de aanwezigheid, duur en reikwijdte van de `Strict-Transport-Security` header op alle gevoelige applicatie-endpoints.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld* |

> *De ernst wordt Gemiddeld als de applicatie PII, sessietokens of credentials verwerkt, aangezien het ontbreken van HSTS het succespercentage van MitM-aanvallen verhoogt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### Is de `Strict-Transport-Security` header aanwezig in HTTPS-responses?
- [ ] Ja — header **is aanwezig** op alle gevoelige endpoints  
- [ ] Ja — header **is aanwezig**, maar alleen op de landingpagina  
- [ ] Nee — header **ontbreekt** volledig in de applicatie  

### Is de `max-age` directive ingesteld op een voldoende lange duur?
- [ ] Ja — `max-age` is ingesteld op een jaar of langer (bijv. `31536000`)  
- [ ] Ja — `max-age` is ingesteld, maar is **te kort** (bijv. minder dan 6 maanden)  
- [ ] Nee — `max-age` directive **ontbreekt** of is ingesteld op `0` *(Kritiek)*  

### Dekt de HSTS-policy alle subdomeinen?
- [ ] Ja — `includeSubDomains` directive **is toegepast**  
- [ ] Nee — `includeSubDomains` directive **ontbreekt**, waardoor subdomeinen kwetsbaar zijn voor downgrade  
- [ ] Nee — functie is niet van toepassing omdat er geen subdomeinen bestaan  

### Is de applicatie geregistreerd voor HSTS Preloading?
- [ ] Ja — `preload` directive **is aanwezig** en het domein staat op de HSTS preload-lijst  
- [ ] Ja — `preload` directive **is aanwezig**, maar het domein staat nog **niet** op de preload-lijst  
- [ ] Nee — `preload` directive **ontbreekt**, wat een window voor MitM toestaat bij het eerste bezoek  

### Wordt de HSTS-header onterecht verzonden over plaintext HTTP?
- [ ] Nee — header wordt **alleen** verzonden over beveiligde HTTPS-verbindingen  
- [ ] Ja — header **wordt verzonden** over HTTP, wat door browsers wordt genegeerd en configuratiedetails kan lekken  

---