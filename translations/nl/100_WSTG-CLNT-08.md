## WSTG-CLNT-08 — Testen op Cross Site Flashing

Cross-Site Flashing (XSF) is een kwetsbaarheid aan de clientzijde die optreedt wanneer een Flash-bestand (SWF) onjuist omgaat met door de gebruiker gecontroleerde invoer, waardoor een aanvaller kwaadaardige ActionScript-code kan uitvoeren of een verbinding kan maken met de JavaScript-omgeving van de browser. Door parameters zoals `FlashVars` of URL-querystrings te manipuleren die naar sinks zoals `ExternalInterface.call`, `getURL` of `loadMovie` worden gestuurd, kan een aanvaller acties uitvoeren in de context van het kwetsbare domein, waaronder session hijacking en data exfiltratie. Deze kwetsbaarheid wordt voornamelijk aangetroffen in legacy bedrijfsapplicaties die nog steeds SWF-bestanden hosten, waar onvoldoende invoervalidatie het mogelijk maakt dat de Flash-movie wordt hergebruikt als een vector voor Cross-Site Scripting (XSS) of om de Same-Origin Policy (SOP) te omzeilen via permissieve cross-domain configuraties.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Worden legacy Adobe Flash (.swf) bestanden gehost op de applicatie?
- [ ] Nee — er worden geen Flash-bestanden gehost of naar verwezen binnen de scope van de applicatie  
- [ ] Ja — er zijn SWF-bestanden aanwezig, maar deze bevatten alleen statische inhoud  
- [ ] Ja — er zijn SWF-bestanden aanwezig en deze accepteren dynamische parameters via `FlashVars` of URL-strings  

### Wordt invoer die naar ActionScript-sinks wordt gestuurd gesaneerd?
- [ ] Ja — alle invoer wordt strikt gevalideerd en ActionScript-sinks **kunnen niet** worden gemanipuleerd  
- [ ] Ja — validatie wordt toegepast, maar een bypass **is mogelijk** via specifieke encoding-technieken  
- [ ] Nee — invoer wordt direct doorgegeven aan gevoelige sinks zoals `ExternalInterface.call` of `getURL` *(Kritiek)*  

### Kan het Flash-bestand worden gebruikt om willekeurige JavaScript uit te voeren (XSS)?
- [ ] Nee — `ExternalInterface` is **uitgeschakeld** of de parameter `allowScriptAccess` is ingesteld op `never`  
- [ ] Ja — `allowScriptAccess` is ingesteld op `sameDomain` maar de SWF wordt gehost op het doeldomein  
- [ ] Ja — `allowScriptAccess` is ingesteld op `always`, wat XSS vanaf elk domein toestaat  

### Voorkomt het `crossdomain.xml` beleid ongeautoriseerde cross-origin verzoeken?
- [ ] Ja — `crossdomain.xml` is restrictief en staat alleen vertrouwde, specifieke origins toe  
- [ ] Nee — het `crossdomain.xml` bestand **ontbreekt**  
- [ ] Ja — het beleid is te permissief (bijv. `<allow-access-from domain="*" />`)  

### Kan het SWF-bestand worden gedwongen om externe, door de aanvaller gecontroleerde movies te laden?
- [ ] Nee — de applicatie **kan niet** worden gedwongen om externe SWF-bestanden te laden  
- [ ] Ja — de functies `loadMovie` of `loadMovieNum` accepteren niet-gevalideerde externe URL's  

---