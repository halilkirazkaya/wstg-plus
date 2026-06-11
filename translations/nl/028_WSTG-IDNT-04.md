## WSTG-IDNT-04 — Testen op Account Enumeration en Raadbare Gebruikersaccounts

Account Enumeration treedt op wanneer een applicatie het bestaan of niet-bestaan van een gebruikersaccount onthult via variaties in HTTP-responses, foutmeldingen of meetbare tijdsverschillen. Aanvallers misbruiken dit gedrag door systematisch lijsten met gebruikersnamen in te dienen om geldige accounts te identificeren, die vervolgens het doelwit worden van Credential Stuffing, Brute Force-aanvallen of Social Engineering. Deze kwetsbaarheid komt vaak voor in inlogportalen, functies voor het herstellen van wachtwoorden, registratieformulieren en API-endpoints die gebruikersspecifieke identifiers verwerken. Vanuit het perspectief van een aanvaller verkleint succesvolle enumeratie het aanvalsoppervlak aanzienlijk door een blinde Brute Force-poging te transformeren in een gerichte exploitatie van bekende, geldige identiteiten.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Zijn de foutmeldingen bij authenticatie consistent in alle scenario's?
- [ ] Ja — er worden generieke foutmeldingen gebruikt voor zowel ongeldige gebruikersnamen als ongeldige wachtwoorden  
- [ ] Ja — de meldingen zijn vergelijkbaar, maar er **zijn** lichte variaties in interpunctie of hoofdlettergebruik aanwezig  
- [ ] Nee — foutmeldingen vermelden expliciet "Gebruiker niet gevonden" of "Onjuist wachtwoord" *(Kwetsbaar)*  

### Lekt de flow voor het herstellen van wachtwoorden of "wachtwoord vergeten" het bestaan van accounts?
- [ ] Nee — de applicatie retourneert een generieke melding, ongeacht of het e-mailadres of de gebruikersnaam bestaat  
- [ ] Ja — er zijn controles aanwezig, maar een bypass **is mogelijk** via de lengte van de response of statuscodes  
- [ ] Ja — de applicatie bevestigt expliciet of er een herstel-e-mail is verzonden of dat het account **niet bestaat**  

### Is het registratieproces beschermd tegen Account Enumeration?
- [ ] Nee — de registratie lekt geen informatie of gebruikt CAPTCHA/Rate Limiting om bulkcontroles te voorkomen  
- [ ] Ja — er zijn controles aanwezig, maar een bypass **is mogelijk** via tijdsverschillen of verschillen in API-responses  
- [ ] Ja — de applicatie stelt de gebruiker onmiddellijk op de hoogte als de gebruikersnaam of het e-mailadres **al in gebruik is**  

### Zijn er meetbare tijdsverschillen bij het vergelijken van geldige versus ongeldige gebruikersnamen?
- [ ] Nee — responstijden zijn consistent ongeacht de geldigheid van het account dankzij constant-time vergelijkingen  
- [ ] Ja — er zijn tijdsverschillen aanwezig, maar deze zijn te klein om betrouwbaar over het netwerk te worden gemeten  
- [ ] Ja — er **zijn** meetbare tijdsverschillen aanwezig (bijv. doordat password hashing alleen wordt uitgevoerd voor geldige gebruikers)  

### Maakt de applicatie gebruik van voorspelbare of raadbare naamgevingsconventies voor accounts?
- [ ] Nee — account-identifiers zijn willekeurig (UUIDs) of door de gebruiker gedefinieerd en complex  
- [ ] Ja — account-identifiers volgen een patroon (bijv. `emp001`, `emp002`) en enumeratie **is mogelijk**  
- [ ] Ja — identifiers zijn opeenvolgende gehele getallen, waardoor volledige database-enumeratie **mogelijk is**  

---