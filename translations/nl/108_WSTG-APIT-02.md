## WSTG-APIT-02 — API Broken Object Level Authorization

Broken Object Level Authorization (BOLA), ook wel bekend als Insecure Direct Object Reference (IDOR), treedt op wanneer een API nalaat te valideren of een gebruiker de juiste machtigingen heeft om toegang te krijgen tot of controle uit te oefenen over een specifieke resource die wordt geïdentificeerd door een ID. Aanvallers maken hier misbruik van door systematisch identifiers te enumereren of te raden—zoals numerieke ID's of UUIDs—in request paths, query parameters of JSON bodies om toegang te krijgen tot gegevens van andere gebruikers. Deze kwetsbaarheid is het meest voorkomende en impactvolle probleem in moderne API-beveiliging en kan leiden tot massale data exfiltration, ongeautoriseerde wijzigingen of volledige account takeover. Vanuit het perspectief van een aanvaller is het doel om endpoints te identificeren die object-identifiers accepteren en te testen of de server-side autorisatielogica ontbreekt of kan worden omzeild door die ID's te manipuleren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Tools:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Zijn object-identifiers voorspelbaar of opsombaar binnen API-requests?
- [ ] Nee — identifiers zijn lang, willekeurig en cryptografisch veilig (bijv. UUIDv4)  
- [ ] Ja — identifiers zijn sequentiële gehele getallen (bijv. `101`, `102`)  
- [ ] Ja — identifiers volgen een voorspelbaar patroon of zijn afgeleid van openbare informatie  

### Voert de API server-side validatie uit op objecteigendom voor elk request?
- [ ] Ja — autorisatiecontroles worden toegepast voor elk request en bypass is **niet mogelijk**  
- [ ] Ja — controles worden toegepast maar een bypass **is mogelijk** via parameter pollution of method tunneling  
- [ ] Nee — de applicatie vertrouwt uitsluitend op het feit dat de gebruiker geauthenticeerd is zonder het eigendom van de specifieke resource te controleren  

### Is het mogelijk om toegang te krijgen tot de resources van een andere gebruiker of deze te wijzigen door de identifier te veranderen?
- [ ] Nee — ongeautoriseerde toegang tot resources van andere gebruikers is **niet mogelijk**  
- [ ] Ja — ongeautoriseerde **lees-toegang** (Horizontale BOLA) **is mogelijk**  
- [ ] Ja — ongeautoriseerde **wijziging** of **verwijdering** (Horizontale BOLA) **is mogelijk**  

### Staat de API toegang tot administratieve of systeem-objecten toe door ID's te vervangen?
- [ ] Nee — administratieve resources worden beschermd door secundaire autorisatielagen  
- [ ] Ja — toegang krijgen tot of het wijzigen van resources op systeemniveau (Verticale BOLA) **is mogelijk**  

### Kan de autorisatiecontrole worden omzeild door de identifier naar een ander deel van het request te verplaatsen?
- [ ] Nee — de autorisatielogica is consistent, ongeacht de locatie van de parameter  
- [ ] Ja — bypass **is mogelijk** wanneer de ID wordt verplaatst van het URL-pad naar de JSON body of headers  
- [ ] Ja — bypass **is mogelijk** wanneer er meerdere ID's worden opgegeven en de server de ongeautoriseerde ID verwerkt  

---