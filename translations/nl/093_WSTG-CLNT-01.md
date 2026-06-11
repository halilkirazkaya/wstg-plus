## WSTG-CLNT-01 — Testing for DOM Based Cross Site Scripting

DOM-based Cross-Site Scripting (DOM XSS) treedt op wanneer een applicatie client-side JavaScript bevat die gegevens van een onbetrouwbare bron op een onveilige manier verwerkt, meestal door de gegevens terug te schrijven naar het Document Object Model (DOM) via een gevaarlijke sink. In tegenstelling tot reflected of stored XSS, bevindt de kwetsbaarheid zich volledig in de client-side code; de payload wordt vaak helemaal niet naar de server verzonden, omdat deze wordt verwerkt door sinks zoals `innerHTML`, `eval()`, of `document.write()`. Aanvallers misbruiken dit door URL's op te stellen met kwaadaardige fragmenten of parameters die, wanneer ze door de browser van een slachtoffer worden geladen, willekeurige JavaScript uitvoeren in de context van de gebruikerssessie. Dit kan leiden tot de exfiltratie van sessietokens, diefstal van gevoelige gegevens en ongeautoriseerde acties uitgevoerd namens de geauthenticeerde gebruiker.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Worden onbetrouwbare bronnen gebruikt om door de gebruiker gecontroleerde gegevens vast te leggen in JavaScript?
- [ ] Nee — JavaScript gebruikt **geen** `location.hash`, `location.search`, of `document.referrer`  
- [ ] Ja — onbetrouwbare bronnen worden gebruikt, maar gegevens worden **niet** doorgegeven aan execution sinks  
- [ ] Ja — onbetrouwbare bronnen worden gebruikt en gegevens vloeien rechtstreeks in de client-side logica  

### Worden door de gebruiker gecontroleerde gegevens doorgegeven aan gevaarlijke DOM sinks?
- [ ] Nee — sinks zoals `innerHTML`, `outerHTML`, `document.write()`, of `eval()` worden **niet** gebruikt  
- [ ] Ja — gevaarlijke sinks zijn aanwezig, maar de gegevens worden strikt **gesaneerd** met een veilige bibliotheek zoals DOMPurify  
- [ ] Ja — gevaarlijke sinks zijn aanwezig en gegevens worden verwerkt met **onvoldoende** of aangepaste op regex gebaseerde filtering  
- [ ] Ja — gevaarlijke sinks zijn aanwezig en gegevens worden doorgegeven **zonder** enige sanering of encoding *(Kritiek)*  

### Kan de execution sink worden bereikt via een op URL gebaseerde payload?
- [ ] Nee — source-to-sink flow kan **niet** worden geactiveerd via externe invoer  
- [ ] Ja — flow is **mogelijk**, maar vereist complexe gebruikersinteractie  
- [ ] Ja — flow is **mogelijk** en kan worden geactiveerd via een eenvoudig URL-fragment of queryparameter  

### Neutraliseren moderne security headers of op de browser gebaseerde mitigaties het risico?
- [ ] Ja — een strikt `Content-Security-Policy` (CSP) met `script-src` en `trusted-types` is **ingeschakeld** en voorkomt uitvoering  
- [ ] Ja — er is een CSP aanwezig, maar `unsafe-inline` of zwakke hashes maken een bypass **mogelijk**  
- [ ] Nee — er is geen CSP aanwezig om op DOM gebaseerde uitvoering te beperken  

### Maakt de applicatie gebruik van client-side frameworks met ingebouwde beveiligingen?
- [ ] Ja — framework (bijv. Angular, React) codeert gegevens automatisch en een bypass is **niet mogelijk**  
- [ ] Ja — framework wordt gebruikt, maar "escape hatches" zoals `dangerouslySetInnerHTML` zijn **ingeschakeld**  
- [ ] Nee — de applicatie gebruikt vanilla JavaScript of legacy bibliotheken (bijv. oude jQuery-versies) zonder automatische escaping  

---