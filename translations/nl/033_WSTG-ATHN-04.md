## WSTG-ATHN-04 — Testing for Bypassing Authentication Schema

Het omzeilen van het authenticatieschema omvat het identificeren en exploiteren van fouten in de applicatielogica die een aanvaller in staat stellen toegang te krijgen tot beschermde bronnen zonder geldige credentials te verstrekken. Deze kwetsbaarheid treedt meestal op wanneer de applicatie vertrouwt op client-side controles, nalaat om server-side autorisatie af te dwingen bij elk verzoek, of wanneer administratieve "backdoors" en debug endpoints openstaan in productieomgevingen. Aanvallers exploiteren deze zwakheden door forced browsing uit te voeren naar gevoelige URL's, HTTP-parameters zoals `authenticated=true` te manipuleren, of headers te spoofen om de applicatie te misleiden zodat deze aanneemt dat er al een sessie tot stand is gebracht. Succesvolle exploitatie resulteert in een volledige ineenstorting van de authenticatieperimeter, wat kan leiden tot ongeautoriseerde data-exfiltratie of een volledige systeemcompromis.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Kunnen gevoelige interne pagina's rechtstreeks worden bezocht zonder een actieve sessie?
- [ ] Nee — directe toegang resulteert in een omleiding naar login of een 403/404-respons  
- [ ] Ja — sommige gevoelige pagina's zijn toegankelijk maar verstrekken beperkte of niet-gevoelige gegevens  
- [ ] Ja — gevoelige administratieve of gebruikersspecifieke pagina's **zijn** volledig toegankelijk zonder authenticatie *(Kritiek)*  

### Vertrouwt de applicatie op client-side flags of parameters om de authenticatiestatus te bepalen?
- [ ] Nee — authenticatiestatus wordt strikt beheerd via de server-side sessiestatus  
- [ ] Ja — client-side flags zijn aanwezig, maar modificatie hiervan **verleent geen** toegang tot beschermde gebieden  
- [ ] Ja — het aanpassen van parameters (bijv. `is_authenticated=true`, `user_role=admin`) **maakt** het omzeilen van de authenticatie **mogelijk**  

### Kan authenticatie worden omzeild door het spoofen of manipuleren van specifieke HTTP-headers?
- [ ] Nee — headers zoals `X-Forwarded-For`, `Referer` of aangepaste headers hebben geen invloed op de authenticatie  
- [ ] Ja — het manipuleren van headers (bijv. `X-Custom-IP-Authorization`, `X-Remote-User`) **is mogelijk** en verleent ongeautoriseerde toegang  

### Zijn er alternatieve paden of ontwikkelingsartefacten die de standaard inlogflow omzeilen?
- [ ] Nee — alle beschermde bronnen worden beveiligd door een gecentraliseerde, productie-klare authenticatiecontrole  
- [ ] Ja — legacy endpoints, debug-routes (bijv. `/debug/login`) of vergeten API-versies **zijn** toegankelijk zonder credentials  

### Verzuimt de applicatie om opnieuw te authenticeren of sessies te verifiëren voor acties met hoge privileges?
- [ ] Nee — acties met hoge privileges of gevoelige acties vereisen een vers of geldig sessietoken  
- [ ] Ja — zodra een bypass is gevonden op één endpoint, **kan** deze worden gebruikt om administratieve acties uit te voeren in de gehele applicatie  

---