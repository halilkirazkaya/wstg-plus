## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Reflected Cross Site Scripting (XSS) treedt op wanneer een applicatie onvertrouwde gegevens opneemt in een HTTP-respons zonder voldoende validatie of encoding, waardoor de payload wordt uitgevoerd in de context van de browser van het slachtoffer. Aanvallers leveren kwaadaardige payloads via social engineering, doorgaans via gemanipuleerde URL's of formulieren, om gebruikerssessies over te nemen, gevoelige cookies te exfiltreren of ongeautoriseerde acties uit te voeren namens de gebruiker. Deze kwetsbaarheid wordt vaak aangetroffen in zoekparameters, foutmeldingen en elk endpoint dat invoer direct teruggeeft aan de gebruikersinterface. Exploitatie is afhankelijk van de interactie van het slachtoffer met een kwaadaardige link, waardoor het een niet-persistente maar zeer effectieve aanvalsvector is voor het targeten van specifieke beheerders of geauthenticeerde gebruikers.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Worden door de gebruiker verstrekte invoergegevens gereflecteerd in de response body?
- [ ] Nee — invoer wordt **nooit** teruggekoppeld aan de gebruiker  
- [ ] Ja — invoer wordt gereflecteerd maar is correct geëncodeerd en een bypass is **niet mogelijk**  
- [ ] Ja — invoer wordt gereflecteerd **zonder** enige encoding of sanitization  

### Is contextbewuste output encoding geïmplementeerd?
- [ ] Ja — de juiste encoding (HTML, Attribute, JavaScript) wordt **toegepast** op basis van de specifieke reflectiecontext  
- [ ] Ja — encoding wordt **toegepast** maar is onvoldoende voor de specifieke context (bijv. HTML-encoding binnen een `<script>` tag)  
- [ ] Nee — encoding wordt **niet toegepast**  

### Kunnen inputvalidatie of Web Application Firewall (WAF) filters worden omzeild?
- [ ] Nee — filters blokkeren effectief alle algemene en geobfusceerde XSS payloads  
- [ ] Ja — filters zijn aanwezig maar een bypass **is mogelijk** door gebruik te maken van character encoding, niet-standaard tags of polyglots  
- [ ] Nee — er zijn geen filters of validatiemechanismen aanwezig  

### Wat is de context van uitvoering van de gereflecteerde invoer?
- [ ] Veilig — invoer wordt gereflecteerd op een niet-uitvoerbare locatie (bijv. binnen standaard `<div>` of `<span>` tags)  
- [ ] Riskant — invoer wordt gereflecteerd binnen HTML-attributen of binnen de `src`/`href` van tags  
- [ ] Kritiek — invoer wordt direct gereflecteerd binnen `<script>` blokken, event handlers of template literals  

### Zijn er moderne browser security headers aanwezig om de impact van XSS te beperken?
- [ ] Ja — `Content-Security-Policy` (CSP) is **ingeschakeld** met een beperkende script-src  
- [ ] Ja — `Content-Security-Policy` (CSP) is **ingeschakeld** maar bevat `unsafe-inline` of zwakke whitelists  
- [ ] Nee — er zijn geen CSP of legacy `X-XSS-Protection` headers aanwezig  

---