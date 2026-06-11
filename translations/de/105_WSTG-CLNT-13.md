## WSTG-CLNT-13 — Prüfung auf Cross-Site Script Inclusion

Cross-Site Script Inclusion (XSSI) tritt auf, wenn eine Webanwendung sensible Daten innerhalb einer dynamisch generierten JavaScript-Datei oder einer JSONP-Antwort exportiert, welche eine vom Angreifer kontrollierte Website anschließend über ein `<script>`-Tag einbinden kann. Da die Einbindung von Skripten die Same-Origin Policy (SOP) umgeht, kann ein Angreifer das Skript in seinem eigenen Kontext ausführen und globale Objekte oder Variablen überschreiben, um die sensiblen Informationen zu exfiltrieren. Diese Schwachstelle manifestiert sich typischerweise in authentifizierten Endpunkten, die benutzerspezifische Daten wie E-Mail-Adressen, API-Schlüssel oder Sitzungsmetadaten im Skriptformat zurückgeben. Aus der Perspektive eines Angreifers beinhaltet die Ausnutzung das Erstellen einer bösartigen Seite, die auf das verwundbare Skript verweist und die resultierenden Änderungen an globalen Variablen oder Funktionsaufrufe erfasst.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch |

> *Der Schweregrad wird als Hoch eingestuft, wenn die geleakten Daten CSRF-Token, Sitzungsidentifikatoren oder hochsensible PII (Personally Identifiable Information) enthalten.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Geben authentifizierte Endpunkte dynamisches JavaScript oder JSONP zurück, das sensible Informationen enthält?
- [ ] Nein — alle Skripte sind statisch und können **keine** benutzerspezifischen Daten enthalten  
- [ ] Ja — dynamische Skripte existieren, enthalten aber **keine** sensiblen Informationen  
- [ ] Ja — dynamische Skripte enthalten PII oder Token und **sind** herkunftsübergreifend (cross-origin) zugänglich  

### Ist der Header `X-Content-Type-Options: nosniff` bei Antworten mit sensiblen Skripten implementiert?
- [ ] Ja — der Header wird **angewendet** und verhindert MIME-Type-Sniffing  
- [ ] Nein — der Header **fehlt**, was potenziell dazu führen kann, dass Nicht-Skript-Inhalte als Skript ausgeführt werden  

### Sind sensible Skripte durch Anti-XSSI-Mechanismen wie nicht ausführbare Präfixe oder benutzerdefinierte Header geschützt?
- [ ] Ja — Skripte erfordern einen benutzerdefinierten Header oder ein Token, das **nicht** über ein standardmäßiges `<script>`-Tag gesendet werden kann  
- [ ] Ja — Skripte verwenden nicht ausführbare Präfixe (z. B. `)]}'`), die **nicht** einfach umgangen werden können  
- [ ] Nein — Skripte sind direkt über GET-Anfragen unter Verwendung von Umgebungscredentials (Ambient Credentials) zugänglich *(Kritisch)*  

### Ist die Exfiltration sensibler Daten durch das Überschreiben globaler Variablen oder Prototypen möglich?
- [ ] Nein — sensible Daten sind lokal begrenzt (scoped) oder durch moderne Browser-Funktionen geschützt  
- [ ] Ja — sensible Daten werden globalen Variablen zugewiesen und **können** erfasst werden  
- [ ] Ja — Daten werden über JSONP-Callbacks geleakt, die abgefangen werden **können**  

---