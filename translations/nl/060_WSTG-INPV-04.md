## WSTG-INPV-04 — Testen op HTTP Parameter Pollution

HTTP Parameter Pollution (HPP) treedt op wanneer een applicatie meerdere HTTP-parameters met dezelfde naam ontvangt en deze op een inconsistente of onveilige manier verwerkt. Door dubbele parameters op te geven, kan een aanvaller de interne logica van de applicatie manipuleren, waardoor Web Application Firewalls (WAF), inputvalidatiefilters of mechanismen voor toegangscontrole (Access Control) mogelijk worden omzeild. Deze kwetsbaarheid is sterk afhankelijk van de onderliggende webserver of het applicatie-framework, dat kan kiezen voor de eerste, de laatste of een samengevoegde (geconcateneerde) versie van de herhaalde parameters. Uitbuiting richt zich doorgaans op endpoints die parameters doorgeven aan back-end API's, databasequery's of URL-redirection-mechanismen, wat kan leiden tot impacts variërend van Cross-Site Scripting (XSS) tot ongeoorloofde data-exfiltratie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Hoe gaat de applicatie om met meerdere HTTP-parameters met dezelfde naam?
- [ ] Nee — dubbele parameters worden **geweigerd** of genegeerd door de server  
- [ ] Ja — de server selecteert consequent alleen de **eerste** of **laatste** instantie  
- [ ] Ja — de server **voegt waarden samen** (concatenatie), wat mogelijk injectie toestaat  

### Kan HPP worden ingezet om beveiligingsmaatregelen zoals een WAF of inputfilter te omzeilen?
- [ ] Nee — beveiligingsmaatregelen inspecteren correct **alle** instanties van een parameter  
- [ ] Ja — een WAF of filter inspecteert alleen de **eerste** instantie, waardoor een malafide **tweede** instantie kan passeren  
- [ ] Ja — door samenvoeging kan een aanvaller een **Payload splitsen** over meerdere parameters om detectie te ontwijken  

### Reflecteert de applicatie vervuilde parameters in de respons zonder de juiste sanitization?
- [ ] Nee — parameters worden gesaneerd of gecodeerd voordat ze in de UI worden gereflecteerd  
- [ ] Ja — vervuiling resulteert in gereflecteerde inhoud, maar sanitization **wordt toegepast**  
- [ ] Ja — vervuiling resulteert in gereflecteerde inhoud en sanitization **wordt niet toegepast**, wat leidt tot XSS of logische fouten  

### Zijn parameters die worden doorgegeven aan interne API-calls of back-end-systemen kwetsbaar voor vervuiling?
- [ ] Nee — back-end-requests maken gebruik van veilige, niet-dynamische constructiemethoden  
- [ ] Ja — HPP stelt een aanvaller in staat om parameters in de back-end-requeststructuur te **injecteren** of te **overschrijven**  

### Vertoont de applicatie ander gedrag wanneer parameters worden vervuild in GET- versus POST-requests?
- [ ] Nee — het gedrag is consistent over alle HTTP-methoden  
- [ ] Ja — alleen GET-parameters zijn kwetsbaar voor vervuiling  
- [ ] Ja — alleen POST (body) parameters zijn kwetsbaar voor vervuiling  

---