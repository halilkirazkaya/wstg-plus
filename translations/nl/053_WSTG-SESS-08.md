## WSTG-SESS-08 — Session Puzzling

Session Puzzling, ook wel bekend als Session Variable Overloading, vindt plaats wanneer een applicatie dezelfde sessievariabele voor meerdere doeleinden gebruikt in verschillende modules, of nalaat om sessiestatussen correct te wissen tijdens workflow-overgangen. Aanvallers maken misbruik van dit gedrag door sessievariabelen in te vullen in de ene context — zoals een niet-geauthenticeerde profielweergave of een registratieproces met meerdere stappen — en vervolgens naar een beveiligd gedeelte te navigeren waar de applicatie deze reeds bestaande variabelen ten onrechte vertrouwt. Dit kan leiden tot kritieke gevolgen, waaronder authentication bypass, privilege escalation of ongeautoriseerde toegang tot gevoelige gegevens, door de applicatie te misleiden en te laten geloven dat een sessie zich in een hoger geprivilegieerde status bevindt dan in werkelijkheid het geval is. De kwetsbaarheid komt het meest voor in complexe, stateful applicaties waar de sessiebeheerlogica inconsistent wordt toegepast over verschillende functionele componenten.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### Gebruikt de applicatie dezelfde namen voor sessievariabelen voor verschillende doeleinden in afzonderlijke modules?
- [ ] Nee — variabelenamen zijn uniek of scopes zijn geïsoleerd  
- [ ] Ja — er bestaan gedeelde variabelenamen, maar de status wordt gewist tussen modules  
- [ ] Ja — er bestaan gedeelde variabelenamen en de status **wordt niet** gewist  

### Kunnen niet-geauthenticeerde acties sessievariabelen beïnvloeden die in geauthenticeerde contexten worden gebruikt?
- [ ] Nee — sessievariabelen worden pas geïnitialiseerd **na** een succesvolle authenticatie  
- [ ] Ja — sessievariabelen kunnen vóór het inloggen worden ingesteld, maar hebben **geen** invloed op de status na het inloggen  
- [ ] Ja — sessievariabelen die tijdens pre-authenticatie zijn ingesteld, **worden** vertrouwd na de authenticatie *(Kritiek)*  

### Herinitialiseert de applicatie de sessie-identifier en de bijbehorende variabelen bij een wijziging in het privilegeniveau?
- [ ] Ja — de sessie wordt volledig gereset en er **wordt** een nieuw ID uitgegeven  
- [ ] Gedeeltelijk — er wordt een nieuw ID uitgegeven, maar sommige sessievariabelen **blijven behouden**  
- [ ] Nee — het sessie-ID en alle variabelen **blijven** ongewijzigd  

### Kunnen beheerdersrechten worden verkregen door sessievariabelen te manipuleren via een account zonder privileges?
- [ ] Nee — privilege-variabelen **kunnen niet** door gebruikers worden gewijzigd  
- [ ] Ja — variabelen kunnen worden gewijzigd, maar de applicatie **valideert** deze tegen de backend-database  
- [ ] Ja — variabelen kunnen worden gewijzigd en de applicatie **vertrouwt** de sessiestatus impliciet *(Kritiek)*  

### Wordt de sessiestatus onmiddellijk gewist na het voltooien van een specifieke workflow (bijv. wachtwoord resetten, afrekenen)?
- [ ] Ja — sessievariabelen voor de workflow worden bij voltooiing vernietigd  
- [ ] Nee — workflow-variabelen **blijven behouden** en kunnen in andere delen van de applicatie worden hergebruikt  

---