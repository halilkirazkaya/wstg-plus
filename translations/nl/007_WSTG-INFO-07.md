## WSTG-INFO-07 — Uitvoeringspaden door de applicatie in kaart brengen

Het in kaart brengen van uitvoeringspaden omvat de systematische identificatie van alle bereikbare eindpunten, functionele stromen en beslissingsvertakkingen binnen een webapplicatie. Dit proces is essentieel om ervoor te zorgen dat er geen "verborgen" functionaliteit — zoals legacy code, debug-interfaces of niet-gedocumenteerde API-routes — buiten het beveiligingsonderzoek blijft, aangezien deze vaak moderne beveiligingscontroles missen. Aanvallers gebruiken een combinatie van geautomatiseerde spidering en handmatige verkenning om de structuur van de applicatie te visualiseren, waarbij ze vaak kwetsbaarheden ontdekken zoals ongeautoriseerde administratieve toegang of fouten in de business logic tijdens het proces. Door inputs te correleren aan specifieke server-side responses kunnen testers gevoelige verwerkingslogica lokaliseren die gerichte diepgaande tests vereist om een robuuste dekking te garanderen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Informationeel |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### Is de applicatie volledig geïndexeerd met behulp van geautomatiseerd en handmatig crawlen?
- [ ] Ja — een uitgebreide kaart van alle zichtbare en gekoppelde bronnen **is voltooid**  
- [ ] Ja — geautomatiseerd crawlen **is voltooid**, maar handmatige verkenning is in behandeling  
- [ ] Nee — de applicatie is **niet** geïndexeerd  

### Zijn verborgen of niet-gedocumenteerde eindpunten geïdentificeerd via JS-analyse of directory brute-forcing?
- [ ] Nee — geen niet-gedocumenteerde paden ontdekt via side-channel analyse  
- [ ] Ja — verborgen paden ontdekt, maar deze **kunnen niet** worden geopend zonder autorisatie  
- [ ] Ja — niet-gedocumenteerde eindpunten **zijn** toegankelijk en bieden gevoelige functionaliteit *(Kritiek / Hoog)*  

### Kunnen uitvoeringspaden worden gewijzigd via niet-standaard parameters of headers?
- [ ] Nee — parameters zoals `debug`, `test`, of `admin` hebben **geen** invloed op de uitvoeringsstroom  
- [ ] Ja — parameters wijzigen de output maar **omzeilen** de beveiligingscontroles niet  
- [ ] Ja — pad-wijzigende parameters **worden toegepast** en maken het mogelijk om de beoogde logica te omzeilen  

### Is de stroom van multi-step business logic (bijv. multi-factor auth, checkout) volledig in kaart gebracht?
- [ ] Ja — alle statussen en overgangen in processen met meerdere stappen **zijn geïdentificeerd**  
- [ ] Nee — complexe state machine transities **kunnen niet** volledig in kaart worden gebracht  

### Zijn API-documentatiebestanden (Swagger/OpenAPI) of client-side mappen (Source Maps) blootgesteld?
- [ ] Nee — er zijn geen architecturale kaarten of documentatiebestanden publiekelijk blootgesteld  
- [ ] Ja — documentatie is aanwezig maar **is beveiligd** door authenticatie  
- [ ] Ja — Swagger UI, OpenAPI JSON, of JS Source Maps **zijn ingeschakeld** en publiekelijk toegankelijk  

---