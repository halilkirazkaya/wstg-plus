## WSTG-CLNT-01 — Testen auf DOM-basiertes Cross Site Scripting

DOM-basiertes Cross-Site Scripting (DOM XSS) tritt auf, wenn eine Anwendung clientseitiges JavaScript enthält, das Daten aus einer nicht vertrauenswürdigen Quelle auf unsichere Weise verarbeitet und diese Daten meist über einen gefährlichen Sink (Senke) zurück in das Document Object Model (DOM) schreibt. Im Gegensatz zu reflektiertem oder gespeichertem XSS existiert die Sicherheitslücke vollständig im clientseitigen Code; der Payload wird oft gar nicht an den Server gesendet, da er von Sinks wie `innerHTML`, `eval()` oder `document.write()` verarbeitet wird. Angreifer nutzen dies aus, indem sie URLs mit bösartigen Fragmenten oder Parametern präparieren, die beim Laden im Browser des Opfers beliebiges JavaScript im Kontext der Benutzersitzung ausführen. Dies kann zur Exfiltration von Session-Token, zum Diebstahl sensibler Daten und zu unbefugten Aktionen im Namen des authentifizierten Benutzers führen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Werden nicht vertrauenswürdige Quellen verwendet, um benutzergesteuerte Daten in JavaScript zu erfassen?
- [ ] Nein — JavaScript verwendet **kein** `location.hash`, `location.search` oder `document.referrer`  
- [ ] Ja — nicht vertrauenswürdige Quellen werden verwendet, aber die Daten werden **nicht** an Execution Sinks übergeben  
- [ ] Ja — nicht vertrauenswürdige Quellen werden verwendet und die Daten fließen direkt in die clientseitige Logik  

### Werden benutzergesteuerte Daten an gefährliche DOM-Sinks übergeben?
- [ ] Nein — Sinks wie `innerHTML`, `outerHTML`, `document.write()` oder `eval()` werden **nicht** verwendet  
- [ ] Ja — gefährliche Sinks sind vorhanden, aber die Daten werden mit einer sicheren Bibliothek wie DOMPurify strikt **bereinigt** (Sanitization)  
- [ ] Ja — gefährliche Sinks sind vorhanden und die Daten werden mit **unzureichender** oder benutzerdefinierter Regex-basierter Filterung verarbeitet  
- [ ] Ja — gefährliche Sinks sind vorhanden und die Daten werden **ohne** jegliche Bereinigung oder Kodierung übergeben *(Kritisch)*  

### Kann der Execution Sink über einen URL-basierten Payload erreicht werden?
- [ ] Nein — der Source-to-Sink-Datenfluss **kann nicht** über externe Eingaben ausgelöst werden  
- [ ] Ja — der Datenfluss ist **möglich**, erfordert jedoch eine komplexe Benutzerinteraktion  
- [ ] Ja — der Datenfluss ist **möglich** und kann über ein einfaches URL-Fragment oder einen Query-Parameter ausgelöst werden  

### Neutralisieren moderne Sicherheits-Header oder browserbasierte Abwehrmaßnahmen das Risiko?
- [ ] Ja — eine strikte `Content-Security-Policy` (CSP) mit `script-src` und `trusted-types` ist **aktiviert** und verhindert die Ausführung  
- [ ] Ja — eine CSP ist vorhanden, aber `unsafe-inline` oder schwache Hashes machen einen Bypass **möglich**  
- [ ] Nein — es ist keine CSP vorhanden, um die DOM-basierte Ausführung zu unterbinden  

### Verwendet die Anwendung clientseitige Frameworks mit integrierten Schutzmechanismen?
- [ ] Ja — das Framework (z. B. Angular, React) kodiert Daten automatisch und ein Bypass ist **nicht möglich**  
- [ ] Ja — ein Framework wird verwendet, aber „Escape Hatches“ wie `dangerouslySetInnerHTML` sind **aktiviert**  
- [ ] Nein — die Anwendung verwendet reines JavaScript (Vanilla JS) oder veraltete Bibliotheken (z. B. alte jQuery-Versionen) ohne automatische Maskierung (Auto-Escaping)  

---