## WSTG-CONF-13 — Path Confusion

Path Confusion entsteht durch Diskrepanzen in der Art und Weise, wie verschiedene Web-Komponenten, wie zum Beispiel Reverse Proxies, Load Balancer und Backend-Applikationsserver, URL-Pfade parsen und interpretieren. Angreifer nutzen diese Inkonsistenzen aus, indem sie spezifische Zeichen wie Semikolons, kodierte Slashes oder Dot-Segmente injizieren, um Sicherheitsfilter zu täuschen und auf eingeschränkte Endpunkte oder statische Ressourcen zuzugreifen. Diese Schwachstelle kann zu kritischen Sicherheitsfehlern führen, einschließlich Authentication Bypass, unbefugtem Datenzugriff und Web Cache Poisoning, da das Front-End Sicherheitsregeln auf einen Pfad anwenden kann, den das Back-End anders interpretiert. Eine erfolgreiche Ausnutzung erfolgt in der Regel an der Architektur-Grenze, an der Request-Routing- und Normalisierungslogik innerhalb des Tech-Stacks divergieren.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Test Status** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn Path Confusion zu einem Bypass der Authentifizierung oder Zugriffskontrolle für sensible administrative Endpunkte führt.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Interpretieren verschiedene Architekturschichten Pfadtrennzeichen (z. B. `;`, `#`, `?`) konsistent?
- [ ] Ja — alle Schichten normalisieren und interpretieren Pfadtrennzeichen identisch  
- [ ] Nein — es existieren Inkonsistenzen, diese können jedoch **nicht** für den Zugriff auf eingeschränkte Ressourcen genutzt werden  
- [ ] Nein — Diskrepanzen ermöglichen es einem Angreifer, Front-End-Filter zu umgehen und die Back-End-Logik zu erreichen (**möglich**)  

### Können Zugriffskontrollen mittels Path Traversal Sequenzen oder kodierten Zeichen umgangen werden?
- [ ] Nein — die Normalisierung wird vor den Sicherheitsprüfungen konsistent angewendet  
- [ ] Ja — ein Bypass ist über Dot-Segment-Sequenzen (z. B. `/admin/..;/`) **möglich**  
- [ ] Ja — ein Bypass ist über URL-kodierte Zeichen (z. B. `%2f`, `%2e%2e%2f`) **möglich**  

### Ist die Anwendung anfällig für Web Cache Poisoning durch Path Confusion?
- [ ] Nein — die Caching-Logik wird nicht durch Pfad-Ambiguität oder zusätzliche Pfadinformationen beeinflusst  
- [ ] Ja — bösartige Inhalte **können** aufgrund von Mapping-Diskrepanzen für legitime Pfade zwischengespeichert werden  
- [ ] Ja — sensible Informationen **können** über `RCD` (Relative Path Overwrite) Techniken in öffentlichen Verzeichnissen zwischengespeichert werden  

### Verarbeitet der Server „zusätzliche Pfadinformationen“ (PathInfo) sicher?
- [ ] Nein — die Funktion ist **deaktiviert** oder nicht vorhanden  
- [ ] Ja — `PathInfo` wird verarbeitet und beeinträchtigt die Sicherheitsfilter **nicht**  
- [ ] Nein — `PathInfo` ermöglicht es einem Angreifer, Daten anzuhängen, welche die Routing-Logik der Anwendung korrumpieren  

---