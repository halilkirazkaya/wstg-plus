## WSTG-ATHN-06 — Testen op zwakheden in browsercache

Een zwakheid in de browsercache treedt op wanneer gevoelige informatie wordt opgeslagen in de lokale browsercache en kan worden opgehaald door een onbevoegde gebruiker met toegang tot dezelfde fysieke machine. Dit gebrek aan de juiste cache-control headers zorgt ervoor dat potentieel gevoelige gegevens, zoals persoonlijke informatie, accountgegevens of sessie-identificatoren, behouden blijven nadat de gebruiker is uitgelogd of de sessie heeft gesloten. Aanvallers of volgende gebruikers op gedeelde of publieke terminals kunnen hiervan misbruik maken door door de browsergeschiedenis te navigeren, de "Terug"-knop te gebruiken of lokale cachebestanden te inspecteren om gecachte antwoorden te exfiltreren. Het waarborgen van de implementatie van strikte richtlijnen zoals `Cache-Control: no-store` and `Pragma: no-cache` is essentieel voor elke applicatie die geauthenticeerde inhoud verwerkt, om te garanderen dat gevoelige gegevens nooit naar de schijf worden geschreven.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Medium* |

> *De ernst wordt Medium als de applicatie regelmatig wordt geopend vanaf gedeelde/publieke kiosken of zeer gevoelige PII/PHI-gegevens bevat.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Tools:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Zijn er geschikte cache-control headers aanwezig op geauthenticeerde of gevoelige pagina's?
- [ ] Ja — `Cache-Control: no-store, no-cache` en `Pragma: no-cache` zijn **aanwezig** en **correct geconfigureerd**  
- [ ] Ja — sommige headers zijn aanwezig, maar `no-store` **ontbreekt**, wat caching op de schijf toestaat  
- [ ] Nee — er zijn geen cache-gerelateerde headers **toegepast**, waardoor wordt vertrouwd op het standaardgedrag van de browser  

### Gebruikt de applicatie de `private` richtlijn voor gebruikersspecifieke gegevens?
- [ ] Ja — `Cache-Control: private` is **ingeschakeld** voor alle gepersonaliseerde inhoud om proxy-caching te voorkomen  
- [ ] Nee — inhoud is gemarkeerd als `public` of de `private` richtlijn ontbreekt, waardoor het **gecachet** kan worden door tussenliggende proxy's  

### Is gevoelige informatie toegankelijk via de "Terug"-knop van de browser na een succesvolle afmelding?
- [ ] Nee — de browser vraagt om hernieuwde authenticatie of de pagina wordt **niet geladen** vanuit de lokale cache  
- [ ] Ja — gevoelige informatie is **nog steeds zichtbaar** via de terug-knop nadat de sessie is beëindigd  

### Zijn gevoelige niet-HTML-bestanden (bijv. PDF's, CSV-exports, JSON API-antwoorden) beschermd tegen caching?
- [ ] Nee — er worden geen gevoelige niet-HTML-bestanden gegenereerd of verwerkt door de applicatie  
- [ ] Ja — strikte cache-control headers zijn **toegepast** op alle gevoelige bestandsdownloads en API-endpoints  
- [ ] Nee — gevoelige downloads of API-antwoorden worden **opgeslagen** in de lokale browsercache of tijdelijke mappen  

### Gebruikt de applicatie `Expires: 0` of een datum in het verleden om het gebruik van verouderde gegevens te voorkomen?
- [ ] Ja — de `Expires` header is ingesteld op `0` of een historische datum, wat **garandeert** dat de browser de inhoud als onmiddellijk verouderd beschouwt  
- [ ] Nee — de `Expires` header **ontbreekt** of is ingesteld op een datum in de toekomst voor gevoelige pagina's  

---