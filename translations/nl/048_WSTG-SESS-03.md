## WSTG-SESS-03 — Session Fixation

Session fixation treedt op wanneer een applicatie nalaat de sessie-identificator te invalideren of te roteren nadat een gebruiker succesvol is geauthenticeerd, waardoor een aanvaller een bekend sessietoken kan afdwingen bij een slachtoffer. Als de applicatie de pre-authenticatie sessie-ID behoudt na de login, kan een aanvaller een slachtoffer voorzien van een specifiek samengestelde link die een vast sessie-ID bevat en vervolgens de geauthenticeerde sessie overnemen (hijacken). Deze kwetsbaarheid manifesteert zich doorgaans in inlogformulieren of via sessie-identificatoren die worden geaccepteerd via URL-parameters en cookies. Vanuit het perspectief van de aanvaller omvat exploitatie het "fixeren" van de sessie via een kwaadaardige link of een header injection kwetsbaarheid en wachten tot het slachtoffer inloggegevens verstrekt, waardoor de noodzaak om een live token te stelen effectief wordt omzeild.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Verandert de sessie-identificator na een succesvolle authenticatie?
- [ ] Ja — er wordt een nieuwe sessie-identificator **uitgegeven** en de oude wordt geïnvalideerd *(Meest veilig)*  
- [ ] Ja — er wordt een nieuwe sessie-identificator **uitgegeven** maar de oude **blijft geldig**  
- [ ] Nee — de sessie-identificator **blijft hetzelfde** voor en na authenticatie *(Kritiek)*  

### Accepteert de applicatie sessie-identificatoren die via URL-parameters worden aangeboden?
- [ ] Nee — sessie-ID's worden **uitsluitend** via cookies beheerd  
- [ ] Ja — sessie-ID's worden geaccepteerd in de URL maar worden **genegeerd** of **geroteerd** bij het inloggen  
- [ ] Ja — sessie-ID's in de URL worden **geaccepteerd** en **behouden** na authenticatie  

### Kan een door de aanvaller gedefinieerde sessie-ID worden afgedwongen bij de applicatie?
- [ ] Nee — de applicatie **weigert** ongeldige of niet-bestaande sessie-ID's die door de gebruiker worden verstrekt  
- [ ] Ja — de applicatie **accepteert** en initialiseert een sessie met een willekeurige ID die in een cookie wordt verstrekt  
- [ ] Ja — de applicatie **accepteert** en initialiseert een sessie met een willekeurige ID die in een URL-parameter wordt verstrekt  

### Worden pre-authenticatie sessies correct beëindigd?
- [ ] Ja — de anonieme sessie wordt **volledig vernietigd** bij de login-gebeurtenis  
- [ ] Nee — de anonieme sessie wordt **niet vernietigd**, wat mogelijke sessie-harvesting toestaat  

### Is de sessie-cookie beschermd tegen client-side injection?
- [ ] Ja — `HttpOnly` and `Secure` flags zijn **toegepast** om op scripts gebaseerde fixation te voorkomen  
- [ ] Nee — flags zijn **niet toegepast**, waardoor sessie-ID's kunnen worden ingesteld via Cross-Site Scripting (XSS)  

---