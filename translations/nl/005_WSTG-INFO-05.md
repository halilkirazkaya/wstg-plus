## WSTG-INFO-05 — Webpaginacontent controleren op informatielekken

Het controleren van webpaginacontent omvat de handmatige en geautomatiseerde inspectie van applicatierespons, inclusief HTML-, JavaScript- en CSS-bestanden, om gevoelige informatie te identificeren die onbedoeld aan gebruikers wordt blootgesteld. Aanvallers onderzoeken de broncode aan de clientzijde op commentaren van ontwikkelaars, verborgen formuliervelden, interne serverpaden, hardcoded API-sleutels en debugging-informatie die kan helpen bij verdere exploitatiefasen. Dit lekken treedt vaak op wanneer ontwikkelaars vergeten metadata of debugging-code te verwijderen voordat deze naar productie wordt verplaatst, wat mogelijk logische fouten of details over de backend-infrastructuur onthult. Vanuit het perspectief van een aanvaller biedt dit een methode met weinig inspanning om de interne structuur van de applicatie in kaart te brengen en over het hoofd geziene toegangspunten te ontdekken zonder beveiligingswaarschuwingen te activeren of interactie te hebben met de backend-logica van de server.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld* |

> *De ernst wordt Hoog als er gevoelige hardcoded credentials, privésleutels of interne omgevingsvariabelen worden ontdekt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Bevat de broncode commentaren van ontwikkelaars met gevoelige informatie?
- [ ] Nee — geen commentaren of alleen generieke, niet-gevoelige commentaren gevonden  
- [ ] Ja — commentaren bestaan maar bevatten **geen** gevoelige logica of gegevens  
- [ ] Ja — commentaren **onthullen** interne paden, SQL-query's of administratieve instructies  
- [ ] Ja — commentaren **onthullen** credentials of gevoelige aantekeningen van ontwikkelaars *(Hoog)*  

### Bevatten verborgen HTML-invoervelden gevoelige gegevens of statusinformatie?
- [ ] Nee — geen verborgen velden gevonden of ze bevatten alleen niet-gevoelige tokens  
- [ ] Ja — verborgen velden worden gebruikt voor state-beheer maar zijn **versleuteld** of **ondertekend**  
- [ ] Ja — verborgen velden bevatten **plaintext** wachtwoorden, gebruikersrollen of interne ID's  

### Zijn er hardcoded secrets, API-sleutels of interne IP-adressen aanwezig in JavaScript-bestanden?
- [ ] Nee — geen gevoelige strings of secrets gevonden in JS-bestanden  
- [ ] Ja — JS-bestanden bevatten publieke API-sleutels met **juiste** restricties  
- [ ] Ja — JS-bestanden bevatten **onbeveiligde** API-sleutels, interne IP's of private endpoints  
- [ ] Ja — JS-bestanden bevatten **hardcoded** administratieve credentials of privésleutels *(Kritiek)*  

### Onthult de applicatie framework-versies of interne bestandspaden in de broncode?
- [ ] Nee — framework-informatie en bestandspaden zijn **geminificeerd** of **geobfusceerd**  
- [ ] Ja — framework-namen en versies zijn **zichtbaar** maar gepatcht  
- [ ] Ja — interne bestandspaden of absolute serverpaden zijn **blootgesteld** in de broncode  

### Is de broncode gevoelig voor lekken door een gebrek aan minificatie of obfuscatie?
- [ ] Nee — productiecode is **volledig geminificeerd** en ontdaan van commentaren  
- [ ] Ja — code is geminificeerd maar commentaren **blijven aanwezig** in de broncode  
- [ ] Ja — source maps (`.map`-bestanden) zijn **publiekelijk toegankelijk**, wat volledige reconstructie van de broncode mogelijk maakt  

---