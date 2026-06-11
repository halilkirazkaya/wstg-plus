## WSTG-INPV-12 — Command Injection

Command Injection treedt op wanneer een applicatie onveilige, door de gebruiker verstrekte gegevens, zoals formulierinvoer, HTTP-headers of cookies, doorgeeft aan een system shell. Deze kwetsbaarheid stelt een aanvaller in staat om willekeurige besturingssysteemcommando's uit te voeren op de server, doorgaans met de rechten van het webapplicatieproces. Vanuit het perspectief van een aanvaller wordt dit misbruikt door shell-metatekens zoals puntkomma's, pipes of backticks te gebruiken om kwaadaardige commando's te koppelen aan de beoogde uitvoeringsstroom. De impact is vaak catastrofaal en leidt tot volledige systeemcompromittering, ongeautoriseerde gegevensexfiltratie of lateral movement binnen het interne netwerk.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Tools:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### Roept de applicatie commando's op systeemniveau of shell-executables aan?
- [ ] Nee — applicatie heeft **geen** interactie met de OS shell  
- [ ] Ja — applicatie maakt gebruik van interne API's of high-level libraries voor systeemtaken  
- [ ] Ja — applicatie roept rechtstreeks shell-commando's aan via system calls zoals `system()`, `exec()`, of `popen()`  

### Wordt gebruikersinvoer gevalideerd of gesaneerd voordat deze aan system calls wordt doorgegeven?
- [ ] Ja — strikte allow-listing en invoervalidatie **wordt toegepast**  
- [ ] Ja — sanering of escaping wordt gebruikt, maar een bypass **is mogelijk**  
- [ ] Nee — gebruikersinvoer wordt rechtstreeks naar de shell doorgegeven **zonder** validatie  

### Kunnen shell-metatekens worden gebruikt om de stroom van de commando-uitvoering te manipuleren?
- [ ] Nee — metatekens worden correct gefilterd of ge-escaped  
- [ ] Ja — in-band uitvoering waarbij de commando-output wordt gereflecteerd in de respons **is mogelijk**  
- [ ] Ja — blind uitvoering waarbij geen output wordt gereflecteerd **is mogelijk**  

### Is out-of-band (OOB) exfiltratie mogelijk vanaf de host?
- [ ] Nee — server heeft geen uitgaand verkeer (egress) of OOB-signalen (DNS/HTTP) falen  
- [ ] Ja — gegevensexfiltratie via DNS of HTTP-gebaseerde OOB-signalen **is mogelijk**  

### Draait het applicatieproces met verhoogde systeemprivileges?
- [ ] Nee — applicatie draait met een specifiek service-account met lage privileges  
- [ ] Ja — applicatie draait als `root`, `Administrator`, of een gebruiker met hoge privileges *(Kritiek)*  

---