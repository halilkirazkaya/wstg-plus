## WSTG-ATHZ-04 — Testing for Insecure Direct Object References

Insecure Direct Object References (IDOR) treden op wanneer een applicatie directe toegang biedt tot objecten op basis van door de gebruiker verstrekte invoer, zonder een autorisatiecontrole uit te voeren om de machtigingen van de aanvrager te verifiëren. Deze kwetsbaarheid heeft een aanzienlijke impact op de vertrouwelijkheid en integriteit, aangezien het aanvallers in staat stelt om gegevens van andere gebruikers of het systeem in te zien, te wijzigen of te verwijderen. Het manifesteert zich doorgaans in URL-parameters, POST body data of JSON keys die verwijzen naar interne database keys, bestandsnamen of account-identifiers. Vanuit het perspectief van een aanvaller houdt exploitatie het manipuleren van deze identifiers in — vaak door middel van incrementele integers of het fuzzen van UUIDs — om toegang te krijgen tot bronnen buiten hun geautoriseerde scope.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### Gebruikt de applicatie directe object-identifiers in request-parameters?
- [ ] Nee — applicatie gebruikt indirecte referenties of versleutelde maps *(Meest veilig)*  
- [ ] Ja — applicatie gebruikt niet-voorspelbare identifiers (bijv. lange UUIDs/hashes)  
- [ ] Ja — applicatie gebruikt voorspelbare identifiers (bijv. incrementele integers of gebruikersnamen)  

### Wordt server-side autorisatie uitgevoerd voor elk verzoek waarbij een object-identifier betrokken is?
- [ ] Ja — server-side controles kunnen voor geen enkele identifier worden omzeild  
- [ ] Ja — server-side controles zijn aanwezig, maar omzeiling is mogelijk via parameter pollution of header-manipulatie  
- [ ] Nee — er wordt geen autorisatiecontrole toegepast om het eigendom van het opgevraagde object te verifiëren  

### Kan een aanvaller objecten van andere gebruikers benaderen of wijzigen (Horizontale IDOR)?
- [ ] Nee — toegang tot gegevens van andere gebruikers is niet mogelijk  
- [ ] Ja — het inzien van gegevens van andere gebruikers is mogelijk, maar wijziging is niet mogelijk  
- [ ] Ja — zowel het inzien als het wijzigen van gegevens van andere gebruikers is mogelijk *(Kritiek)*  

### Is het mogelijk om objecten op beheerders- of systeemniveau te benaderen (Verticale IDOR)?
- [ ] Nee — toegang tot objecten met hogere privileges is niet mogelijk  
- [ ] Ja — toegang tot beheerdersconfiguraties of systeembestanden is mogelijk  

### Lekt de applicatie object-identifiers via zoekfuncties, metadata of andere endpoints?
- [ ] Nee — identifiers worden niet onbedoeld blootgesteld  
- [ ] Ja — identifiers voor andere gebruikers/objecten kunnen worden geënumereerd (enumeration) via secundaire endpoints of publieke profielen  

---