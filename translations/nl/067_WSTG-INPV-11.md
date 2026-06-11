## WSTG-INPV-11 — Code Injection

Code-injectie treedt op wanneer een applicatie door de gebruiker aangeleverde codefragmenten evalueert binnen de eigen runtime-omgeving, meestal via onveilige taalspecifieke functies zoals `eval()`, `exec()`, of `system()`. Deze kwetsbaarheid verschilt van Command Injection omdat het zich richt op de executiecontext van de programmeertaal (bijv. PHP, Python, Node.js) in plaats van de shell van het onderliggende besturingssysteem. Aanvallers misbruiken deze punten om willekeurige logica uit te voeren, omgevingsvariabelen te exfiltreren of volledige Remote Code Execution (RCE) op de server te bewerkstelligen. Pentesters moeten functies onderzoeken die wiskundige expressies, server-side templates of dynamische configuratieparameters verwerken waarbij invoer geïnterpreteerd zou kunnen worden als uitvoerbare code.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Critical |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Tools:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Evalueren applicatiefuncties dynamische invoer via taalspecifieke evaluatiefuncties?
- [ ] Nee — dynamische evaluatiefuncties **zijn niet aanwezig** in de codebase  
- [ ] Ja — functies zoals `eval()`, `exec()`, of `include()` worden gebruikt, maar accepteren **geen** gebruikersinvoer  
- [ ] Ja — functies worden gebruikt en verwerken door de gebruiker aangeleverde invoer  

### Worden er mechanismen voor input-validatie of sanitisatie toegepast op de invoer vóór evaluatie?
- [ ] Ja — invoer wordt strikt gevalideerd tegen een **hardcoded whitelist**  
- [ ] Ja — invoer wordt gesaneerd via blacklisting, maar een bypass **is mogelijk**  
- [ ] Nee — invoer wordt direct doorgegeven aan de evaluatiefunctie en er worden **geen controles** toegepast  

### Is de executie-omgeving gesandboxed of beperkt om toegang op systeemniveau te voorkomen?
- [ ] Ja — de executie vindt plaats in een strikt beperkte sandbox waar **geen** system calls kunnen worden uitgevoerd  
- [ ] Ja — er bestaan enkele beperkingen, maar een sandbox-escape **is mogelijk**  
- [ ] Nee — de omgeving biedt volledige toegang tot de onderliggende taal-API's en OS-functies *(Critical)*  

### Kan de code-injectie worden gebruikt om gevoelige informatie te exfiltreren?
- [ ] Nee — de executie is blind en er is **geen** out-of-band communicatie mogelijk  
- [ ] Ja — omgevingsvariabelen of lokale bestanden **kunnen** worden geëxfiltreerd via HTTP/DNS-verzoeken  
- [ ] Ja — resultaten van de uitgevoerde code worden **direct gereflecteerd** in het antwoord van de applicatie  

### Staat de applicatie Remote File Inclusion of het dynamisch laden van bibliotheken toe?
- [ ] Nee — alleen lokale, vooraf gedefinieerde codepaden kunnen worden uitgevoerd  
- [ ] Ja — de applicatie **kan** worden gedwongen om code te laden en uit te voeren vanaf een externe of door de aanvaller beheerde bron  

---