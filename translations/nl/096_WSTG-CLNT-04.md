## WSTG-CLNT-04 — Testen op Client-Side URL Redirect

Client-side URL redirection treedt op wanneer een webapplicatie door de gebruiker gecontroleerde invoer accepteert—meestal via URL-parameters of hash-fragmenten—en deze gebruikt binnen een client-side redirection sink zoals `window.location` of `document.location.href`. Deze kwetsbaarheid wordt voornamelijk door aanvallers misbruikt om geavanceerde phishingcampagnes uit te voeren, aangezien het slachtoffer in eerste instantie het legitieme domein vertrouwt voordat deze ongemerkt wordt doorgeleid naar een kwaadaardige site. Naast phishing kunnen client-side redirects vaak worden gekoppeld (chained) om gevoelige gegevens te exfiltreren, zoals OAuth access tokens of sessie-identifiers, indien de omleiding plaatsvindt terwijl deze waarden aanwezig zijn in de URL of de `Referer` header. In moderne Single Page Applications (SPA's) komt deze kwetsbaarheid vaak voor in de routing-logica, "return to"-parameters na authenticatie of functionaliteiten voor het wisselen van taal.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Medium* |

> *De ernst (Severity) wordt Hoog als de redirect kan worden gebruikt om OAuth tokens of sessie-ID's te exfiltreren, of om beveiligingscontroles in een authenticatieflow te omzeilen.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Maakt de applicatie gebruik van client-side scripts om redirects te verwerken op basis van gebruikersinvoer?
- [ ] Nee — redirection wordt uitsluitend aan de serverzijde afgehandeld via `3xx` HTTP-statuscodes  
- [ ] Ja — de applicatie gebruikt JavaScript sinks zoals `window.location` om te navigeren op basis van URL-parameters  
- [ ] Ja — de applicatie maakt gebruik van een client-side framework-router (bijv. React Router, Vue Router) die door de gebruiker opgegeven paden verwerkt  

### Zijn er validatie- of whitelisting-controles geïmplementeerd voor de bestemmings-URL?
- [ ] Ja — een strikte **whitelist** van toegestane domeinen/paden wordt afgedwongen en een bypass is **niet mogelijk**  
- [ ] Ja — validatie is aanwezig maar vertrouwt op zwakke regex of blacklists, en een bypass is **mogelijk**  
- [ ] Nee — de applicatie accepteert elke willekeurige string als redirection-doel  

### Kan de redirection-logica worden omzeild met gangbare obfuscatietechnieken?
- [ ] Nee — controles verwerken correct gecodeerde tekens, protocol-relatieve URL's (`//`) en backslashes  
- [ ] Ja — bypass is **mogelijk** via URL-encoding of double-encoding  
- [ ] Ja — bypass is **mogelijk** via protocol-relatieve URL's of `@`-tekens (bijv. `https://expected.com@attacker.com`)  
- [ ] Ja — bypass is **mogelijk** via specifieke browser quirks of null-byte-injectie  

### Stelt het redirection-proces gevoelige gegevens bloot aan de bestemmingssite?
- [ ] Nee — er zijn geen gevoelige gegevens aanwezig in de URL of verzonden via headers tijdens de redirect  
- [ ] Ja — gevoelige gegevens (bijv. sessietokens, API-sleutels) **worden gelekt** naar de externe site via de `Referer` header  
- [ ] Ja — gevoelige gegevens in het URL-fragment (`#`) zijn **toegankelijk** voor de scripts van de bestemmingssite  

### Is het mogelijk om JavaScript uit te voeren via de redirect-sink (DOM-based XSS)?
- [ ] Nee — de sink staat alleen navigatie naar `http`- of `https`-protocollen toe  
- [ ] Ja — de redirect-sink staat `javascript:` of `data:` URI's toe, waardoor DOM-based XSS **mogelijk** is  

---