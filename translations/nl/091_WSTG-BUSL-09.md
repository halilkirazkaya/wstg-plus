## WSTG-BUSL-09 — Test het uploaden van kwaadaardige bestanden

Functionaliteit voor het uploaden van bestanden stelt gebruikers in staat gegevens naar de server te verzenden, wat een direct vector vormt voor het introduceren van kwaadaardige inhoud in de omgeving van de applicatie. Aanvallers maken misbruik van zwakke validatielogica om web shells, malware of speciaal samengestelde bestanden — zoals SVG of HTML — te uploaden om Remote Code Execution (RCE) of Cross-Site Scripting (XSS) te bewerkstelligen. Deze kwetsbaarheid wordt doorgaans aangetroffen in functies zoals profielinstellingen, documentbeheersystemen en bijlagenverwerkers waar de server nalaat om bestandstypen, inhoud en uitvoeringsrechten correct te verifiëren. Succesvolle exploitatie leidt vaak tot volledige systeemcompromis, data-exfiltratie of het gebruik van de server als pivot point voor aanvallen op het interne netwerk.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### Biedt de applicatie functionaliteit om bestanden te uploaden?
- [ ] Nee — er bestaat geen functionaliteit voor het uploaden van bestanden  
- [ ] Ja — functionaliteit bestaat voor geauthenticeerde gebruikers  
- [ ] Ja — functionaliteit is beschikbaar voor **niet-geauthenticeerde** gebruikers *(Kritiek)*  

### Wordt validatie van bestandsextensies afgedwongen aan de serverzijde?
- [ ] Ja — een strikte allowlist van extensies wordt **toegepast** en bypass is **niet mogelijk**  
- [ ] Ja — een allowlist wordt gebruikt, maar bypass **is mogelijk** via hoofdlettergevoeligheid of dubbele extensies  
- [ ] Ja — er wordt een blocklist gebruikt, die **kan** worden omzeild met alternatieve extensies (bijv. `.phtml`, `.asa`)  
- [ ] Nee — er wordt geen extensievalidatie **toegepast** aan de serverzijde  

### Wordt de bestandsinhoud gevalideerd buiten de extensie of het MIME-type?
- [ ] Ja — de applicatie verifieert magic bytes en voert diepgaande inhoudsinspectie uit  
- [ ] Ja — de applicatie controleert alleen de `Content-Type` header, die eenvoudig kan worden gespooft  
- [ ] Nee — de applicatie vertrouwt volledig op de bestandsextensie voor validatie  

### Worden geüploade bestanden opgeslagen in een directory met uitvoeringsrechten?
- [ ] Nee — bestanden worden opgeslagen op een speciale opslagservice (bijv. S3) of in een niet-uitvoerbare directory  
- [ ] Ja — bestanden worden opgeslagen op de webserver, maar uitvoering is **uitgeschakeld** via configuratie  
- [ ] Ja — bestanden worden opgeslagen in een via het web toegankelijke directory en uitvoering **is ingeschakeld** *(Kritiek)*  

### Kunnen beveiligingsfilters worden omzeild via bestandsnaammanipulatie?
- [ ] Nee — bestandsnamen worden geschoond of willekeurig opnieuw gegenereerd door de server  
- [ ] Ja — filters **kunnen** worden omzeild met null bytes (bijv. `shell.php%00.jpg`)  
- [ ] Ja — path traversal karakters (bijv. `../../`) **kunnen** worden gebruikt om bestanden naar willekeurige directories te uploaden  

---