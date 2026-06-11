## WSTG-ATHZ-02 — Testen op het omzeilen van het autorisatieschema

Het omzeilen van autorisatieschema's vindt plaats wanneer een applicatie er niet in slaagt toegangscontroles af te dwingen, waardoor een aanvaller toegang krijgt tot resources of functies buiten de beoogde permissies. Deze kwetsbaarheid manifesteert zich doorgaans als Horizontal Privilege Escalation, waarbij een aanvaller toegang krijgt tot gegevens van een andere gebruiker met dezelfde rang, of Vertical Privilege Escalation, waarbij een gebruiker met lage privileges administratieve mogelijkheden verkrijgt. Aanvallers misbruiken deze gebreken door verzoekparameters zoals resource IDs te manipuleren, sessietokens te wijzigen of HTTP-headers zoals `X-Original-URL` te gebruiken om padgebaseerde beperkingen te omzeilen. Het waarborgen van robuuste autorisatie is essentieel voor het behoud van de vertrouwelijkheid van gegevens en het voorkomen van ongeautoriseerde administratieve acties die het gehele systeem in gevaar zouden kunnen brengen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Is horizontal privilege escalation voorkomen tussen gebruikers met dezelfde rol?
- [ ] Ja — autorisatiecontroles **worden toegepast** op elk resourceverzoek en omzeiling is **niet mogelijk**  
- [ ] Ja — autorisatiecontroles **worden toegepast**, maar kunnen worden omzeild via IDOR of parameter tampering  
- [ ] Nee — gebruikers **hebben toegang** tot gegevens van andere gebruikers door simpelweg een resource ID te wijzigen *(Hoog)*  

### Is vertical privilege escalation voorkomen voor gebruikers met lage privileges?
- [ ] Nee — de applicatie heeft slechts één privilegeniveau  
- [ ] Ja — administratieve functies zijn **niet toegankelijk** voor gebruikers met lage privileges  
- [ ] Ja — administratieve functies zijn verborgen in de UI, maar **kunnen** worden geraadpleegd via een directe URL  
- [ ] Nee — gebruikers met lage privileges **kunnen** administratieve acties uitvoeren door rollen of permissies te manipuleren *(Kritiek)*  

### Kan autorisatie worden omzeild met behulp van HTTP verb tampering?
- [ ] Nee — toegangscontroles worden afgedwongen ongeacht de gebruikte HTTP-methode  
- [ ] Ja — het wijzigen van de methode (bijv. van `GET` naar `POST` of `PUT`) **omzeilt** de autorisatiecontroles  

### Zijn administratieve of beperkte endpoints toegankelijk zonder enige authenticatie?
- [ ] Nee — alle gevoelige endpoints vereisen een geldige sessie en de juiste permissies  
- [ ] Ja — sommige administratieve endpoints zijn toegankelijk als de aanvaller de directe URL kent  
- [ ] Ja — ongeauthenticeerde toegang tot gevoelige functies **is mogelijk** *(Kritiek)*  

### Maken HTTP-headers het mogelijk om padgebaseerde autorisatie te omzeilen?
- [ ] Nee — de applicatie is **niet** vatbaar voor op headers gebaseerde omzeilingen  
- [ ] Ja — headers zoals `X-Original-URL`, `X-Rewrite-URL`, of `X-Forwarded-For` **kunnen** worden gebruikt om toegangscontroles te omzeilen  

---