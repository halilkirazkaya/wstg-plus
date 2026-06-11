## WSTG-BUSL-05 — Testen van limieten op het aantal keren dat een functie kan worden gebruikt

Deze test evalueert of een applicatie business logic restricties afdwingt op de frequentie en het totale aantal keren dat een specifieke actie of functie kan worden uitgevoerd door een enkele gebruiker, sessie of IP-adres. Het niet implementeren van deze limieten stelt aanvallers in staat om waardevolle functies te misbruiken—zoals het verzenden van SMS OTPs, het triggeren van e-mails voor het herstellen van wachtwoorden, het toepassen van kortingscodes of het uitvoeren van zware data-exports—wat kan leiden tot financieel verlies, uitputting van resources of reputatieschade. Deze kwetsbaarheden worden doorgaans gevonden in transactionele endpoints of communicatiefuncties en worden geëxploiteerd via geautomatiseerde scripts die de actie herhalen ver buiten de beoogde zakelijke drempels. Vanuit het perspectief van een aanvaller is dit een belangrijk doelwit voor SMS pumping, mail bombing of het brute-forcen van workflows die geen expliciete rate-limiting of op aantallen gebaseerde throttles hebben.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als de functie financiële transacties of SMS-kosten betreft, of de systeembrede beschikbaarheid beïnvloedt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### Definieert en handhaaft de applicatie limieten op gevoelige bedrijfsfuncties?
- [ ] Ja — limieten worden strikt gehandhaafd en bypass is **niet mogelijk**  
- [ ] Ja — limieten worden gehandhaafd maar zijn te hoog om misbruik te voorkomen  
- [ ] Nee — er worden geen limieten toegepast op de uitvoering van de functie  

### Worden rate limits en gebruikstellers server-side afgedwongen?
- [ ] Ja — handhaving is strikt server-side en **kan niet** worden gemanipuleerd  
- [ ] Nee — handhaving vertrouwt op client-side logica (bijv. JavaScript of verborgen velden)  
- [ ] Nee — er bestaat geen handhaving op welk niveau dan ook  

### Kunnen de gebruikslimieten worden omzeild via header- of sessiemanipulatie?
- [ ] Nee — bypass via `X-Forwarded-For`, `Origin`, of sessierotatie is **niet mogelijk**  
- [ ] Ja — limieten **kunnen** worden omzeild door het spoofen van IP-headers of het wissen van cookies  
- [ ] Ja — limieten **kunnen** worden omzeild door het aanmaken van meerdere accounts of sessies  

### Triggert het overschrijden van de limiet een passende defensieve reactie?
- [ ] Ja — applicatie retourneert `429 Too Many Requests` en logt de gebeurtenis *(Meest veilig)*  
- [ ] Ja — applicatie retourneert een foutmelding maar logt het potentiële misbruik **niet**  
- [ ] Nee — applicatie blijft verzoeken verwerken maar negeert de output  
- [ ] Nee — applicatie geeft geen reactie of crasht onder belasting  

### Is de impact van het misbruiken van de functie aanzienlijk voor de organisatie?
- [ ] Nee — misbruik van de functie heeft een verwaarloosbare kosten- of resource-impact  
- [ ] Ja — misbruik **kan** leiden tot gering resourceverbruik of irritatie bij gebruikers  
- [ ] Ja — misbruik **kan** leiden tot aanzienlijke financiële kosten (bijv. SMS-kosten) of denial of service *(Kritiek)*  

---