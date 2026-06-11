## WSTG-CLNT-03 — HTML Injection

HTML Injection vindt plaats wanneer een applicatie er niet in slaagt om door de gebruiker geleverde invoer te saniteren voordat deze in de browser wordt gerenderd, waardoor een aanvaller willekeurige HTML-tags in het Document Object Model (DOM) van de webpagina kan injecteren. Hoewel het vergelijkbaar is met Cross-Site Scripting (XSS), is het primaire doel van HTML Injection het manipuleren van de visuele weergave van de pagina of het faciliteren van phishing-aanvallen door het injecteren van kwaadaardige formulieren en misleidende inhoud. Aanvallers richten zich op parameters die worden gereflecteerd in de UI, zoals zoektermen, gebruikersprofieldelen of foutmeldingen, om slachtoffers te misleiden tot het onthullen van gevoelige inloggegevens of interactie met ongeautoriseerde links van derden. Deze kwetsbaarheid vormt een aanzienlijk risico voor de integriteit van de gebruikersinterface van de applicatie en kan een voorloper zijn van complexere social engineering- of sessiegerelateerde aanvallen.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Laag / Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Worden door de gebruiker gecontroleerde invoervelden in de DOM gereflecteerd zonder de juiste HTML-encoding?
- [ ] Nee — alle gereflecteerde invoer is strikt HTML-encoded voordat deze wordt gerenderd  
- [ ] Ja — sommige tekens **zijn** ge-encoded, maar bypasses voor bepaalde tags **zijn mogelijk**  
- [ ] Ja — invoer wordt onbewerkt gereflecteerd en HTML injection **is mogelijk**  

### Kunnen structurele HTML-elementen worden geïnjecteerd om de lay-out van de pagina te wijzigen?
- [ ] Nee — de applicatie filtert of verwijdert structurele tags zoals `<div>`, `<table>`, of `<iframe>`  
- [ ] Ja — structurele elementen **kunnen** worden geïnjecteerd, maar hebben **geen** significante impact op de UI  
- [ ] Ja — aanzienlijke UI-vervorming (defacement) **is mogelijk** via geïnjecteerde tags  

### Is het mogelijk om functionele phishing-elementen zoals formulieren of kwaadaardige links te injecteren?
- [ ] Nee — `<form>`, `<input>`, en `<a>` tags worden effectief geblokkeerd of gesaniteerd  
- [ ] Ja — kwaadaardige links **kunnen** worden geïnjecteerd om gebruikers naar externe sites om te leiden  
- [ ] Ja — functionele `<form>` elementen **kunnen** worden geïnjecteerd om inloggegevens te verzamelen *(Medium)*  

### Beperken beveiligingsheaders of Content Security Policies (CSP) de impact van de injectie?
- [ ] Ja — er is een beperkende CSP aanwezig en `form-action` of `frame-src` **is toegepast**  
- [ ] Nee — beveiligingsheaders zijn aanwezig, maar de CSP **is uitgeschakeld** of bevat unsafe-inline/wildcards  
- [ ] Nee — er zijn geen CSP of relevante beveiligingsheaders **ingeschakeld**  

---