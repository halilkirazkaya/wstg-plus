## WSTG-CONF-02 — Configuratietesten van het Applicatieplatform

Het testen van de configuratie van het applicatieplatform omvat het auditen van de instellingen van de onderliggende webserver, applicatieserver en het framework om te waarborgen dat deze gehard (hardened) zijn tegen veelvoorkomende exploits. Aanvallers zoeken actief naar standaard inloggegevens (default credentials), niet-gepatchte platformkwetsbaarheden en blootgestelde administratieve interfaces om ongeautoriseerde toegang te verkrijgen of code uit te voeren. Configuratiefouten leiden vaak tot de onthulling van gevoelige omgevingsvariabelen, interne systeempaden of de aanwezigheid van overbodige voorbeeldapplicaties die het aanvalsoppervlak vergroten. Vanuit een professioneel perspectief dient een slecht geconfigureerd platform als een toegangspunt met een hoge waarschijnlijkheid voor het verkrijgen van initiële toegang tot de doelomgeving.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog indien administratieve interfaces toegankelijk zijn met standaard inloggegevens of indien voorbeeldscripts Remote Code Execution mogelijk maken.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Zijn er standaard inloggegevens of accounts actief op het platform?
- [ ] Nee — alle standaardaccounts zijn **uitgeschakeld** of wachtwoorden zijn gewijzigd  
- [ ] Ja — standaardaccounts bestaan, maar zijn **niet toegankelijk** vanaf het externe netwerk  
- [ ] Ja — standaardaccounts zijn **actief** en toegankelijk via het internet *(Kritiek)*  

### Onthult het platform gevoelige versie-informatie via headers of foutpagina's?
- [ ] Nee — versiestrings zijn **verborgen** of generiek  
- [ ] Ja — gedetailleerde versie-informatie **wordt onthuld** in HTTP-headers (bijv. `Server`, `X-Powered-By`)  
- [ ] Ja — volledige stack traces en interne systeempaden **worden blootgesteld** in uitgebreide foutmeldingen  

### Zijn administratieve of beheerinterfaces op de juiste wijze beperkt?
- [ ] Nee — administratieve interfaces zijn **niet blootgesteld**  
- [ ] Ja — interfaces bestaan, maar zijn **beperkt** door IP allowlisting of Multi-Factor Authentication  
- [ ] Ja — interfaces (bijv. `/admin`, `/manager`, `/console`) zijn **publiekelijk toegankelijk** met single-factor authenticatie  

### Zijn er voorbeeldbestanden, scripts of documentatie aanwezig op de productieserver?
- [ ] Nee — alle niet-essentiële bestanden zijn **verwijderd**  
- [ ] Ja — voorbeeldbestanden of documentatie **zijn aanwezig**, maar lekken geen gevoelige informatie  
- [ ] Ja — voorbeeldscripts (bijv. `info.php`, `examples/`) **zijn aanwezig** en bieden een directe aanvalsvector  

### Is de server geconfigureerd om onveilige HTTP-methoden te ondersteunen?
- [ ] Nee — alleen `GET` en `POST` zijn **ingeschakeld**  
- [ ] Ja — potentieel risicovolle methoden zoals `PUT`, `DELETE` of `TRACE` zijn **ingeschakeld**, maar beperkt  
- [ ] Ja — risicovolle methoden zijn **ingeschakeld** en maken ongeautoriseerde bestandsaanpassing of diefstal van inloggegevens (credential theft) mogelijk  

---