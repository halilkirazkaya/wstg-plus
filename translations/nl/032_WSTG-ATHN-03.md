## WSTG-ATHN-03 — Testen op zwakke accountvergrendelingsmechanismen

Een accountvergrendelingsmechanisme is een cruciaal defense-in-depth controlemechanisme, ontworpen om Brute Force- en Credential Stuffing-aanvallen te beperken door een account uit te schakelen na een bepaald aantal mislukte authenticatiepogingen. Zonder een robuust vergrendelingsbeleid kunnen aanvallers geautomatiseerde tools gebruiken om systematisch wachtwoorden te raden voor meerdere accounts (Password Spraying) of een specifiek account met een hoge waarde te targeten tot de poging slaagt. Deze kwetsbaarheid manifesteert zich doorgaans op inlogportalen, formulieren voor het opnieuw instellen van wachtwoorden en API-endpoints die geen Rate Limiting of beveiliging op accountniveau hebben. Vanuit het perspectief van een aanvaller maakt een zwak of ontbrekend vergrendelingsmechanisme een oneindig aantal wachtwoordpogingen mogelijk, terwijl een te agressief mechanisme kan worden misbruikt om een distributed Denial of Service (DoS) tegen legitieme gebruikers te verooraken door hun accounts opzettelijk te vergrendelen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt "Hoog" als de applicatie gevoelige PII, financiële gegevens verwerkt, of als beheerdersaccounts vatbaar zijn voor Brute Force zonder secundaire controles.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Wordt er een accountvergrendelingsmechanisme afgedwongen na een vastgesteld aantal mislukte pogingen?
- [ ] Ja — account wordt vergrendeld na een gedefinieerde, redelijke drempelwaarde (bijv. 5-10 pogingen)  
- [ ] Ja — account wordt vergrendeld, maar pas na een buitengewoon hoog aantal pogingen  
- [ ] Nee — account **kan niet** worden vergrendeld en staat onbeperkte Brute Force toe  

### Kan het vergrendelingsmechanisme worden omzeild met gangbare ontwijkingstechnieken?
- [ ] Nee — vergrendeling wordt aan de serverzijde afgedwongen en wordt **niet** beïnvloed door wijzigingen aan de clientzijde  
- [ ] Ja — vergrendeling **kan** worden omzeild door IP-adressen te roteren of een proxy pool te gebruiken  
- [ ] Ja — vergrendeling **kan** worden omzeild door HTTP-headers zoals `X-Forwarded-For` of `User-Agent` te manipuleren  
- [ ] Ja — vergrendeling **kan** worden omzeild door cookies te verwijderen of nieuwe sessies te starten  

### Biedt het vergrendelingsmechanisme een mogelijkheid voor accountherstel of verloop?
- [ ] Ja — account wordt automatisch ontgrendeld na een ingestelde tijdsduur of via e-mailverificatie  
- [ ] Ja — account vereist tussenkomst van een beheerder om te ontgrendelen  
- [ ] Nee — vergrendeling is permanent zonder duidelijk herstelpad, wat het DoS-risico verhoogt  

### Maakt de applicatie tijdens de vergrendeling onderscheid tussen een geldige en ongeldige gebruikersnaam?
- [ ] Nee — de respons van de applicatie is identiek voor zowel bestaande als niet-bestaande gebruikers  
- [ ] Ja — de applicatie onthult of een account is vergrendeld, wat **Username Enumeration** mogelijk maakt  

### Is het vergrendelingsmechanisme vatbaar voor een Denial of Service (DoS)-aanval?
- [ ] Nee — controles zoals CAPTCHA of IP-gebaseerde Rate Limiting voorkomen massale accountvergrendeling  
- [ ] Ja — een aanvaller **kan** systematisch alle bekende gebruikers vergrendelen door onjuiste wachtwoorden op te geven  

---