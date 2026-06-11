## WSTG-INPV-20 — Mass Assignment

Mass Assignment, ook bekend als Overposting of Insecure Deserialization van parameters, treedt op wanneer een webapplicatie inkomende HTTP-verzoekparameters automatisch koppelt aan interne objecteigenschappen zonder de juiste filtering van attributen. Deze kwetsbaarheid komt veel voor in moderne MVC-frameworks en RESTful API's waar JSON, XML, of formuliergecodeerde gegevens rechtstreeks worden gedeserialiseerd naar datamodellen aan de applicatiezijde. Aanvallers misbruiken dit gedrag door extra, niet-vermelde parameters in een verzoek te injecteren — zoals `isAdmin`, `role`, of `account_balance` — om gevoelige velden te wijzigen die de ontwikkelaars niet bedoeld hadden bloot te stellen aan gebruikerscontrole. Succesvolle exploitatie resulteert doorgaans in privilege-escalatie, ongeautoriseerde gegevenswijziging of het omzeilen van kritieke business logic-workflows.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Hoog / Gemiddeld* |

> *De ernst wordt Hoog als administratieve attributen, permissieniveaus of factureringsvelden kunnen worden gewijzigd.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### Gebruikt de applicatie automatische binding voor inkomende verzoekparameters?
- [ ] Nee — parameters worden handmatig toegewezen aan specifieke variabelen of veilige objecten  
- [ ] Ja — automatische binding wordt gebruikt, maar strikte **whitelists** of DTO's (Data Transfer Objects) worden afgedwongen  
- [ ] Ja — automatische binding wordt gebruikt en gevoelige interne attributen **kunnen** worden gewijzigd  

### Kunnen administratieve of permissie-gerelateerde attributen succesvol worden geïnjecteerd?
- [ ] Nee — geïnjecteerde parameters worden genegeerd, verwijderd of veroorzaken een validatiefout  
- [ ] Ja — extra parameters worden geaccepteerd, maar hebben **geen** invloed op de status van het backend-object  
- [ ] Ja — het injecteren van parameters zoals `is_admin`, `role`, of `permissions` leidt **succesvol** tot privilege-escalatie *(Kritiek)*  

### Is het mogelijk om gevoelige business logic of financiële velden te wijzigen via overposting?
- [ ] Nee — velden zoals `price`, `balance`, of `status` zijn beschermd tegen mass assignment  
- [ ] Ja — gevoelige zakelijke attributen **kunnen** worden gewijzigd door ze toe te voegen aan de JSON of de body van het formulierverzoek  

### Onthult de applicatie interne objectstructuren die het raden van parameters vergemakkelijken?
- [ ] Nee — antwoorden bevatten alleen de beoogde publieke velden en documentatie is beperkt  
- [ ] Ja — interne objectvelden zijn zichtbaar in GET-responses, waardoor parameter-discovery **mogelijk** is  
- [ ] Ja — API-documentatie (Swagger/OpenAPI) vermeldt expliciet interne eigenschappen die **toegewezen** kunnen worden  

---