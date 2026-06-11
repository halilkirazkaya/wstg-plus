## WSTG-INFO-08 — Fingerprint Web Application Framework

Das Fingerprinting eines Web-Application-Frameworks umfasst die Identifizierung des spezifischen Technologie-Stacks und der Softwareversionen, die zum Erstellen und Bereitstellen der Anwendung verwendet werden. Angreifer analysieren HTTP-Response-Header, framework-spezifische Cookies, vorhersehbare Dateistrukturen und eindeutige JavaScript-Artefakte, um festzustellen, ob das Ziel auf Plattformen wie React, Angular, Django oder Spring läuft. Diese Reconnaissance-Phase ist entscheidend, da sie es einem Tester ermöglicht, die Angriffsfläche auf bekannte Schwachstellen (CVEs) und häufige framework-spezifische Konfigurationsfehlern einzugrenzen. Durch das Verständnis der zugrunde liegenden Architektur kann ein Angreifer von generischen Tests zu hochgradig gezielten Exploitation-Strategien übergehen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Informativ (Informational) |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Geben HTTP-Response-Header den Namen oder die Version des Frameworks preis?
- [ ] Nein — Header wie `X-Powered-By` oder `Server` sind **nicht** vorhanden oder wurden bereinigt (sanitized)  
- [ ] Ja — Der Name des Frameworks ist vorhanden, aber die Version **ist verborgen**  
- [ ] Ja — Sowohl der Name des Frameworks als auch die Version **werden offengelegt**  

### Sind framework-spezifische Cookies oder Verzeichnisstrukturen identifizierbar?
- [ ] Nein — Gängige Framework-Artefakte (z. B. `.js`-Pfade, Cookie-Namen) sind verschleiert (obfuscated) oder umbenannt  
- [ ] Ja — Framework-spezifische Cookies (z. B. `JSESSIONID`, `PHPSESSID`, `csrftoken`) **sind** identifizierbar  
- [ ] Ja — Verzeichnisstrukturen (z. B. `/wp-content/`, `_next/static/`) **sind** sichtbar und bestätigen das Framework  

### Geben Fehlermeldungen oder Quellcode-Kommentare Details zum Framework preis?
- [ ] Nein — Die Fehlerbehandlung ist generisch und Kommentare wurden entfernt  
- [ ] Ja — Framework-spezifische Stack-Traces **sind aktiviert** und in den Antworten sichtbar  
- [ ] Ja — Der HTML-Quellcode enthält Kommentare oder Metadaten-Tags, die das Framework identifizieren  

### Gibt es bekannte Schwachstellen in Verbindung mit der identifizierten Framework-Version?
- [ ] Nein — Das identifizierte Framework ist aktuell und weist **keine bekannten** öffentlichen Schwachstellen auf  
- [ ] Ja — Die Framework-Version ist veraltet und spezifische CVEs **können** identifiziert werden  

---