## WSTG-INPV-08 — Testing for SSI Injection

Server-Side Includes (SSI) Injection vindt plaats wanneer een applicatie er niet in slaagt gebruikersinvoer op te schonen voordat deze wordt opgenomen in HTML-bestanden die worden verwerkt door de SSI-engine van de server. Aanvallers misbruiken dit door SSI-directives te injecteren, zoals `<!--#exec cmd="id" -->`, om willekeurige shell-commando's uit te voeren, lokale bestanden te lezen of omgevingsvariabelen te exfiltreren. Deze kwetsbaarheid wordt doorgaans aangetroffen in applicaties die verouderde bestandsextensies gebruiken zoals `.shtml`, `.shtm`, of `.stm`, of wanneer de webserver expliciet is geconfigureerd om SSI te parsen binnen standaard HTML-bestanden. Succesvolle exploitatie verleent de aanvaller dezelfde rechten als het webserverproces, wat vaak leidt tot een volledig systeemcompromis of de blootstelling van gevoelige gegevens.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Ondersteunt en verwerkt de webserver SSI-directives?
- [ ] Nee — server ondersteunt **geen** SSI of bestandsextensies zoals `.shtml` zijn **uitgeschakeld**  
- [ ] Ja — SSI **is ingeschakeld** maar gebruikersinvoer wordt **niet** gereflecteerd in verwerkte pagina's  
- [ ] Ja — SSI **is ingeschakeld** en gebruikersinvoer wordt **wel** gereflecteerd in verwerkte pagina's  

### Kunnen willekeurige systeemcommando's worden uitgevoerd via de `#exec` directive?
- [ ] Nee — `#exec` directive is **uitgeschakeld** of beperkt door de serverconfiguratie  
- [ ] Ja — uitvoering van commando's **is mogelijk** via `#exec cmd` of `#exec cgi` payloads  

### Is de exfiltratie van gevoelige bestanden of omgevingsvariabelen mogelijk?
- [ ] Nee — directives zoals `#include` of `#config` zijn **uitgeschakeld**  
- [ ] Ja — het lezen van lokale bestanden (bijv. `/etc/passwd`) **is mogelijk** via `#include virtual`  
- [ ] Ja — server-omgevingsvariabelen **kunnen** worden geëxfiltreerd via `#printenv` of `#echo`  

### Zijn invoervalidatie of WAF-controles effectief tegen SSI-payloads?
- [ ] Ja — strikte invoervalidatie **is toegepast** en omzeiling is **niet mogelijk**  
- [ ] Ja — validatie/WAF **is toegepast** maar omzeiling **is mogelijk** door gebruik te maken van karakter-encoding of afwijkende SSI-syntaxis  
- [ ] Nee — er is geen validatie of WAF-beveiliging aanwezig voor SSI-karakters (`<`, `!`, `#`, `-`, `"`)  

---