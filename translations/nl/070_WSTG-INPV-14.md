## WSTG-INPV-14 — Testing for Incubated Vulnerabilities

Incubated vulnerabilities, ook wel persistente of second-order kwetsbaarheden genoemd, treden op wanneer een kwaadaardige payload door de applicatie in een "cold" state wordt opgeslagen en later wordt uitgevoerd of verwerkt in een andere context. Dit aanvalspatroon richt zich doorgaans op back-end systemen, administratieve consoles of geautomatiseerde rapportagetools die gegevens ophalen uit een gedeelde database of bestandssysteem zonder adequate her-validatie of output encoding. Pentesters moeten de levenscyclus van gebruikersinvoer volgen vanaf het punt van binnenkomst (de "incubator") naar elke locatie waar die gegevens uiteindelijk worden gerenderd of gebruikt door andere componenten of secundaire applicaties. Succesvolle exploitatie leidt vaak tot resultaten met een hoge impact, zoals administratieve session hijacking via Stored XSS of data-exfiltratie via second-order SQL Injection in interne rapportagemodules.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Tools:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Zijn er endpoints waar door de gebruiker gecontroleerde invoer wordt opgeslagen voor latere ophaling door andere gebruikers of processen?
- [ ] Nee — de applicatie slaat geen gebruikersinvoer op voor later gebruik  
- [ ] Ja — invoer wordt opgeslagen in databasevelden, logbestanden of configuratie-instellingen  

### Wordt invoervalidatie of sanitization toegepast voordat gegevens naar de persistente opslaglaag worden geschreven?
- [ ] Ja — strikte allow-list validatie wordt toegepast op het punt van binnenkomst  
- [ ] Ja — generieke sanitization wordt toegepast, maar een bypass is mogelijk via encoding of niet-standaard karakters  
- [ ] Nee — invoer wordt opgeslagen in zijn ruwe, niet-gevalideerde vorm  

### Worden opgeslagen gegevens correct ge-encodeerd of gesaneerd wanneer ze worden gerenderd in administratieve of secundaire interfaces?
- [ ] Ja — contextbewuste output encoding wordt overal toegepast waar de gegevens worden gerenderd  
- [ ] Ja — encoding wordt op sommige locaties toegepast, maar ontbreekt op andere (bijv. intern admin-dashboard)  
- [ ] Nee — gegevens worden gerenderd zonder enige encoding of sanitization, wat de uitvoering van payloads mogelijk maakt  

### Kunnen payloads die in de database zijn opgeslagen invloed hebben op back-end batchprocessen of out-of-band monitoringsystemen?
- [ ] Nee — back-end processen maken gebruik van veilige parsing-methoden of geparametriseerde queries  
- [ ] Ja — opgeslagen payloads kunnen interacties in de back-end logica triggeren (bijv. CSV injection, command injection in logs)  
- [ ] Ja — opgeslagen payloads kunnen out-of-band (OOB) verzoeken naar door de aanvaller gecontroleerde servers triggeren  

---