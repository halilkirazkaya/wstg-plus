## WSTG-BUSL-10 — Testen van de Betalingsfunctionaliteit

Het testen van de betalingsfunctionaliteit omvat het identificeren van Business Logic fouten die een aanvaler in staat stellen om payment gateways te omzeilen, ordertotalen te manipuleren of succesvolle transactiesignalen te spoofen. Deze kwetsbaarheden bevinden zich doorgaans in de communicatie tussen de applicatie, de client-browser en externe payment processors zoals Stripe, PayPal of interne grootboeksystemen. Een aanvaler kan proberen om prijsinstellingen tijdens het transport te wijzigen, negatieve hoeveelheden in te dienen om de totale kosten te verlagen, of succesvolle transactie-callbacks te replayen om bestellingen te voltooien zonder daadwerkelijke betaling. Succesvolle exploitatie leidt tot direct financieel verlies, voorraadverschillen en een mogelijke aantasting van de e-commerce-integriteit van de applicatie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Wordt het transactiebedrag aan de serverzijde gevalideerd voorafgaand aan de verwerking?
- [ ] Ja — bedrag wordt strikt gevalideerd tegen de productdatabase aan de serverzijde *(Meest veilig)*  
- [ ] Ja — bedrag wordt gevalideerd, maar een logic bypass **is mogelijk** via parameter pollution of het wisselen van valuta  
- [ ] Nee — het transactiebedrag **kan** worden gewijzigd in het request aan de clientzijde en wordt geaccepteerd door de backend  

### Kan het aantal artikelen worden gemanipuleerd, resulterend in een totaal van nul of een negatief bedrag?
- [ ] Nee — negatieve of nul-hoeveelheden worden afgewezen door de validatielogica van de applicatie  
- [ ] Ja — negatieve hoeveelheden worden geaccepteerd in de winkelwagen, maar het totaal wordt gecorrigeerd bij het afrekenen  
- [ ] Ja — negatieve hoeveelheden **kunnen** worden gebruikt om het totale orderbedrag te verlagen of een tegoed te verkrijgen *(Kritiek)*  

### Verwerkt de applicatie payment gateway callbacks of webhooks op een veilige manier?
- [ ] Ja — callbacks vereisen een geldige cryptografische handtekening die **niet** gespoofd kan worden  
- [ ] Ja — handtekeningen worden gecontroleerd, maar het geheim is zwak of de verificatie **kan** worden omzeild  
- [ ] Nee — de applicatie vertrouwt op niet-geauthenticeerde of niet-geverifieerde callbacks om de betalingsstatus te bevestigen  

### Is de applicatie kwetsbaar voor race conditions tijdens de afreken- of betalingsfase?
- [ ] Nee — transactionele integriteit en database-locking-mechanismen zijn **ingeschakeld**  
- [ ] Ja — race conditions **zijn mogelijk**, maar hebben geen invloed op de uiteindelijke prijs of levering  
- [ ] Ja — gelijktijdige requests **kunnen** leiden tot meerdere leveringen voor een enkele betaling of het "double-spending" van cadeaubonnen  

### Zijn kortingscodes of loyaliteitspunten onderhevig aan manipulatie of hergebruik?
- [ ] Nee — codes worden gevalideerd voor eenmalig gebruik en logische beperkingen worden **toegepast**  
- [ ] Ja — codes kunnen worden hergebruikt of toegepast op niet-in aanmerking komende artikelen, maar de impact is beperkt  
- [ ] Ja — aanvallers **kunnen** meerdere conflicterende codes toepassen of de vervaldata en gebruiksbeperkingen omzeilen  

---