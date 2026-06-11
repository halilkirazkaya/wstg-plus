## WSTG-CLNT-09 — Testen op Clickjacking

Clickjacking, ook wel bekend als UI redressing, vindt plaats wanneer een aanvaller transparante of ondoorzichtige lagen gebruikt om een gebruiker te misleiden om op een knop of link op een andere pagina te klikken, terwijl de gebruiker de intentie had om op de bovenliggende pagina te klikken. Deze kwetsbaarheid tast de integriteit van gebruikersacties aan, wat kan leiden tot ongeautoriseerde gegevenswijziging, accountaanpassingen of financiële overschrijvingen die namens het slachtoffer worden uitgevoerd. Aanvallers exploiteren dit meestal door de doelwebsite in te sluiten in een `<iframe>` op een gecontroleerde kwaadaardige site en deze te overlappen met misleidende inhoud of verleidelijke lokmiddelen. Het komt het vaakst voor op pagina's die gevoelige, statuswijzigende acties uitvoeren, zoals "Account verwijderen"-knoppen, formulieren voor het opnieuw instellen van wachtwoorden of administratieve configuratiepanelen.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Medium / Hoog* |

> *De ernst wordt Hoog als gevoelige acties (bijv. geldovermakingen, accountverwijdering of privilege-escalatie) kunnen worden getriggerd via clickjacking.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Tools:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Implementeert de applicatie server-side headers om ongeautoriseerde framing te voorkomen?
- [ ] Ja — `X-Frame-Options` of `Content-Security-Policy` **zijn toegepast** en voorkomen framing *(Meest veilig)*  
- [ ] Ja — headers zijn aanwezig maar **verkeerd geconfigureerd** (bijv. `X-Frame-Options: ALLOW-FROM` die brede browserondersteuning mist)  
- [ ] Nee — er zijn geen headers voor framing-beveiliging **toegepast**  

### Is de `Content-Security-Policy` (CSP) `frame-ancestors` richtlijn geïmplementeerd?
- [ ] Ja — `frame-ancestors 'none'` of `'self'` **is toegepast**  
- [ ] Ja — `frame-ancestors` is aanwezig maar **staat** onbetrouwbare of te brede origins toe  
- [ ] Nee — richtlijn **ontbreekt** of CSP is niet geïmplementeerd  

### Is de `X-Frame-Options` (XFO) header correct geconfigureerd voor ondersteuning van legacy browsers?
- [ ] Ja — `DENY` of `SAMEORIGIN` **is toegepast**  
- [ ] Ja — XFO is aanwezig maar wordt door moderne browsers **genegeerd** omdat er een CSP `frame-ancestors` richtlijn bestaat  
- [ ] Nee — XFO-header **ontbreekt** of is ingesteld op een onveilige waarde  

### Kunnen de framing-beveiligingen worden omzeild met bekende technieken?
- [ ] Nee — bypass **niet mogelijk** via standaardtechnieken (double-framing, etc.)  
- [ ] Ja — beveiliging **kan** worden omzeild vanwege zwakke regex of logica in de `frame-ancestors` whitelist  
- [ ] Ja — beveiliging **kan** worden omzeild omdat de applicatie uitsluitend vertrouwt op JavaScript-gebaseerde frame-busting  

### Is een gevoelige actie exploiteerbaar via een clickjacking proof-of-concept?
- [ ] Nee — framing wordt geblokkeerd of er zijn geen gevoelige acties bereikbaar  
- [ ] Ja — UI redressing **is mogelijk** maar vereist complexe gebruikersinteractie  
- [ ] Ja — UI redressing **is mogelijk** en staat onmiddellijke statuswijzigende acties toe *(Kritiek)*  

---