## WSTG-CONF-04 — Controle van oude back-upbestanden en niet-gerefereerde bestanden op gevoelige informatie

Het controleren van oude back-upbestanden en niet-gerefereerde bestanden omvat het identificeren van vergeten, tijdelijke of verborgen bestanden op een webserver die niet bedoeld zijn voor publieke toegang. Deze bestanden bevatten vaak broncode-back-ups (`.zip`, `.bak`), swap-bestanden van teksteditors (`.swp`, `~`) of source control metadata (`.git`, `.svn`) die gevoelige informatie kunnen lekken. Aanvallers gebruiken geautomatiseerde wordlists en discovery-tools om via Brute Force veelvoorkomende naamgevingsconventies te raden, met als doel database-credentials, hardcoded API-keys of logica die verdere Exploitatie vergemakkelijkt te exfiltreren. Deze kwetsbaarheid komt doorgaans voort uit handmatig serveronderhoud of gebrekkige CI/CD-pipelines die de productie web root niet correct opschonen.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Gemiddeld / Hoog* |

> *De Severity wordt Hoog wanneer configuratiebestanden, broncode-archieven of credentials worden ontdekt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Zijn directory listings ingeschakeld op de webserver?
- [ ] Nee — directory listing is **uitgeschakeld** en retourneert een 403 Forbidden of een aangepaste foutmelding  
- [ ] Ja — directory listing is **ingeschakeld** op niet-gevoelige mappen  
- [ ] Ja — directory listing is **ingeschakeld** op mappen die broncode of gevoelige bestanden bevatten *(Kritiek)*  

### Zijn back-upbestanden met veelvoorkomende extensies (bijv. .bak, .old, .save) vindbaar?
- [ ] Nee — veelvoorkomende back-up-extensies zijn **niet gevonden** of worden geblokkeerd door de serverconfiguratie  
- [ ] Ja — back-upbestanden bestaan, maar bevatten **geen** gevoelige informatie  
- [ ] Ja — back-upbestanden voor configuratie of broncode **zijn** toegankelijk *(Hoog)*  

### Zijn source control-directories (bijv. .git, .svn) of metadata blootgesteld?
- [ ] Nee — source control metadata is **niet aanwezig** of wordt correct geblokkeerd  
- [ ] Ja — metadata bestaat, maar de volledige repository kan **niet** worden gereconstrueerd  
- [ ] Ja — de volledige broncode-repository **kan** worden geëxfiltreerd via blootgestelde metadata  

### Zijn er gecomprimeerde archieven (bijv. .zip, .tar.gz, .rar) van de applicatie aanwezig in de web root?
- [ ] Nee — geen gevoelige archiefbestanden ontdekt via Brute Force of enumeratie  
- [ ] Ja — archieven gevonden, maar deze zijn beveiligd met een wachtwoord of bevatten publieke bestanden  
- [ ] Ja — onversleutelde archieven met de broncode of gegevens van de applicatie **zijn** publiekelijk toegankelijk  

### Lekt de applicatie tijdelijke bestanden die zijn aangemaakt door teksteditors of IDE's?
- [ ] Nee — tijdelijke bestanden zoals `.swp`, `~` of `.DS_Store` zijn **niet aanwezig**  
- [ ] Ja — niet-gevoelige tijdelijke bestanden zijn **aanwezig**  
- [ ] Ja — tijdelijke bestanden onthullen fragmenten van broncode of interne paden  

---