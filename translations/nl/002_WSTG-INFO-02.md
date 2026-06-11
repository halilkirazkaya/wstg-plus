## WSTG-INFO-02 — Fingerprint Web Server

Webserver-fingerprinting is het proces van het identificeren van het specifieke softwaretype, de versie en het onderliggende besturingssysteem van een doel-webserver. Aanvallers voeren deze reconnaissance uit om bekende kwetsbaarheden te identificeren, zoals ongepatchte CVE's of configuratiezwakheden, die specifiek zijn voor bepaalde versies van Apache, Nginx, IIS of andere servertechnologieën. Dit wordt doorgaans bereikt door het analyseren van HTTP-responskoppen (bijv. `Server`, `X-Powered-By`), het onderzoeken van uniek protocolgedrag of het observeren van informatie die wordt gelekt via standaard foutpagina's en bestanden. Succesvolle fingerprinting stelt een aanvaller in staat om hun exploitatiestrategie aan te passen, overgaand van generiek scannen naar aanvallen met een hoge waarschijnlijkheid op succes tegen bekende architecturale fouten.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Informatief / Laag* |

> *De ernst wordt Hoog als de fingerprinting een verouderde of end-of-life (EOL) serverversie onthult met bekende kritieke kwetsbaarheden.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Zijn er identificerende strings aanwezig in de HTTP-responskoppen?
- [ ] Nee — `Server` en `X-Powered-By` headers zijn **niet aanwezig** of bevatten generieke waarden  
- [ ] Ja — headers onthullen serversoftware maar **geen** specifieke versienummers  
- [ ] Ja — headers onthullen specifieke serversoftware en **exacte** versienummers  

### Lekken standaard foutpagina's of door het systeem gegenereerde responsen servergegevens?
- [ ] Nee — er worden aangepaste foutpagina's gebruikt en deze onthullen **geen** serverinformatie  
- [ ] Ja — standaard foutpagina's lekken de naam van de serversoftware en/of de versie  
- [ ] Ja — foutpagina's lekken serverdetails, versie en het **onderliggende besturingssysteem**  

### Wordt de serverversie onthuld via standaardbestanden of mappenstructuren?
- [ ] Nee — standaard installatiebestanden, handleidingen en testscripts zijn **verwijderd**  
- [ ] Ja — standaardbestanden (bijv. `info.php`, `manual/`, `test.html`) zijn **aanwezig** en onthullen versiegegevens  

### Kan het servertype nauwkeurig worden afgeleid via een analyse van uniek gedrag?
- [ ] Nee — serverresponsen zijn genormaliseerd en op gedrag gebaseerde fingerprinting is **niet mogelijk**  
- [ ] Ja — uniek gedrag (bijv. volgorde van headers, specifieke foutcodes of TCP/IP-stackkenmerken) **maken** serveridentificatie mogelijk  

### Wordt de geïdentificeerde serverversie momenteel ondersteund en gepatcht?
- [ ] Ja — de geïdentificeerde versie is actueel en wordt **ondersteund** door de leverancier  
- [ ] Nee — de geïdentificeerde versie is **verouderd** of **end-of-life (EOL)** met bekende kwetsbaarheden  

---