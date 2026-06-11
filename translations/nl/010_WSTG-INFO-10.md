## WSTG-INFO-10 — Applicatiearchitectuur in kaart brengen

Het in kaart brengen van de applicatiearchitectuur omvat het identificeren van de onderliggende technologieën, infrastructuurcomponenten en gegevensstromen die de webapplicatie ondersteunen. Door HTTP-responskoppen (headers), foutmeldingen, cookieformaten en bestandsextensies te analyseren, kunnen testers een schema reconstrueren van de gebruikte webservers, applicatieservers, databases en integraties van derden. Deze kennis is fundamenteel voor het afstemmen van daaropvolgende aanvallen, omdat het de selectie van platformspecifieke exploits en de identificatie van potentiële knelpunten of verkeerd geconfigureerde tussenliggende apparaten zoals Load Balancers en WAFs mogelijk maakt. Een aanvaller gebruikt deze structurele kaart om over te gaan van generiek scannen naar gerichte exploitatie van de specifieke softwarestack en de onderling verbonden componenten.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Informatief / Laag* |

> *De ernst wordt Gemiddeld (Medium) als de interne netwerktopologie of private IP-adressen worden onthuld.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Kunnen de webserver- en applicatieservertechnologieën nauwkeurig worden geïdentificeerd?
- [ ] Nee — geen identificatie **mogelijk** vanwege effectieve hardening of obfuscation  
- [ ] Ja — het technologietype is bekend, maar specifieke versies **kunnen niet** worden vastgesteld  
- [ ] Ja — technologie en specifieke versies **worden** volledig onthuld via headers of bestandsgegevens (file signatures) *(Informatief)*  

### Zijn tussenliggende apparaten (WAFs, Load Balancers, Proxies) detecteerbaar in het communicatiepad?
- [ ] Nee — er zijn geen tussenliggende apparaten gedetecteerd of ze zijn volledig transparant  
- [ ] Ja — het bestaan wordt vermoed via timing of gedrag, maar de identiteit **is niet** bevestigd  
- [ ] Ja — specifieke producten (bijv. Cloudflare, Nginx, F5) **zijn** geïdentificeerd via unieke headers of gedrag  

### Lekt de applicatie informatie over de interne mappenstructuur of netwerktopologie?
- [ ] Nee — interne paden en IP's worden **niet** onthuld  
- [ ] Ja — interne paden worden gelekt in foutmeldingen of stack traces, maar interne IP's **niet**  
- [ ] Ja — zowel interne mappenstructuren als private IP-adressen **worden** onthuld  

### Kan het type en de versie van de backend-database worden afgeleid uit het gedrag van de applicatie?
- [ ] Nee — databasegegevens **kunnen niet** worden vastgesteld  
- [ ] Ja — het databasetype (bijv. PostgreSQL, MSSQL) wordt afgeleid via foutmeldingen of gedrag  
- [ ] Ja — het databasetype en de versie **worden** expliciet onthuld in response bodies of headers  

### Wordt de applicatie gehost bij identificeerbare cloudproviders of specifieke CDN's van derden?
- [ ] Nee — de hostinginfrastructuur is **niet** identificeerbaar  
- [ ] Ja — de cloudprovider (AWS, Azure, GCP) is geïdentificeerd, maar specifieke diensten **niet**  
- [ ] Ja — specifieke clouddiensten (bijv. S3 buckets, Lambda, Azure Functions) **zijn** in kaart gebracht en bereikbaar  

---