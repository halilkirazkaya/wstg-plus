## WSTG-BUSL-09 — Test des Uploads bösartiger Dateien

Die Datei-Upload-Funktionalität ermöglicht es Benutzern, Daten an den Server zu übermitteln, was einen direkten Vektor für das Einschleusen bösartiger Inhalte in die Umgebung der Anwendung darstellt. Angreifer nutzen schwache Validierungslogik aus, um Web Shells, Malware oder speziell präparierte Dateien – wie SVG oder HTML – hochzuladen, um Remote Code Execution (RCE) oder Cross-Site Scripting (XSS) zu erreichen. Diese Schwachstelle findet sich typischerweise in Funktionen wie Profileinstellungen, Dokumentenmanagementsystemen und Attachment-Handlern, bei denen der Server Dateitypen, Inhalte und Ausführungsberechtigungen nicht ordnungsgemäß überprüft. Eine erfolgreiche Ausnutzung führt häufig zur vollständigen Kompromittierung des Systems, zum Datenabfluss (Data Exfiltration) oder zur Nutzung des Servers als Pivot-Punkt für Angriffe auf das interne Netzwerk.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### Bietet die Anwendung Funktionen zum Hochladen von Dateien?
- [ ] Nein — es existiert keine Datei-Upload-Funktionalität  
- [ ] Ja — Funktionalität existiert für authentifizierte Benutzer  
- [ ] Ja — Funktionalität ist für **nicht authentifizierte** Benutzer verfügbar *(Kritisch)*  

### Wird die Validierung der Dateiendung serverseitig erzwungen?
- [ ] Ja — eine strikte Allowlist von Endungen wird **angewendet** und ein Bypass ist **nicht möglich**  
- [ ] Ja — eine Allowlist wird verwendet, aber ein Bypass **ist möglich** über Groß-/Kleinschreibung oder doppelte Endungen  
- [ ] Ja — eine Blocklist wird verwendet, welche mit alternativen Endungen (z. B. `.phtml`, `.asa`) umgangen werden **kann**  
- [ ] Nein — es wird keine Validierung der Dateiendung auf der Serverseite **angewendet**  

### Wird der Dateiinhalt über die Dateiendung oder den MIME-Typ hinaus validiert?
- [ ] Ja — die Anwendung überprüft Magic Bytes und führt eine Tiefenprüfung des Inhalts (Deep Content Inspection) durch  
- [ ] Ja — die Anwendung prüft nur den `Content-Type`-Header, welcher leicht manipuliert (**spoofed**) werden kann  
- [ ] Nein — die Anwendung verlässt sich bei der Validierung vollständig auf die Dateiendung  

### Werden hochgeladene Dateien in einem Verzeichnis mit Ausführungsberechtigungen gespeichert?
- [ ] Nein — Dateien werden auf einem dedizierten Speicherdienst (z. B. S3) oder in einem nicht ausführbaren Verzeichnis gespeichert  
- [ ] Ja — Dateien werden auf dem Webserver gespeichert, aber die Ausführung ist über die Konfiguration **deaktiviert**  
- [ ] Ja — Dateien werden in einem web-zugänglichen Verzeichnis gespeichert und die Ausführung **ist aktiviert** *(Kritisch)*  

### Können Sicherheitsfilter durch Manipulation des Dateinamens umgangen werden?
- [ ] Nein — Dateinamen werden vom Server bereinigt (sanitized) oder zufällig neu generiert  
- [ ] Ja — Filter **können** unter Verwendung von Null-Bytes (z. B. `shell.php%00.jpg`) umgangen werden  
- [ ] Ja — Path Traversal-Zeichen (z. B. `../../`) **können** verwendet werden, um Dateien in beliebige Verzeichnisse hochzuladen  

---