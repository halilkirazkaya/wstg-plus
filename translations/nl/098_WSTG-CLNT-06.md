## WSTG-CLNT-06 — Testen op Client-Side Resource Manipulation

Client-side resource manipulation vindt plaats wanneer een applicatie toestaat dat door de gebruiker gecontroleerde invoer de bron of het pad van door de browser geladen bronnen beïnvloedt, zoals scripts, stylesheets of iframes. Door URI fragments, query parameters of andere DOM sources te manipuleren, kan een aanvaller de applicatie dwingen om kwaadaardige externe inhoud te laden of ongeautoriseerde code uit te voeren. Deze kwetsbaarheid wordt doorgaans aangetroffen in JavaScript-logica die dynamisch de `src`- of `href`-attributen van HTML-elementen vult zonder voldoende validatie tegen een vertrouwde allow-list. Vanuit het perspectief van een aanvaller wordt dit misbruikt door een URL op te stellen die het laden van bronnen omleidt naar een door de aanvaller beheerde server, wat kan leiden tot DOM-based Cross-Site Scripting (XSS), exfiltratie van gevoelige gegevens of UI redressing.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### Maakt de applicatie gebruik van client-side sinks om bronnen dynamisch te laden of in te sluiten?
- [ ] Nee — bronnen worden uitsluitend geladen via statische HTML of server-side logica  
- [ ] Ja — client-side JavaScript vult dynamisch attributen in zoals `src`, `href` of `data`  

### Zijn er validatiecontroles of allow-lists geïmplementeerd voor URI-bronnen?
- [ ] Ja — strikte allow-lists en origin-validatie **worden toegepast** op alle dynamische bronnen  
- [ ] Ja — validatie is aanwezig, maar vertrouwt op zwakke blacklists of eenvoudig te omzeilen regex  
- [ ] Nee — er worden geen validatiecontroles toegepast op door de gebruiker gecontroleerde paden van bronnen  

### Is het mogelijk om URI-validatie te omzeilen met behulp van alternatieve protocollen of codering?
- [ ] Nee — het omzeilen van filters voor bronnen is **niet mogelijk**  
- [ ] Ja — omzeiling **is mogelijk** met behulp van verschillende protocollen zoals `data:`, `vbscript:` of `javascript:`  
- [ ] Ja — omzeiling **is mogelijk** door gebruik te maken van gecodeerde karakters of null bytes om eenvoudige string-matches te omzeilen  

### Kan een aanvaller het laden van bronnen omleiden naar een extern, niet-vertrouwd domein?
- [ ] Nee — paden van bronnen zijn beperkt tot dezelfde origin of een vaste submap  
- [ ] Ja — de applicatie **kan** worden gedwongen om scripts of stijlen te laden vanaf een willekeurig extern domein  

### Wat is de maximale impact van de resource manipulation?
- [ ] Laag — manipulatie heeft alleen invloed op niet-uitvoerbare elementen zoals afbeeldingen of niet-gevoelige tekst  
- [ ] Gemiddeld — manipulatie maakt CSS-injectie of aanzienlijke UI redressing mogelijk  
- [ ] Hoog — manipulatie leidt tot DOM-based XSS of de uitvoering van willekeurige JavaScript *(Kritiek)*  

---