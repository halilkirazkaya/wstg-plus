## WSTG-CLNT-13 — Testen op Cross Site Script Inclusion

Cross-Site Script Inclusion (XSSI) treedt op wanneer een webapplicatie gevoelige gegevens exporteert in een dynamisch gegenereerd JavaScript-bestand of JSONP-respons, die een door een aanval gecontroleerde site vervolgens kan insluiten via een `<script>`-tag. Omdat script-insluiting de Same-Origin Policy (SOP) omzeilt, kan een aanvaller het script in zijn eigen context uitvoeren en globale objecten of variabelen overschrijven om de gevoelige informatie te exfiltreren. Deze kwetsbaarheid manifesteert zich doorgaans in geauthenticeerde eindpunten die gebruikersspecifieke gegevens zoals e-mailadressen, API-sleutels of sessiemetadata in scriptformaat retourneren. Vanuit het perspectief van een aanvaller omvat exploitatie het opstellen van een kwaadaardige pagina die naar het kwetsbare script verwijst en de resulterende wijzigingen in globale variabelen of functieaanroepen onderschept.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Gemiddeld / Hoog |

> *De Severity wordt Hoog als de gelekte gegevens CSRF-tokens, sessie-identifiers of zeer gevoelige PII bevatten.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Retourneren geauthenticeerde eindpunten dynamische JavaScript of JSONP die gevoelige informatie bevat?
- [ ] Nee — alle scripts zijn statisch en **kunnen geen** gebruikersspecifieke gegevens bevatten  
- [ ] Ja — er bestaan dynamische scripts, maar deze **bevatten geen** gevoelige informatie  
- [ ] Ja — dynamische scripts bevatten PII of tokens en **zijn** toegankelijk via verschillende origins  

### Is de `X-Content-Type-Options: nosniff`-header geïmplementeerd op gevoelige script-responses?
- [ ] Ja — de header is **toegepast** en voorkomt MIME-type sniffing  
- [ ] Nee — de header **ontbreekt**, wat mogelijk toestaat dat niet-scriptinhoud als script wordt uitgevoerd  

### Worden gevoelige scripts beschermd door anti-XSSI mechanismen zoals niet-uitvoerbare prefixes of aangepaste headers?
- [ ] Ja — scripts vereisen een aangepaste header of token die **niet** via een standaard `<script>`-tag kan worden verzonden  
- [ ] Ja — scripts maken gebruik van niet-uitvoerbare prefixes (bijv. `)]}'`) die **niet** eenvoudig kunnen worden omzeild  
- [ ] Nee — scripts zijn direct toegankelijk via GET-verzoeken met gebruik van ambient credentials *(Kritiek)*  

### Is exfiltratie van gevoelige gegevens mogelijk door globale variabelen of prototypes te overschrijven?
- [ ] Nee — gevoelige gegevens zijn lokaal gescoped of beschermd via moderne browserfuncties  
- [ ] Ja — gevoelige gegevens zijn toegewezen aan globale variabelen en **kunnen** worden onderschept  
- [ ] Ja — gegevens worden gelekt via JSONP-callbacks die **kunnen** worden onderschept  

---