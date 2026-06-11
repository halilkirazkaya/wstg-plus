## WSTG-SESS-01 — Testen van het Sessiebeheer-schema

Het testen van het sessiebeheer-schema richt zich op het analyseren van hoe de applicatie de levenscyclus en structuur van sessie-identificatoren afhandelt om ervoor te zorgen dat deze robuust zijn tegen voorspelling en overname (hijacking). Aanvallers onderscheppen meerdere sessietokens om statistische analyses uit te voeren, waarbij ze zoeken naar patronen, lage entropie (entropy) of voorspelbare toenames die hen in staat stellen om geldige sessies voor andere gebruikers te vervalsen. Zwakheden in het schema uiten zich vaak als sessiefixatie (Session Fixation) kwetsbaarheden of het gebruik van onvoldoende willekeurige waarden, die doorgaans worden aangetroffen in HTTP-cookies, URL-parameters of aangepaste header-implementaties. Het compromitteren van het sessieschema is een aanval met een hoge impact die een aanvaller volledige toegang geeft tot de geauthenticeerde status van een slachtoffer zonder dat de primaire inloggegevens nodig zijn.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Is de sessie-identificator voldoende complex en willekeurig?
- [ ] Ja — token slaagt voor statistische willekeurigheidstests (hoge entropie) en een bypass **is niet mogelijk**  
- [ ] Ja — token is lang maar bevat voorspelbare patronen of statische segmenten  
- [ ] Nee — token is sequentieel, gebaseerd op voorspelbare timestamps, of heeft een **kritiek lage** entropie *(Kritiek)*  

### Waar wordt de sessie-identificator verzonden en opgeslagen?
- [ ] Identificator wordt opgeslagen in een `Secure` en `HttpOnly` cookie *(Meest veilig)*  
- [ ] Identificator wordt opgeslagen in een cookie maar mist de `Secure` of `HttpOnly` flags  
- [ ] Identificator wordt **alleen** verzonden via URL-parameters of `GET`-verzoeken *(Hoog risico)*  

### Genereert de applicatie een nieuwe sessie-identificator bij een succesvolle authenticatie?
- [ ] Ja — een nieuw, uniek sessie-ID **wordt gegenereerd** onmiddellijk na het inloggen  
- [ ] Nee — het sessie-ID blijft hetzelfde voor en na het inloggen *(Sessiefixatie/Session Fixation mogelijk)*  

### Is de sessie-identificator gekoppeld aan een specifiek IP-adres of User-Agent om hergebruik te voorkomen?
- [ ] Ja — sessie wordt ongeldig gemaakt als de vingerafdruk (fingerprint) van de client aanzienlijk verandert  
- [ ] Nee — sessie **kan** worden hergebruikt over verschillende IP-adressen of apparaten zonder aanvullende verificatie  

### Worden sessie-identificatoren aan de serverzijde correct ongeldig gemaakt na uitloggen of time-out?
- [ ] Ja — sessiestatus aan de serverzijde wordt **volledig vernietigd** bij het uitloggen  
- [ ] Nee — sessie blijft geldig op de server, zelfs als de cookie aan de clientzijde wordt verwijderd  
- [ ] Nee — sessie **verloopt nooit** of heeft een buitensporig lange time-outperiode  

---