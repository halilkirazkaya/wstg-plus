## WSTG-CONF-03 — Testen van de afhandeling van bestandsextensies voor gevoelige informatie

Het testen van de afhandeling van bestandsextensies omvat het identificeren of de webserver of applicatieserver gevoelige informatie prijsgeeft door bestanden aan te bieden die beperkt of uitgevoerd zouden moeten worden. Aanvallers zoeken regelmatig naar reservekopieën (backup files), configuratiebestanden en broncodefragmenten (bijv. `.bak`, `.old`, `.env`, `.inc`) die in de web root zijn achtergelaten en als platte tekst worden geserveerd vanwege een gebrek aan specifieke handler mappings. Deze kwetsbaarheid is van belang omdat het kan leiden tot de blootstelling van database-inloggegevens, API-sleutels en interne bedrijfslogica, wat diepere exploitatie van de infrastructuur vergemakkelijkt. Deze blootstellingen vinden doorgaans plaats in publieke mappen, uploadmappen of via verkeerd geconfigureerde MIME-type instellingen op de server.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als configuratiebestanden met inloggegevens of volledige broncode toegankelijk zijn.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Zijn gevoelige reservekopieën of tijdelijke bestandsextensies (bijv. `.bak`, `.old`, `.swp`, `~`) toegankelijk?
- [ ] Nee — de server retourneert 403 Forbidden of 404 Not Found voor algemene backup-extensies  
- [ ] Ja — de server staat het downloaden van backup-bestanden toe, maar deze bevatten **geen** gevoelige gegevens  
- [ ] Ja — de server staat het downloaden van backup-bestanden toe die gevoelige gegevens of broncode bevatten *(Hoog)*  

### Stelt de server omgevings- of systeemconfiguratiebestanden bloot (bijv. `.env`, `.config`, `.yml`, `.ini`)?
- [ ] Nee — beperkte configuratiebestanden zijn **niet** toegankelijk en retourneren 403/404  
- [ ] Ja — gevoelige configuratiebestanden **zijn** toegankelijk en onthullen interne geheimen of inloggegevens  

### Kan broncode worden opgevraagd door alternatieve extensies toe te voegen (bijv. `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] Nee — applicatiecode wordt **niet** als platte tekst weergegeven, ongeacht manipulatie van de extensie  
- [ ] Ja — broncode wordt onthuld omdat de server **geen** handler heeft voor de toegevoegde extensie  

### Zijn administratieve of metadata-mappen (bijv. `.git/`, `.svn/`, `.DS_Store`) geblokkeerd voor publieke toegang?
- [ ] Nee — deze mappen/bestanden bestaan **niet** in de web root  
- [ ] Ja — toegang is **niet mogelijk** vanwege server-side rewrite-regels of machtigingen  
- [ ] Ja — metadata-mappen **zijn** toegankelijk en maken volledige reconstructie van de broncode mogelijk  

### Hoe gaat de server om met verzoeken voor bestanden met meerdere extensies (bijv. `file.php.jpg`)?
- [ ] Nee — de server geeft correcte prioriteit aan de beveiliging van de interne extensie of blokkeert het verzoek  
- [ ] Ja — de server verwerkt het bestand als de eerste extensie, maar een bypass voor uitvoering is **niet mogelijk**  
- [ ] Ja — de server negeert de laatste extensie, waardoor uitvoering van code of onthulling van broncode **mogelijk is**  

---