## WSTG-INPV-15 — HTTP Splitting Smuggling

HTTP Splitting und Smuggling Schwachstellen entstehen durch Diskrepanzen in der Art und Weise, wie Frontend-Proxies und Backend-Server HTTP-Anfragegrenzen interpretieren und verarbeiten, insbesondere in Bezug auf `Content-Length` und `Transfer-Encoding` Header. Durch das Erstellen mehrdeutiger Anfragen kann ein Angreifer eine versteckte Anfrage an das Backend „schmuggeln“ oder CRLF-Sequenzen injizieren, um eine Antwort aufzuteilen, was zu Cache Poisoning, Request Hijacking oder dem Umgehen von Sicherheitskontrollen führt. Diese Fehler treten typischerweise in komplexen Umgebungen auf, die Reverse Proxies, Load Balancer oder CDNs verwenden, welche eine inkonsistente Parsing-Logik aufweisen. Aus der Sicht eines Angreifers ermöglicht dies die Umleitung des Benutzerverkehrs, den Diebstahl sensibler Session-Token und die Ausführung unbefugter Aktionen im Kontext der Sitzungen anderer Benutzer.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Werkzeuge:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Ist die Umgebung anfällig für Request Smuggling über CL.TE- oder TE.CL-Diskrepanzen?
- [ ] Nein — Frontend- und Backend-Server verarbeiten Anfragegrenzen konsistent  
- [ ] Ja — Diskrepanzen existieren, aber eine Ausnutzung ist aufgrund von Infrastruktur-Mitigationen **nicht möglich**  
- [ ] Ja — CL.TE oder TE.CL Smuggling **ist möglich**, wodurch versteckte Anfragen das Backend erreichen können *(Hoch)*  
- [ ] Ja — TE.TE (Double Encoding/Obfuscation) **ist möglich**, um Frontend-Filter zu umgehen *(Kritisch)*  

### Kann HTTP Response Splitting durch CRLF-Injection in Headern erreicht werden?
- [ ] Nein — die Anwendung bereinigt CRLF-Sequenzen in allen Header-Eingaben ordnungsgemäß  
- [ ] Ja — CRLF-Sequenzen werden in Headern reflektiert, aber Response Splitting ist **nicht möglich**  
- [ ] Ja — CRLF-Injection **ist möglich**, was Header-Injection oder Cache Poisoning erlaubt  

### Werden Sicherheitskontrollen (WAF/ACLs) mithilfe von geschmuggelten Anfragen umgangen?
- [ ] Nein — Sicherheitskontrollen gelten sowohl für äußere als auch für geschmuggelte Anfragen  
- [ ] Ja — geschmuggelte Anfragen **können** Frontend-WAF-Regeln oder IP-basierte ACLs umgehen  

### Ist es möglich, Sitzungen anderer Benutzer zu übernehmen (Hijacking) oder deren Datenverkehr umzuleiten?
- [ ] Nein — Request/Response-Streams sind isoliert und können nicht gekreuzt werden  
- [ ] Ja — Request Smuggling **ist möglich** und erlaubt das Erfassen von Anfragen anderer Benutzer *(Kritisch)*  
- [ ] Ja — Response Splitting **ist möglich** und erlaubt Browser-seitiges Cache Poisoning oder XSS  

---