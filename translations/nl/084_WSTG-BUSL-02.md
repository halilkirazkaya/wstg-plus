## WSTG-BUSL-02 — Testen van de mogelijkheid om requests te vervalsen

Het vervalsen van requests binnen een business logic context omvat het manipuleren van door de applicatie gedefinieerde parameters om acties uit te voeren buiten de beoogde workflow of het autorisatieniveau. Aanvallers richten zich op processen die uit meerdere stappen bestaan, zoals checkout-systemen of accountregistratie, waarbij de status (state) wordt bijgehouden via client-side parameters die de server niet opnieuw valideert tegen een vertrouwde backend-bron. Door verborgen velden, JSON-waarden of REST-parameters zoals prijzen, hoeveelheden of interne statusvlaggen te wijzigen, kan een aanvaller betalingsvereisten omzeilen, privileges escaleren, of onbedoelde statuswijzigingen veroorzaken. Deze kwetsbaarheid komt doorgaans voor in stateful webformulieren of API's waarbij de server impliciet de representatie van de business state door de client vertrouwt tijdens een transactie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek* |

> *De ernst wordt Kritiek als het vervalste request leidt tot financieel verlies, ongeautoriseerde administratieve acties of volledige accountovername.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Valideert de applicatie transactionele parameters tegen een server-side "source of truth"?
- [ ] Ja — alle gevoelige parameters (prijs, rol, ID) worden gevalideerd tegen de database en een bypass is **niet mogelijk**  
- [ ] Ja — validatie wordt toegepast, maar kan worden omzeild via parameter pollution of alternatieve encoding  
- [ ] Nee — de applicatie vertrouwt op waarden aan de client-zijde om de kosten of de uitkomst van een transactie te bepalen *(Kritiek)*  

### Kunnen bedrijfskritische waarden (bijv. hoeveelheden, bedragen) worden aangepast naar ongeautoriseerde waarden?
- [ ] Nee — negatieve waarden, nulwaarden of buitengewoon hoge waarden worden door de server geweigerd  
- [ ] Ja — de server accepteert gewijzigde waarden, maar alleen binnen een beperkt bereik  
- [ ] Ja — negatieve of nulwaarden **kunnen** worden verzonden, wat leidt tot financiële bypasses of omzeiling van de logica  

### Worden verborgen velden of API-parameters gebruikt om de status (state) te behouden over processen met meerdere stappen?
- [ ] Nee — de status wordt veilig beheerd via server-side sessies of ondertekende tokens  
- [ ] Ja — de status wordt via de client doorgegeven, maar integriteitscontroles (HMAC/Signatures) zijn **ingeschakeld** en robuust  
- [ ] Ja — de status wordt via de client doorgegeven en integriteitscontroles zijn **uitgeschakeld** of kunnen worden verwijderd  

### Is het mogelijk om requests te vervalsen die verplichte tussenstappen in een workflow overslaan?
- [ ] Nee — de server dwingt een strikte volgorde van bewerkingen af voor elke transactie  
- [ ] Ja — stappen **kunnen** worden overgeslagen, maar de uiteindelijke actie vereist nog steeds geldige gegevens uit eerdere stappen  
- [ ] Ja — het uiteindelijke "submit" of "complete" request **kan** direct worden vervalst om autorisatie- of betalingsstappen te omzeilen  

---