## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Stored Cross Site Scripting (XSS), ook bekend als Persistent XSS, vindt plaats wanneer een applicatie gegevens van een gebruiker ontvangt en deze opslaat in een persistente database, bestandssysteem of ander opslagmedium zonder adequate validatie of encoding. Wanneer een slachtoffer vervolgens naar een pagina navigeert die deze niet-gesaneerde gegevens ophaalt en weergeeft, wordt het kwaadaardige script uitgevoerd binnen hun browsercontext onder de origin van de applicatie. Deze kwetsbaarheid is bijzonder gevaarlijk omdat het niet vereist dat een slachtoffer op een speciaal samengestelde link klikt; elke gebruiker die de betreffende pagina bekijkt, wordt een doelwit, wat kan leiden tot session hijacking, ongeautoriseerde acties of credential harvesting. Aanvallers richten zich doorgaans op commentaarsecties, gebruikersprofielen, message boards en administratieve logs waar invoer consistent wordt gerenderd voor andere gebruikers of beheerders.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Tools:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Zijn er invoerpunten die gebruikersinvoer opslaan voor latere weergave aan andere gebruikers?
- [ ] Nee — de applicatie slaat geen door de gebruiker controleerbare invoer op voor latere weergave  
- [ ] Ja — invoer wordt opgeslagen (bijv. profielen, reacties) maar wordt **niet** gerenderd in een HTML-context  
- [ ] Ja — invoer wordt opgeslagen en gerenderd voor andere gebruikers of beheerders  

### Wordt output encoding of sanitization toegepast wanneer opgeslagen gegevens worden gerenderd?
- [ ] Ja — context-aware output encoding **is toegepast** en bypass is **niet mogelijk** *(Meest veilig)*  
- [ ] Ja — sanitization (bijv. DOMPurify) **is toegepast** maar bypass **is mogelijk** via configuratiefouten  
- [ ] Nee — gegevens worden direct in de DOM gerenderd **zonder** encoding of sanitization *(Hoog)*  

### Kan de inputvalidatie of sanitization worden omzeild met alternatieve payloads?
- [ ] Nee — filters verwerken verschillende encodings, niet-standaard tags en event handlers op veilige wijze  
- [ ] Ja — bypass **is mogelijk** met behulp van HTML-entity encoding of specifieke tags (bijv. `<svg>`, `<img>`)  
- [ ] Ja — bypass **is mogelijk** via polyglot payloads of mutation-based XSS (mXSS)  

### Biedt een Content Security Policy (CSP) een secundaire verdedigingslaag?
- [ ] Ja — een strikt CSP is **ingeschakeld** en voorkomt de uitvoering van inline scripts en ongeautoriseerde bronnen  
- [ ] Ja — een CSP is **ingeschakeld** maar is verkeerd geconfigureerd (bijv. `unsafe-inline` of een brede `script-src`)  
- [ ] Nee — er is geen CSP **ingeschakeld** voor de applicatie  

### Is het mogelijk om "Stored XSS naar RCE" of andere high-impact ketens (Blind XSS) te bereiken?
- [ ] Nee — impact is beperkt tot de browsercontext aan de clientzijde  
- [ ] Ja — stored XSS **kan** worden gebruikt om beheerders te targeten (Blind XSS) in interne panelen  
- [ ] Ja — stored XSS **kan** worden gecombineerd met andere kwetsbaarheden (bijv. CSRF, exfiltratie van gevoelige gegevens)  

---