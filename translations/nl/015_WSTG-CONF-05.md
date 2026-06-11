## WSTG-CONF-05 — Inventariseren van infrastructuur- en applicatiebeheerinterfaces

Beheerinterfaces (Administrative interfaces) vormen het beheercontrolepaneel van een applicatie en de onderliggende infrastructuur. Deze interfaces verlenen vaak geprivilegieerde toegang tot systeemconfiguraties, gebruikersgegevens en server-side bewerkingen. Aanvallers geven prioriteit aan het inventariseren van deze interfaces via directory Brute Force, analyse van `robots.txt` of door het observeren van voorspelbare URL-patronen om toegangspunten te vinden die mogelijk geen robuuste authenticatie hebben of vertrouwen op standaard inloggegevens (default credentials). De ontdekking van een blootgesteld beheerderspaneel verhoogt het risico op volledige systeemovername, ongeoorloofde gegevenswijziging of serviceonderbreking aanzienlijk. Deze interfaces worden vaak aangetroffen op niet-standaard poorten of subdomeinen, waardoor ze belangrijke doelwitten zijn voor geautomatiseerde scanners en handmatige verkenning (reconnaissance).


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als de interface systeemwijde configuratiewijzigingen, gebruikersbeheer of data-exfiltratie toestaat zonder multifactorauthenticatie (MFA).

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Zijn beheerinterfaces ontdekbaar via directory fuzzing of verkenning (reconnaissance)?
- [ ] Nee — er zijn geen beheerinterfaces blootgesteld of ontdekbaar  
- [ ] Ja — interfaces zijn ontdekbaar, maar de toegang **is beperkt** tot interne IP-bereiken  
- [ ] Ja — interfaces **zijn** ontdekbaar en publiekelijk toegankelijk  

### Wordt authenticatie afgedwongen op de ontdekte beheerportalen?
- [ ] Ja — multifactorauthenticatie (MFA) **wordt afgedwongen**  
- [ ] Ja — single-factor authenticatie **wordt afgedwongen**  
- [ ] Nee — er is geen authenticatie vereist om toegang te krijgen tot de interface *(Kritiek)*  

### Zijn beheerderspaden verborgen of niet-standaard om ontdekking te voorkomen?
- [ ] Ja — paden maken gebruik van niet-standaard, willekeurige of niet-voorspelbare naamgeving  
- [ ] Nee — paden maken gebruik van gangbare naamgevingsconventies (bijv. `/admin`, `/manager`, `/console`) en **kunnen** gemakkelijk worden geraden  

### Biedt de interface toegang tot gevoelige systeem- of applicatiefunctionaliteit?
- [ ] Nee — de interface is een "read-only" statusdashboard met minimale impact  
- [ ] Ja — de interface **kan** applicatieconfiguraties of gebruikersgegevens wijzigen  
- [ ] Ja — de interface **kan** commando's op systeemniveau uitvoeren of databasebeheer uitvoeren  

---