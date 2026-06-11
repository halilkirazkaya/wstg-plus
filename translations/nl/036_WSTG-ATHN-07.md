## WSTG-ATHN-07 — Testen op Zwak Wachtwoordbeleid

Het testen op een zwak wachtwoordbeleid omvat het evalueren van de vereisten voor complexiteit, lengte en entropie die door een applicatie worden afgedwongen tijdens het aanmaken van accounts en het bijwerken van inloggegevens. Onvoldoende beleid vormt een directe bedreiging voor de vertrouwelijkheid van gebruikersaccounts, omdat deze hierdoor vatbaar worden voor geautomatiseerde Dictionary-aanvallen, Brute Force-pogingen en Credential Stuffing. Deze kwetsbaarheden bevinden zich doorgaans in registratie-endpoints, wachtwoordreset-workflows en administratieve beheerderspanelen. Vanuit het perspectief van een aanvaller verlaagt een laks beleid aanzienlijk de computationele kosten en de tijd die nodig is om inloggegevens succesvol te compromitteren met behulp van gangbare wordlists of patroongebaseerd kraken.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Gemiddeld / Laag* |

> *De ernst wordt Hoog als de applicatie geen mechanismen voor account lockout of Rate Limiting heeft om geautomatiseerd raden te voorkomen.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Tools:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Wordt een minimale wachtwoordlengte afgedwongen door de applicatie?
- [ ] Ja — minimale lengte is 12+ tekens *(Meest veilig)*  
- [ ] Ja — minimale lengte ligt tussen 8 en 11 tekens  
- [ ] Ja — minimale lengte is **minder dan** 8 tekens  
- [ ] Nee — er wordt geen minimale lengte afgedwongen  

### Worden complexiteitsvereisten (hoofdletters, kleine letters, cijfers, symbolen) afgedwongen?
- [ ] Ja — meerdere tekenklassen zijn verplicht en een bypass is **niet mogelijk**  
- [ ] Ja — tekenklassen worden gesuggereerd maar **niet** afgedwongen  
- [ ] Nee — er worden geen complexiteitsvereisten toegepast  

### Blokkeert de applicatie veelvoorkomende/zwakke wachtwoorden of wachtwoorden die gebruikersspecifieke gegevens bevatten?
- [ ] Ja — bekende zwakke wachtwoorden (bijv. "password123") en op de gebruikersnaam gebaseerde wachtwoorden **kunnen niet** worden gebruikt  
- [ ] Ja — veelvoorkomende wachtwoorden worden geblokkeerd, maar gebruikersspecifieke gegevens (bijv. gebruikersnaam, e-mail) **kunnen** worden gebruikt  
- [ ] Nee — elke string die voldoet aan de lengte-/complexiteitsvereisten wordt geaccepteerd  

### Kunnen complexiteits- of lengtevereisten worden omzeild via client-side aanpassingen?
- [ ] Nee — beleid wordt strikt afgedwongen aan de **server-side**  
- [ ] Ja — beleid wordt afgedwongen via JavaScript en **kan** worden omzeild door het verzoek te onderscheppen  

### Wordt het wachtwoordbeleid duidelijk gecommuniceerd naar de gebruiker zonder implementatiedetails te lekken?
- [ ] Ja — beleid is zichtbaar tijdens de invoer en geeft generieke foutmeldingen  
- [ ] Ja — beleid is zichtbaar, maar foutmeldingen onthullen specifieke regex- of logicagaten  
- [ ] Nee — beleid is **niet** zichtbaar, wat dwingt tot ontdekking via trial-and-error  

---