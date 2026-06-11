## WSTG-INFO-01 — Uitvoeren van zoekmachine-onderzoek en verkenningsactiviteiten naar informatielekken

Zoekmachine-onderzoek omvat het gebruik van openbare zoekmachines, gecachte pagina's en indexeringsdiensten om informatie te identificeren die de doelorganisatie onbedoeld heeft blootgesteld. Aanvallers gebruiken geavanceerde zoekoperators (`Google Dorks`) en indexeringsdiensten van derden om gevoelige bestanden, mappen, inlogportalen, foutmeldingen en metadata te ontdekken die niet openbaar toegankelijk zouden mogen zijn. Deze verkenningsfase is cruciaal omdat er geen directe interactie met het doelwit vereist is, waardoor het vrijwel ondetecteerbaar is, terwijl het potentieel waardevolle toegangspunten onthult zoals blootgestelde beheerpanelen, configuratiebestanden, database-dumps en interne documentatie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Worden gevoelige bestanden of mappen geïndexeerd door zoekmachines?
- [ ] Nee — geen gevoelige inhoud gevonden in de resultaten van zoekmachines  
- [ ] Ja — configuratiebestanden, back-ups of interne documenten **worden** geïndexeerd *(Kritiek)*  

### Onthult Google Dorking blootgestelde beheerders- of inloginterfaces?
- [ ] Nee — beheerpanelen en inlogpagina's zijn **niet** geïndexeerd  
- [ ] Ja — beheerpanelen **zijn** geïndexeerd maar vereisen sterke multifactorauthenticatie  
- [ ] Ja — beheerpanelen **zijn** geïndexeerd en openbaar toegankelijk **zonder** voldoende controles  

### Worden via gecachte versies van pagina's gevoelige gegevens blootgesteld die inmiddels zijn verwijderd?
- [ ] Nee — geen gevoelige gegevens gevonden in gecachte snapshots  
- [ ] Ja — gecachte pagina's bevatten inloggegevens, interne IP-adressen of gevoelige informatie  

### Onthult het `robots.txt` bestand onbedoeld gevoelige paden?
- [ ] Nee — `robots.txt` onthult **geen** gevoelige mapstructuren  
- [ ] Ja — `robots.txt` vermeldt gevoelige paden die de verkenning door een aanvaller vergemakkelijken  
- [ ] Er bestaat geen `robots.txt` bestand  

### Onthullen diensten van derden (`Shodan`, `Censys`, `Wayback Machine`) historische of huidige blootstelling?
- [ ] Nee — geen significante bevindingen via indexeringsdiensten van derden  
- [ ] Ja — historische snapshots of scans van diensten onthullen gevoelige metadata of service-banners  

---