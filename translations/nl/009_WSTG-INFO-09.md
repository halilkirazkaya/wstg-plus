## WSTG-INFO-09 — Fingerprinting van webapplicaties

Het fingerprinten van een webapplicatie is het proces van het identificeren van het specifieke framework, Content Management System (CMS) of de onderliggende technologiestack en de bijbehorende versies. Deze verkenningsfase (reconnaissance) is essentieel omdat het een aanvaler in staat stelt om geïdentificeerde componenten te koppelen aan bekende kwetsbaarheden (CVE's) en publieke exploits, waardoor het aanvalsoppervlak aanzienlijk wordt verkleind. Informatie wordt doorgaans verkregen uit HTTP-responsheaders, unieke bestandspaden, cookienamen, metadata in de HTML-broncode en cryptografische hashes van statische assets. Door de technologiestack nauwkeurig te bepalen, kan een aanvaller overstappen van generieke geautomatiseerde scanning naar zeer gerichte exploitatie van versiespecifieke zwakheden.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Informatief |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Onthullen HTTP-responsheaders de technologiestack of versienummers?
- [ ] Nee — gevoelige headers zijn verwijderd of bevatten generieke waarden  
- [ ] Ja — standaardheaders zoals `X-Powered-By`, `Server` of `X-AspNet-Version` **zijn** aanwezig  
- [ ] Ja — headers onthullen specifieke, verouderde versies van de webserver of het applicatieframework  

### Is het applicatieframework of CMS identificeerbaar via unieke bestandspaden of naamgevingsconventies?
- [ ] Nee — bestandspaden zijn geobfusceerd en onthullen de onderliggende technologie **niet**  
- [ ] Ja — gangbare mappenstructuren (bijv. `/wp-content/`, `/_next/`, `/node_modules/`) **zijn** zichtbaar  
- [ ] Ja — unieke bestanden zoals `robots.txt`, `humans.txt` of `sitemap.xml` bevatten technologiespecifieke metadata  

### Worden specifieke versienummers blootgesteld in de HTML-broncode of statische assets?
- [ ] Nee — er is geen versie-informatie gevonden in commentaren of asset-parameters  
- [ ] Ja — HTML-commentaren of meta-tags (bijv. `<meta name="generator" content="...">`) **zijn** aanwezig  
- [ ] Ja — versiestrings zijn toegevoegd aan CSS/JS-bestandsnamen (bijv. `jquery.js?v=1.12.4`) en **kunnen niet** worden uitgeschakeld  

### Onthullen standaard foutpagina's of beheerinterfaces de softwareomgeving?
- [ ] Nee — de applicatie gebruikt aangepaste, generieke foutpagina's voor alle statuscodes  
- [ ] Ja — standaard foutpagina's voor de webserver of het framework **zijn** ingeschakeld en lekken versiegegevens  
- [ ] Ja — administratieve loginportalen (bijv. `/wp-admin`, `/phpmyadmin`) **zijn** toegankelijk en identificeerbaar  

### Onthullen cookies het framework voor sessiebeheer?
- [ ] Nee — sessiecookies gebruiken generieke of aangepaste namen  
- [ ] Ja — technologiespecifieke cookienamen (bijv. `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) **worden** gebruikt  

---