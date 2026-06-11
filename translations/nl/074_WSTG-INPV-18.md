## WSTG-INPV-18 — Testen op Server-Side Template Injection

Server-Side Template Injection (SSTI) vindt plaats wanneer een applicatie door de gebruiker gecontroleerde input invoegt in een server-side template engine zonder voldoende sanitization of escaping. Dit stelt een aanvaller in staat om kwaadaardige template-directives te injecteren, die vervolgens op de server worden uitgevoerd, wat potentieel kan leiden tot een volledige systeemcompromis via remote code execution (RCE). SSTI manifesteert zich doorgaans in functionaliteiten zoals dynamische e-mailgeneratie, profielpagina's en notificatiesystemen die engines gebruiken zoals Jinja2, Twig of FreeMarker. Vanuit het perspectief van een aanvaller wordt deze kwetsbaarheid vaak misbruikt om gevoelige omgevingsvariabelen te lekken, interne bestanden te lezen of systeemcommando's uit te voeren door gebruik te maken van de native objecten en methoden van de template engine.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Tools:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### Maakt de applicatie gebruik van een server-side template engine om gebruikersinput te verwerken?
- [ ] Nee — applicatie gebruikt geen server-side templates voor dynamische content  
- [ ] Ja — templates worden gebruikt, maar gebruikersinput wordt **niet** ingebed in template-directives  
- [ ] Ja — gebruikersinput wordt ingebed in templates **zonder** de juiste sanitization  

### Worden basis rekenkundige expressies geëvalueerd wanneer ze in invoervelden worden geïnjecteerd?
- [ ] Nee — payloads zoals `{{7*7}}` of `${7*7}` worden weergegeven als letterlijke strings  
- [ ] Ja — expressies worden geëvalueerd (bijv. resultaat `49`), wat bevestigt dat de template engine **kwetsbaar is**  

### Kunnen de ingebouwde objecten of methoden van de template engine worden benaderd?
- [ ] Nee — toegang tot gevaarlijke objecten, methoden of configuraties is **beperkt** of vindt plaats in een sandbox  
- [ ] Ja — ingebouwde objecten (bijv. `self`, `config`, `__class__`) **kunnen** worden benaderd om gevoelige gegevens te lekken  
- [ ] Ja — remote code execution (RCE) via system calls of OS-libraries **is mogelijk**  

### Is er een sandbox- of allowlist-mechanisme aanwezig dat de uitvoering van gevaarlijke directives voorkomt?
- [ ] Ja — een strikte allowlist of beveiligde sandbox is aanwezig en een bypass is **niet mogelijk**  
- [ ] Ja — er is een sandbox aanwezig, maar een bypass **is mogelijk** via specifieke engine-afhankelijke payloads  
- [ ] Nee — er is **geen** sandbox of allowlist toegepast op de template engine  

---