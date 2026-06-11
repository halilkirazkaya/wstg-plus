## WSTG-INPV-07 — XML Injection

XML Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Daten in ein XML-Dokument oder einen Stream einfügt, ohne eine ordnungsgemäße Validierung, Bereinigung (Sanitization) oder Maskierung (Escaping) von XML-Metazeichen durchzuführen. Durch das Injizieren spezifischer XML-Tags oder Strukturelemente kann ein Angreifer die Logik des Dokuments manipulieren, Authentifizierungsmechanismen umgehen oder die Integrität der vom Backend verarbeiteten Daten beeinträchtigen. Diese Schwachstelle tritt häufig in SOAP-basierten Webdiensten, REST APIs, die XML akzeptieren, SAML-Assertions sowie in Anwendungen auf, die XML-Konfigurationsdateien oder Berichte dynamisch generieren. Aus der Sicht eines Angreifers besteht das Ziel darin, aus dem vorgesehenen Datenkontext auszubrechen, um die Dokumentstruktur zu verändern, was potenziell zu einer unbefugten Ausweitung von Berechtigungen (Privilege Escalation) oder der Ausführung von unbeabsichtigter Geschäftslogik führen kann.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Tools:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### Akzeptiert und verarbeitet die Anwendung vom Benutzer bereitgestellte Eingaben im XML-Format?
- [ ] Nein — die Anwendung verarbeitet keine XML-Eingaben  
- [ ] Ja — XML-Eingaben werden über APIs (SOAP/REST) verarbeitet  
- [ ] Ja — XML wird über Datei-Uploads (SVG, DOCX usw.) verarbeitet  

### Werden XML-Metazeichen (z. B. `<`, `>`, `&`, `'`, `"`) ordnungsgemäß maskiert oder bereinigt?
- [ ] Ja — alle Metazeichen werden **ordnungsgemäß** maskiert oder bereinigt *(Am sichersten)*  
- [ ] Ja — es wird eine gewisse Filterung angewendet, aber ein Bypass ist mittels Kodierung **möglich**  
- [ ] Nein — Roheingaben werden **ohne** Bereinigung direkt in XML-Strukturen eingebettet  

### Wird eine serverseitige Schema-Validierung (XSD/DTD) für alle XML-Eingaben erzwungen?
- [ ] Ja — eine strikte Schema-Validierung ist **aktiviert** und verhindert strukturelle Änderungen  
- [ ] Ja — die Validierung ist **aktiviert**, verhindert jedoch **keine** Tag-Injection innerhalb von Datenfeldern  
- [ ] Nein — die Schema-Validierung ist **deaktiviert** oder nicht implementiert  

### Kann ein Angreifer neue XML-Tags injizieren, um die Struktur oder Logik des Dokuments zu verändern?
- [ ] Nein — die Struktur bleibt unabhängig von der Eingabe intakt  
- [ ] Ja — Tags können injiziert werden, können aber die Geschäftslogik **nicht** verändern  
- [ ] Ja — eine Tag-Injection **ist möglich** und erlaubt die Umgehung der Logik oder Datenmodifikation *(Kritisch)*  

### Kann der XML-Parser mithilfe von CDATA-Abschnitten oder XML-Kommentaren manipuliert werden?
- [ ] Nein — der Parser verarbeitet oder lehnt CDATA-/Kommentar-Markierungen korrekt ab  
- [ ] Ja — die Injection von `<![CDATA[...]]>` oder `<!-- ... -->` **ist möglich**, um Filter zu verbergen oder zu umgehen  

---