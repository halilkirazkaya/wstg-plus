## WSTG-ATHN-02 — Testen op standaard inloggegevens (Default Credentials)

Het testen op standaard inloggegevens (Default Credentials) omvat het identificeren van administratieve interfaces, diensten of applicatiecomponenten die nog steeds gebruikmaken van door de fabriek ingestelde of door de leverancier verstrekte gebruikersnamen en wachtwoorden. Deze kwetsbaarheid is een doelwit met een hoge prioriteit voor aanvallers, omdat het vaak een directe weg biedt naar een volledige systeemcompromis, administratieve toegang of Remote Code Execution (RCE) met minimale inspanning. Dit komt doorgaans voor in kant-en-klare software (off-the-shelf), CMS-platforms, databasebeheertools en geïntegreerde hardware zoals IoT-apparaten of netwerkapparatuur. Exploitatie wordt meestal uitgevoerd door geïdentificeerde softwareversies te vergelijken met openbare databases voor standaardwachtwoorden of door gebruik te maken van geautomatiseerde `Credential Stuffing` tools tijdens de initiële verkennings- en exploitatiefasen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst / Severity** | Kritiek / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Zijn administratieve of beheerinterfaces blootgesteld aan het internet of aan onbetrouwbare netwerksegmenten?
- [ ] Nee — er zijn geen beheerinterfaces toegankelijk  
- [ ] Ja — interfaces zijn aanwezig, maar beperkt door controles op netwerkniveau (bijv. VPN, IP Allowlist)  
- [ ] Ja — interfaces **zijn** publiek toegankelijk en vereisen authenticatie  

### Zijn de door de leverancier geleverde standaard inloggegevens gewijzigd voor alle geïdentificeerde componenten?
- [ ] Ja — alle standaard inloggegevens **zijn gewijzigd** voor alle componenten *(Meest veilig)*  
- [ ] Ja — inloggegevens **zijn gewijzigd** voor de hoofdapplicatie, maar secundaire componenten (bijv. CMS, DB) maken nog gebruik van de standaardinstellingen  
- [ ] Nee — standaard inloggegevens **zijn actief** op een of meer identificeerbare interfaces *(Kritiek)*  

### Dwingt de applicatie een wachtwoordwijziging af bij de eerste login voor nieuwe of administratieve accounts?
- [ ] Ja — een wachtwoordwijziging **is verplicht** voordat er administratieve acties kunnen worden ondernomen  
- [ ] Nee — de applicatie **staat** het gebruik van standaard inloggegevens voor onbepaalde tijd **toe**  

### Zijn lockout-mechanismen of Rate Limiting-controles actief om geautomatiseerd testen van standaard inloggegevens te voorkomen?
- [ ] Ja — agressieve Rate Limiting of account lockout **wordt toegepast**  
- [ ] Ja — controles zijn aanwezig, maar een bypass **is mogelijk** via header-manipulatie of IP-rotatie  
- [ ] Nee — er zijn geen Rate Limiting- of lockout-mechanismen **ingeschakeld**  

### Maken plug-ins van derden, middleware of administratieve tools (bijv. phpMyAdmin, Tomcat Manager) gebruik van unieke inloggegevens?
- [ ] Nee — er zijn geen componenten van derden aanwezig in de omgeving  
- [ ] Ja — alle componenten van derden gebruiken unieke, niet-standaard inloggegevens  
- [ ] Nee — ten minste één component van derden **is toegankelijk** via bekende standaard inloggegevens  

---