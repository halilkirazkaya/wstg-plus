## WSTG-CLNT-11 — Test Web Messaging

Web Messaging, oder `postMessage`, ermöglicht die Cross-Origin-Kommunikation zwischen Window-Objekten, wie z. B. einer Parent-Seite und einem erzeugten Pop-up oder einem eingebetteten iframe. Dieser Mechanismus ist für moderne Web-Funktionalitäten entscheidend, birgt jedoch erhebliche Risiken, wenn die empfangende Anwendung die Identität des Absenders nicht verifiziert oder den Message-Payload unsachgemäß verarbeitet. Angreifer nutzen diese Schwachstellen aus, indem sie eine bösartige Website hosten, welche die Zielanwendung einbettet und manipulierte Nachrichten sendet, um unbeabsichtigte Aktionen auszulösen, sensible Daten zu exfiltrieren oder DOM-based Cross-Site Scripting (XSS) auszuführen. Eine erfolgreiche Ausnutzung erfolgt, wenn Message-Listener die `origin`-Eigenschaft nicht strikt validieren oder `message.data` direkt an gefährliche Execution Sinks übergeben.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Werkzeuge:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### Nutzt die Anwendung die `postMessage` API für die dokumentübergreifende Kommunikation?
- [ ] Nein — `postMessage` Listener sind im clientseitigen Code **nicht** vorhanden  
- [ ] Ja — `postMessage` wird für die interne Kommunikation zwischen Komponenten verwendet  
- [ ] Ja — `postMessage` wird verwendet, um mit Drittanbieter-Domains oder Widgets zu kommunizieren  

### Wird die Origin eingehender Nachrichten strikt validiert?
- [ ] Ja — die Origin wird gegen eine strikte Whitelist mittels `===` oder identischem Vergleich geprüft *(Am sichersten)*  
- [ ] Ja — die Origin wird mittels Regex validiert, aber die Logik **ist** anfällig für einen Bypass (z. B. `^https://trusted.com`)  
- [ ] Nein — die `origin`-Eigenschaft wird **nicht** geprüft, was jeder Domain das Senden von Nachrichten erlaubt  
- [ ] Nein — ein Wildcard `*` wird in der `targetOrigin` des Absenders verwendet, was potenziell Daten an bösartige Listener abfließen lässt  

### Wird der Message-Payload bereinigt (sanitized), bevor er von der Anwendung verarbeitet wird?
- [ ] Ja — Daten werden als Klartext behandelt und es werden keine gefährlichen Sinks verwendet  
- [ ] Ja — Daten werden geparst (z. B. `JSON.parse`) und vor der Verwendung validiert, ein Bypass ist **nicht möglich**  
- [ ] Nein — Message-Daten werden direkt an gefährliche Sinks wie `innerHTML`, `eval()` oder `setTimeout()` übergeben  
- [ ] Nein — Message-Daten werden verwendet, um im Fenster zu navigieren oder die `location.href` ohne Validierung zu ändern  

### Kann ein Angreifer durch eine bösartige Nachricht unbefugte Aktionen auslösen oder Daten exfiltrieren?
- [ ] Nein — selbst mit einer gefälschten Nachricht können keine sensiblen Aktionen ausgelöst oder Daten abgerufen werden  
- [ ] Ja — eine manipulierte Nachricht **kann** sensible Zustandsänderungen auslösen (z. B. Passwortänderung, Profilaktualisierung)  
- [ ] Ja — eine manipulierte Nachricht **kann** zu DOM-based XSS oder zur Exfiltration von CSRF-Tokens/Sitzungsdaten führen  

---