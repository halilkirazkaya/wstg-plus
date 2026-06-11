## WSTG-CLNT-12 — Test Browser Storage

Het testen van browseropslag omvat het analyseren van de wijze waarop een applicatie gebruikmaakt van client-side mechanismen zoals LocalStorage, SessionStorage, IndexedDB en WebSQL om gegevens te behouden. Onveilig opgeslagen gevoelige informatie — inclusief PII, authentication tokens of session identifiers — kan worden gecompromitteerd als een aanvaller Cross-Site Scripting (XSS) uitvoert of fysieke toegang verkrijgt tot het apparaat van de gebruiker. Pentesters moeten vaststellen of gegevens in cleartext worden opgeslagen, of ze toegankelijk blijven na het uitloggen en of de opslagcyclus op de juiste manier wordt beheerd om datalekken te voorkomen. Vanuit het perspectief van een aanvaller zijn deze opslagmechanismen lucratieve doelen voor het exfiltreren van statusinformatie waarmee session hijacking of langdurige account-persistentie mogelijk wordt.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als session tokens of gevoelige PII worden opgeslagen in LocalStorage en toegankelijk zijn via XSS.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### Slaat de applicatie gevoelige informatie op in de browseropslag?
- [ ] Nee — de applicatie gebruikt geen browseropslag of slaat alleen niet-gevoelige UI-status op  
- [ ] Ja — gevoelige gegevens worden opgeslagen maar zijn versleuteld met robuuste client-side cryptografie  
- [ ] Ja — gevoelige gegevens (tokens, PII, secrets) worden opgeslagen in **cleartext** *(Gemiddeld)*  

### Zijn de opgeslagen gegevens toegankelijk voor niet-geautoriseerde scripts (XSS-risico)?
- [ ] Nee — gegevens worden opgeslagen in HttpOnly cookies (niet in browseropslag) of opslag wordt niet gebruikt  
- [ ] Ja — LocalStorage/SessionStorage wordt gebruikt, waardoor gegevens **volledig toegankelijk** zijn via elke XSS Payload *(Hoog)*  

### Wordt de browseropslag op de juiste manier gewist bij het uitloggen van de gebruiker?
- [ ] Ja — alle applicatiegerelateerde opslag wordt **expliciet gewist** tijdens het uitlogproces  
- [ ] Nee — opslag blijft behouden na uitloggen, maar bevat geen gevoelige sessiegegevens  
- [ ] Nee — authentication tokens of PII **blijven aanwezig** in de opslag nadat de sessie is beëindigd *(Gemiddeld)*  

### Gebruikt de applicatie IndexedDB of WebSQL voor grote/gevoelige datasets?
- [ ] Nee — deze opslagmechanismen worden niet gebruikt  
- [ ] Ja — gegevens worden opgeslagen en **correct gesaneerd** voordat ze in de DOM worden gerenderd  
- [ ] Ja — gevoelige datasets worden in **cleartext** opgeslagen binnen IndexedDB/WebSQL-structuren  

### Kan de integriteit van de opgeslagen gegevens worden gemanipuleerd om de applicatielogica te beïnvloeden?
- [ ] Nee — de applicatie valideert of ondertekent gegevens voordat ze vanuit de opslag worden verwerkt  
- [ ] Ja — het wijzigen van opslagwaarden (bijv. gebruikersrollen, flags) **is mogelijk** en resulteert in gewijzigd gedrag van de applicatie  

---