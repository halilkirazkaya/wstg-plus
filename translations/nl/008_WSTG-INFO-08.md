## WSTG-INFO-08 — Fingerprint Web Application Framework

Het fingerprinten van een webapplicatie-framework houdt in dat de specifieke technologiestack en softwareversies worden geïdentificeerd die zijn gebruikt om de applicatie te bouwen en te hosten. Aanvallers analyseren HTTP-response headers, framework-specifieke cookies, voorspelbare bestandsstructuren en unieke JavaScript-artefacten om te bepalen of het doelwit draait op platforms zoals React, Angular, Django of Spring. Deze verkenningsfase (reconnaissance) is essentieel omdat het een tester in staat stelt het aanvalsoppervlak (attack surface) te verkleinen tot bekende kwetsbaarheden (CVE's) en veelvoorkomende configuratiezwakheden die specifiek zijn voor dat framework. Door de onderliggende architectuur te begrijpen, kan een aanvaller overstappen van generieke testen naar zeer gerichte exploitatiestrategieën.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Informatief |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Onthullen HTTP-response headers de naam of versie van het framework?
- [ ] Nee — headers zoals `X-Powered-By` of `Server` zijn **niet** aanwezig of zijn gesaneerd (sanitized)  
- [ ] Ja — de naam van het framework is aanwezig, maar de versie **is verborgen**  
- [ ] Ja — zowel de naam van het framework als de versie **worden onthuld**  

### Zijn framework-specifieke cookies of directorystructuren identificeerbaar?
- [ ] Nee — veelvoorkomende framework-artefacten (bijv. `.js` paden, cookienamen) zijn geobfusceerd of hernoemd  
- [ ] Ja — framework-specifieke cookies (bijv. `JSESSIONID`, `PHPSESSID`, `csrftoken`) **zijn** identificeerbaar  
- [ ] Ja — directorystructuren (bijv. `/wp-content/`, `_next/static/`) **zijn** zichtbaar en bevestigen het framework  

### Onthullen foutmeldingen of commentaren in de broncode details over het framework?
- [ ] Nee — foutafhandeling (error handling) is generiek en commentaren zijn verwijderd  
- [ ] Ja — framework-specifieke stack traces **staan aan** en zijn zichtbaar in responses  
- [ ] Ja — de HTML-broncode bevat commentaren of metadata-tags die het framework identificeren  

### Zijn er bekende kwetsbaarheden geassocieerd met de geïdentificeerde framework-versie?
- [ ] Nee — het geïdentificeerde framework is up-to-date en er zijn **geen bekende** publieke kwetsbaarheden  
- [ ] Ja — de framework-versie is verouderd en specifieke CVE's **kunnen worden** geïdentificeerd  

---