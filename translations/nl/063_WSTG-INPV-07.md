## WSTG-INPV-07 — XML Injection

XML Injection treedt op wanneer een applicatie door de gebruiker verstrekte gegevens opneemt in een XML-document of -stream zonder de juiste validatie, sanitization of escaping van XML meta-characters. Door specifieke XML-tags of structurele elementen te injecteren, kan een aanvaller de logica van het document manipuleren, authenticatiemechanismen omzeilen of de integriteit van de door de backend verwerkte gegevens verstoren. Deze kwetsbaarheid wordt vaak aangetroffen in op SOAP gebaseerde webdiensten, REST API's die XML accepteren, SAML-assertions en applicaties die dynamisch XML-configuratiebestanden of rapporten genereren. Vanuit het perspectief van een aanvaller is het doel om uit de beoogde gegevenscontext te breken om de documentstructuur te wijzigen, wat mogelijk kan leiden tot ongeautoriseerde privilege escalation of de uitvoering van onbedoelde business logic.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Tools:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### Accepteert en verwerkt de applicatie door XML geformatteerde input van de gebruiker?
- [ ] Nee — applicatie verwerkt **geen** XML-input  
- [ ] Ja — XML-input wordt verwerkt via API's (SOAP/REST)  
- [ ] Ja — XML wordt verwerkt via bestandsuploads (SVG, DOCX, etc.)  

### Worden XML meta-characters (bijv. `<`, `>`, `&`, `'`, `"`) op de juiste manier ge-escaped of gesanitize?
- [ ] Ja — alle meta-characters worden **correct** ge-escaped of gesanitize *(Meest veilig)*  
- [ ] Ja — er wordt enige filtering toegepast, maar een bypass **is mogelijk** met behulp van encoding  
- [ ] Nee — ruwe input wordt direct in XML-structuren ingebed **zonder** sanitization  

### Wordt server-side schema-validatie (XSD/DTD) afgedwongen voor alle XML-inputs?
- [ ] Ja — strikte schema-validatie is **ingeschakeld** en voorkomt structurele wijzigingen  
- [ ] Ja — validatie is **ingeschakeld**, maar voorkomt **geen** tag-injectie binnen datavelden  
- [ ] Nee — schema-validatie is **uitgeschakeld** of niet geïmplementeerd  

### Kan een aanvaller nieuwe XML-tags injecteren om de documentstructuur of -logica te wijzigen?
- [ ] Nee — structuur blijft intact ongeacht de input  
- [ ] Ja — tags kunnen worden geïnjecteerd, maar kunnen de business logic **niet** wijzigen  
- [ ] Ja — tag-injectie **is mogelijk** en staat logica-bypass of gegevensmodificatie toe *(Kritiek)*  

### Kan de XML-parser worden gemanipuleerd met behulp van CDATA-secties of XML-commentaren?
- [ ] Nee — parser verwerkt of weigert CDATA/commentaar-markers correct  
- [ ] Ja — injectie van `<![CDATA[...]]>` of `<!-- ... -->` **is mogelijk** om filters te verbergen of te omzeilen  

---