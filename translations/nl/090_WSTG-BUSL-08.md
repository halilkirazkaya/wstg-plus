## WSTG-BUSL-08 — Testen van het uploaden van onverwachte bestandstypen

Het uploaden van onverwachte bestandstypen stelt aanvallers in staat om business logic-beperkingen te omzeilen door bestanden in te dienen die de applicatie niet bedoeld heeft te verwerken, wat mogelijk kan leiden tot Remote Code Execution (RCE), Cross-Site Scripting (XSS) of Denial of Service (DoS). Aanvallers onderzoeken deze kwetsbaarheden door bestandsextensies, MIME-typen en Magic Bytes aan te passen om de validatielogica aan de serverzijde te misleiden, zodat deze kwaadaardige inhoud accepteert. Dit doet zich doorgaans voor bij het uploaden van profielfoto's, documentbeheersystemen of bijlagen bij support-tickets waar onvoldoende validatie aanwezig is op de back-end. Succesvolle exploitatie kan de hostserver compromitteren als het geüploade bestand wordt uitgevoerd, of leiden tot client-side aanvallen als het bestand aan andere gebruikers wordt geserveerd met een onjuiste Content-Type.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst / Severity** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Beperkt de applicatie bestandsuploads tot een specifieke set extensies?
- [ ] Ja — een strikte whitelist van extensies wordt afgedwongen en bypass is **niet mogelijk**  
- [ ] Ja — een whitelist van extensies is aanwezig, maar een bypass **is mogelijk** via dubbele extensies of null bytes  
- [ ] Nee — elke bestandsextensie **kan** worden geüpload  

### Wordt de bestandsinhoud gevalideerd buiten de bestandsextensie?
- [ ] Ja — server-side logica verifieert Magic Bytes en voert diepgaande inhoudsinspectie uit  
- [ ] Ja — server-side logica verifieert het MIME-type uitsluitend via de `Content-Type` header  
- [ ] Nee — er **wordt geen** inhoudsvalidatie toegepast na de controle van de extensie  

### Kan een aanvaller code uitvoeren door een web shell of script te uploaden?
- [ ] Nee — geüploade bestanden worden opgeslagen in een niet-uitvoerbare directory of een storage bucket (S3/Azure Blob)  
- [ ] Ja — bestanden worden opgeslagen in een via het web toegankelijke directory, maar uitvoering **is uitgeschakeld** via serverconfiguratie  
- [ ] Ja — geüploade kwaadaardige scripts **kunnen** direct worden uitgevoerd via een voorspelbaar URL-pad *(Kritiek)*  

### Zijn client-side aanvallen zoals XSS mogelijk via geüploade bestandstypen?
- [ ] Nee — bestanden worden geserveerd met `Content-Disposition: attachment` en `X-Content-Type-Options: nosniff`  
- [ ] Ja — SVG- of HTML-bestanden **kunnen** worden geüpload en gerenderd in de browser, wat leidt tot XSS  
- [ ] Ja — metadata van afbeeldingen (EXIF) **wordt verwerkt** en weergegeven in de UI zonder opschoning (sanitization)  

---