## WSTG-CONF-13 — Path Confusion

Path Confusion vloeit voort uit discrepanties in de manier waarop verschillende webcomponenten, zoals reverse proxies, load balancers en backend applicatieservers, URL-paden parsen en interpreteren. Aanvallers misbruiken deze inconsistenties door specifieke karakters te injecteren, zoals puntkomma's, gecodeerde slashes of dot-segments, om beveiligingsfilters te misleiden en toegang te krijgen tot beperkte endpoints of statische bronnen. Deze kwetsbaarheid kan leiden tot kritieke beveiligingsfouten, waaronder authentication bypass, ongeautoriseerde toegang tot gegevens en web cache poisoning, aangezien de front-end beveiligingsregels kan toepassen op een pad dat door de back-end anders wordt geïnterpreteerd. Succesvolle exploitatie vindt doorgaans plaats op de architecturale grens waar request routing en normalisatielogica uiteenlopen binnen de tech stack.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als path confusion resulteert in een bypass van authenticatie of toegangscontrole voor gevoelige administratieve endpoints.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Interpreteren verschillende architecturale lagen pad-scheidingstekens (bijv. `;`, `#`, `?`) consistent?
- [ ] Ja — alle lagen normaliseren en interpreteren pad-scheidingstekens identiek  
- [ ] Nee — er bestaan inconsistenties, maar deze **kunnen niet** worden gebruikt om toegang te krijgen tot beperkte bronnen  
- [ ] Nee — discrepanties maken het voor een aanvaller **mogelijk** om front-end filters te omzeilen en de back-end logica te bereiken  

### Kunnen toegangscontroles worden omzeild met behulp van path traversal sequenties of gecodeerde karakters?
- [ ] Nee — normalisatie wordt consistent toegepast vóór beveiligingscontroles  
- [ ] Ja — bypass is **mogelijk** via dot-segment sequenties (bijv. `/admin/..;/`)  
- [ ] Ja — bypass is **mogelijk** via URL-encoded karakters (bijv. `%2f`, `%2e%2e%2f`)  

### Is de applicatie vatbaar voor Web Cache Poisoning via path confusion?
- [ ] Nee — caching-logica wordt niet beïnvloed door onduidelijkheid in het pad of extra pad-informatie  
- [ ] Ja — kwaadaardige inhoud **kan** worden gecachet voor legitieme paden als gevolg van mapping-discrepanties  
- [ ] Ja — gevoelige informatie **kan** worden gecachet in publieke mappen via `RCD` (Relative Path Overwrite) technieken  

### Verwerkt de server "extra pad-informatie" (PathInfo) op een veilige manier?
- [ ] Nee — functie is **uitgeschakeld** of bestaat niet  
- [ ] Ja — `PathInfo` wordt afgehandeld en interfereert **niet** met beveiligingsfilters  
- [ ] Nee — `PathInfo` stelt een aanvaller in staat om gegevens toe te voegen die de routing-logica van de applicatie verwarren  

---