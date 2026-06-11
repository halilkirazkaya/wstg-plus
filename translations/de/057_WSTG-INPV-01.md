## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Reflected Cross Site Scripting (XSS) tritt auf, wenn eine Anwendung nicht vertrauenswürdige Daten ohne ausreichende Validierung oder Kodierung in eine HTTP-Antwort einfügt, wodurch der Payload im Browser-Kontext des Opfers ausgeführt wird. Angreifer übermitteln bösartige Payloads mittels Social Engineering, in der Regel durch manipulierte URLs oder Formulare, um Benutzersitzungen zu kapern, sensible Cookies zu exfiltrieren oder unbefugte Aktionen im Namen des Benutzers durchzuführen. Diese Schwachstelle tritt häufig in Suchparametern, Fehlermeldungen und allen Endpunkten auf, die Eingaben direkt an die Benutzeroberfläche zurückgeben. Die Ausnutzung setzt voraus, dass das Opfer mit einem schädlichen Link interagiert, was es zu einem nicht-persistenten, aber hochwirksamen Angriffsvektor gegen spezifische administrative oder authentifizierte Benutzer macht.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Werden vom Benutzer bereitgestellte Eingaben im Response Body reflektiert?
- [ ] Nein — Eingaben werden **nie** an den Benutzer zurückgegeben  
- [ ] Ja — Eingaben werden reflektiert, sind jedoch ordnungsgemäß kodiert und ein Bypass ist **nicht möglich**  
- [ ] Ja — Eingaben werden **ohne** Kodierung oder Bereinigung reflektiert  

### Ist eine kontextsensitive Ausgabekodierung implementiert?
- [ ] Ja — eine korrekte Kodierung (HTML, Attribute, JavaScript) wird basierend auf dem spezifischen Reflexionskontext **angewendet**  
- [ ] Ja — eine Kodierung wird **angewendet**, ist jedoch für den spezifischen Kontext unzureichend (z. B. HTML-Kodierung innerhalb eines `<script>`-Tags)  
- [ ] Nein — es wird **keine** Kodierung angewendet  

### Können Eingabevalidierungen oder Filter der Web Application Firewall (WAF) umgangen werden?
- [ ] Nein — Filter blockieren effektiv alle gängigen und obfuskerten XSS-Payloads  
- [ ] Ja — Filter sind vorhanden, aber ein Bypass **ist möglich** durch Zeichenkodierung, nicht-standardkonforme Tags oder Polyglots  
- [ ] Nein — es sind keine Filter oder Validierungsmechanismen vorhanden  

### Was ist der Ausführungskontext der reflektierten Eingabe?
- [ ] Sicher — die Eingabe wird an einer nicht ausführbaren Stelle reflektiert (z. B. innerhalb von standardmäßigen `<div>`- oder `<span>`-Tags)  
- [ ] Riskant — die Eingabe wird innerhalb von HTML-Attributen oder in den `src`/`href`-Attributen von Tags reflektiert  
- [ ] Kritisch — die Eingabe wird direkt in `<script>`-Blöcken, Event-Handlern oder Template-Literals reflektiert  

### Sind moderne Browser-Sicherheits-Header vorhanden, um die Auswirkungen von XSS zu minimieren?
- [ ] Ja — `Content-Security-Policy` (CSP) ist mit einer restriktiven script-src **aktiviert**  
- [ ] Ja — `Content-Security-Policy` (CSP) ist **aktiviert**, enthält jedoch `unsafe-inline` oder schwache Whitelists  
- [ ] Nein — es sind weder CSP- noch veraltete `X-XSS-Protection`-Header vorhanden  

---