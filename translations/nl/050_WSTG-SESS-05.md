## WSTG-SESS-05 — Testen op Cross-Site Request Forgery

Cross-Site Request Forgery (CSRF) is een kwetsbaarheid waarbij een aanvaller de browser van een slachtoffer misleidt om een ongewenste actie uit te voeren op een andere website waar het slachtoffer momenteel is geauthenticeerd. Deze exploit maakt gebruik van het gedrag van de browser om automatisch "ambient" inloggegevens, zoals sessiecookies of Authorization-headers, toe te voegen aan uitgaande verzoeken. Aanvallers richten zich doorgaans op statusveranderende operaties zoals het wijzigen van wachtwoorden, het bijwerken van e-mailadressen of het uitvoeren van financiële overschrijvingen door kwaadaardige scripts of verborgen formulieren op een externe site te hosten. Succesvolle exploitatie kan leiden tot volledige accountovername of ongeautoriseerde gegevenswijziging zonder medeweten of toestemming van de gebruiker.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt "Hoog" als de kwetsbare actie accountovername, privilege-escalatie of ongeautoriseerde financiële transacties mogelijk maakt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Tools:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Zijn anti-CSRF tokens geïmplementeerd voor statusveranderende verzoeken?
- [ ] Ja — unieke, cryptografisch sterke tokens zijn vereist voor alle statusveranderende acties  
- [ ] Ja — tokens zijn aanwezig, maar zijn **niet** uniek per sessie of zijn voorspelbaar  
- [ ] Nee — anti-CSRF tokens zijn **niet** geïmplementeerd  

### Is de server-side validatie van het anti-CSRF token robuust?
- [ ] Ja — de server valideert het token strikt en bypass is **niet mogelijk**  
- [ ] Ja — validatie wordt uitgevoerd, maar **kan** worden omzeild door de tokenparameter te verwijderen  
- [ ] Ja — validatie wordt uitgevoerd, maar **kan** worden omzeild door een leeg of dummy token op te geven  
- [ ] Ja — validatie wordt uitgevoerd, maar het token is **niet** gekoppeld aan de sessie van de gebruiker  

### Vertrouwt de applicatie op eenvoudig te omzeilen methoden voor CSRF-bescherming?
- [ ] Nee — de applicatie vertrouwt niet alleen op zwakke headers of origin-controles  
- [ ] Ja — de bescherming vertrouwt uitsluitend op de `Referer`- of `Origin`-header, die **gespooft** of verwijderd kan worden  
- [ ] Ja — de bescherming vertrouwt op het controleren van de `X-Requested-With`-header, wat **omzeild kan worden** via CORS-misconfiguraties  

### Zijn sessiecookies geconfigureerd met het `SameSite`-attribuut?
- [ ] Ja — `SameSite` is ingesteld op `Strict` of `Lax` voor alle sessiegerelateerde cookies  
- [ ] Nee — het `SameSite`-attribuut **ontbreekt**, waardoor wordt teruggevallen op browserspecifiek gedrag  
- [ ] Nee — `SameSite` is expliciet ingesteld op `None` zonder de `Secure`-vlag of aanvullende mitigerende maatregelen  

### Kan de CSRF-bescherming worden omzeild door de HTTP-verzoekmethode te wijzigen?
- [ ] Nee — bescherming wordt afgedwongen ongeacht de gebruikte HTTP-methode  
- [ ] Ja — tokens worden alleen gevalideerd bij `POST`-verzoeken, maar de actie **kan** worden uitgevoerd via `GET`  
- [ ] Ja — het wijzigen van de methode (bijv. naar `PUT` of `DELETE`) omzeilt de tokencontrole  

---