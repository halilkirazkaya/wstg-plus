## WSTG-CLNT-05 — Testen op CSS Injection

CSS Injection treedt op wanneer een applicatie toestaat dat door de gebruiker verstrekte invoer de Cascading Style Sheets (CSS) van een webpagina beïnvloedt zonder de juiste sanitization of escaping. Hoewel het vaak wordt gezien als een cosmetisch probleem, kunnen aanvallers CSS injection misbruiken om gevoelige gegevens, zoals CSRF-tokens of sessie-ID's, te exfiltreren door gebruik te maken van attribute selectors en background-image eigenschappen om externe verzoeken te triggeren. Deze kwetsbaarheid doet zich doorgaans voor bij eindpunten waar gebruikersvoorkeuren (bijv. aangepaste thema's, lettertypekleuren) worden gereflecteerd binnen `<style>` blokken of inline `style` attributen. Vanuit het perspectief van een aanvaller kan een succesvolle exploitatie leiden tot UI redressing, phishing via ongeautoriseerde wijziging van de lay-out, of heimelijke data-exfiltratie in omgevingen waar een strikt Content Security Policy (CSP) anders op JavaScript gebaseerde aanvallen zou blokkeren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als het injectiepunt de exfiltratie van gevoelige attributen zoals CSRF-tokens toestaat of als het kan worden gebruikt voor UI redressing in gevoelige workflows.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Reflecteert de applicatie door de gebruiker gecontroleerde invoer binnen CSS-contexten?
- [ ] Nee — invoer wordt **niet** gereflecteerd in style-tags, attributen of CSS-bestanden  
- [ ] Ja — invoer wordt gereflecteerd maar is strikt beperkt tot veilige alfanumerieke waarden  
- [ ] Ja — invoer wordt gereflecteerd binnen een `style` attribuut of `<style>` blok  

### Worden CSS-specifieke metatekens en functies correct gesaneerd?
- [ ] Ja — tekens zoals `{`, `}`, `:` en functies zoals `url()` worden **correct ge-escaped**  
- [ ] Ja — sanitization is aanwezig, maar een bypass **is mogelijk** via encoding of CSS-comments  
- [ ] Nee — er wordt geen sanitization toegepast op CSS-metatekens  

### Is data-exfiltratie mogelijk met behulp van CSS-selectors?
- [ ] Nee — selectors **kunnen geen** externe verzoeken triggeren of gegevens lekken  
- [ ] Ja — gedeeltelijke data-exfiltratie **is mogelijk** via attribute selectors en `background-image`  
- [ ] Ja — volledige exfiltratie van gevoelige tokens **is mogelijk** met behulp van geautomatiseerde brute-force technieken  

### Beperkt een Content Security Policy (CSP) het risico op CSS injection?
- [ ] Ja — `style-src` is beperkt tot 'self' en `img-src` of `connect-src` **is beperkt**  
- [ ] Ja — CSP is aanwezig maar gebruikt 'unsafe-inline' of staat wildcard externe domeinen toe  
- [ ] Nee — er is geen CSP geïmplementeerd om het laden van externe bronnen via CSS te voorkomen  

### Kan de kwetsbaarheid worden gebruikt voor UI redressing of phishing?
- [ ] Nee — de reikwijdte van de injectie is te beperkt om de lay-out significant te wijzigen  
- [ ] Ja — wijziging van de UI **is mogelijk**, wat het overlayeren van kwaadaardige elementen toestaat  
- [ ] Ja — volledige controle over de pagina-lay-out **is mogelijk**, wat zeer overtuigende phishing-aanvallen vergemakkelijkt  

---