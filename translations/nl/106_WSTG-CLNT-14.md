## WSTG-CLNT-14 — Testen op Reverse Tabnabbing

Reverse Tabnabbing is een client-side kwetsbaarheid waarbij een pagina die wordt geopend via `target="_blank"` een functionele referentie naar de bovenliggende pagina behoudt via het `window.opener` object. Dit stelt een door de aanvaller gecontroleerde bestemmingssite in staat om het originele, vertrouwde tabblad programmatisch om te leiden naar een kwaadaardige URL, meestal een phishing-pagina die is ontworpen om inloggegevens te stelen. De aanval maakt misbruik van het inherente vertrouwen van de gebruiker in het oorspronkelijke tabblad, aangezien het onwaarschijnlijk is dat zij merken dat de pagina die ze zojuist hebben verlaten, naar een ander domein is genavigeerd. Moderne beveiligingspraktijken vereisen het gebruik van de attributen `rel="noopener"` of `rel="noreferrer"` op anchor-tags, of het instellen van de `opener`-eigenschap op null in JavaScript, om deze communicatie tussen vensters te voorkomen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld* |

> *De ernst is Laag in de meeste moderne browsers, aangezien deze `target="_blank"` nu standaard instellen op `rel="noopener"`. De ernst wordt Gemiddeld als de applicatie zich richt op verouderde browsers of `window.open()` gebruikt zonder de eigenschap `opener` op null in te stellen.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Zijn er links aanwezig die in een nieuw venster of tabblad openen?
- [ ] Nee — er worden geen links met `target="_blank"` of gelijkwaardige op JavaScript gebaseerde omleidingen gebruikt  
- [ ] Ja — `target="_blank"` wordt uitsluitend gebruikt op interne, vertrouwde links  
- [ ] Ja — `target="_blank"` wordt gebruikt op externe links of door de gebruiker opgegeven URL's  

### Is de `window.opener`-relatie beperkt op anchor-tags?
- [ ] Ja — `rel="noopener"` of `rel="noreferrer"` wordt **consequent toegepast** op alle links met `target="_blank"` *(Meest veilig)*  
- [ ] Ja — attributen worden toegepast, maar **ontbreken** op risicovolle of door gebruikers gegenereerde links  
- [ ] Nee — attributen worden **niet toegepast**, waardoor de onderliggende pagina toegang heeft tot `window.opener`  

### Is de applicatie kwetsbaar voor Reverse Tabnabbing via JavaScript `window.open()`?
- [ ] Nee — `window.open()` wordt niet gebruikt of stelt expliciet de eigenschap `opener` in op null  
- [ ] Ja — `window.open()` wordt gebruikt en de `opener`-referentie **blijft toegankelijk** voor het onderliggende venster  

### Kan een aanvaller de bestemming bepalen van een link die in een nieuw tabblad opent?
- [ ] Nee — alle linkbestemmingen zijn hardcoded en verwijzen naar vertrouwde domeinen  
- [ ] Ja — linkbestemmingen worden door de gebruiker opgegeven (bijv. social media-profielen, bio-links) en zijn **niet** geschoond voor tabnabbing-beveiligingen  

---