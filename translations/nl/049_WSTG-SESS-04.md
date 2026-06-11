## WSTG-SESS-04 — Testen op Blootgestelde Sessievariabelen

Blootgestelde sessievariabelen treden op wanneer gevoelige sessie-identifiers of statusgerelateerde gegevens worden verzonden via onveilige kanalen, zoals URL-querystrings, Referer-headers of systeemlogs. Deze blootstelling verhoogt het risico op Session Hijacking aanzienlijk, aangezien identifiers kunnen worden opgeslagen in de browsergeschiedenis, gelogd door tussenliggende proxy's of gelekt naar websites van derden via de Referer-header. Pentesters identificeren deze blootstellingen door alle uitgaande verzoeken te monitoren en applicatielogs of serverconfiguraties te inspecteren op onbedoelde datalekken. In de praktijk maken aanvallers misbruik van deze lekken door geldige sessietokens te verzamelen van gedeelde systemen of door verkeerspatronen te analyseren om authenticatie te omzeilen en ongeautoriseerde toegang tot gebruikersaccounts te verkrijgen.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Teststatus** | Niet Uitgevoerd |
| **Severity** | Gemiddeld / Hoog* |

> *Severity wordt Hoog indien de blootgestelde variabele een sessietoken is dat directe account takeover mogelijk maakt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### Wordt de sessie-identifier verzonden in de URL-querystring?
- [ ] Nee — sessie-identifiers zijn **niet** aanwezig in de URL-querystring  
- [ ] Ja — identifiers zijn aanwezig, maar de sessie is kortstondig of van laag risico  
- [ ] Ja — unieke sessie-ID's **zijn** aanwezig in de URL-querystring *(Kritiek)*  

### Lekt de applicatie sessietokens via de Referer-header?
- [ ] Nee — `Referrer-Policy` voorkomt lekkage of er bestaan geen links naar derden  
- [ ] Ja — sessietokens **worden** verzonden naar domeinen van derden via de Referer-header  

### Worden sessievariabelen opgeslagen in server-side of applicatielogs?
- [ ] Nee — sessievariabelen zijn gemaskeerd of uitgesloten van logs  
- [ ] Ja — sessievariabelen **worden** in plain text opgenomen in applicatie- of webserverlogs  

### Gebruikt de applicatie GET-verzoeken voor statuswijzigende operaties?
- [ ] Nee — de applicatie gebruikt `POST` of andere niet-idempotente methoden voor gevoelige operaties  
- [ ] Ja — `GET` wordt gebruikt, waardoor sessiegegevens worden gecached in de browsergeschiedenis en tussenliggende proxy's  

### Is de `Cache-Control` header geconfigureerd om caching van sessiegerelateerde gegevens te voorkomen?
- [ ] Ja — `Cache-Control: no-store` (of vergelijkbaar) **is toegepast** op gevoelige antwoorden  
- [ ] Ja — controles zijn aanwezig, maar een bypass **is mogelijk** via browser edge cases  
- [ ] Nee — gevoelige antwoorden die sessiegegevens bevatten **kunnen** door de browser worden gecached  

---