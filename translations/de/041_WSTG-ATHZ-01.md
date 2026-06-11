## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Schwachstellen wie Directory Traversal und File Inclusion entstehen, wenn eine Anwendung benutzergesteuerte Eingaben zur Konstruktion von Pfaden zu Dateien oder Verzeichnissen verwendet, ohne eine ausreichende Validierung oder Bereinigung (Sanitization) durchzuführen. Angreifer nutzen diese Fehler aus, indem sie Sequenzen wie `../` injizieren, um außerhalb des vorgesehenen Verzeichnisses zu navigieren und potenziell auf sensible Systemdateien, Konfigurationsdaten oder den Quellcode der Anwendung zuzugreifen. In schwerwiegenderen Fällen, die Local File Inclusion (LFI) oder Remote File Inclusion (RFI) betreffen, kann ein Angreifer durch das Einbinden bösartiger Skripte oder die Nutzung von Log Poisoning und PHP-Wrappern eine Remote Code Execution erzielen. Diese Schwachstellen befinden sich typischerweise in Parametern, die für das dynamische Laden von Inhalten, Template-Engines oder Endpunkte zum Abrufen von Bildern verwendet werden, bei denen die serverseitige Logik Dateisystempfade verarbeitet.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Tools:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Akzeptieren Parameter Dateinamen oder Pfade für die serverseitige Verarbeitung?
- [ ] Nein — keine Parameter scheinen mit dem Dateisystem zu interagieren  
- [ ] Ja — Parameter existieren, verwenden jedoch eine strikte Allowlist für Datei-Identifikatoren  
- [ ] Ja — Parameter akzeptieren direkte Dateinamen oder Pfade  

### Werden Eingaben bereinigt, um Directory Traversal-Sequenzen zu verhindern?
- [ ] Ja — Eingaben werden gegen eine strikte Allowlist validiert und Traversal-Sequenzen sind **nicht möglich**  
- [ ] Ja — Eingaben werden durch Entfernen von `../`-Sequenzen bereinigt, aber ein rekursiver Bypass **ist möglich**  
- [ ] Nein — es erfolgt **keine Bereinigung oder Validierung** für pfadbezogene Eingaben  

### Ist es möglich, über Traversal-Sequenzen auf Dateien außerhalb des eingeschränkten Verzeichnisses zuzugreifen?
- [ ] Nein — die Anwendung oder das Betriebssystem verhindert den Zugriff auf Dateien außerhalb des definierten Bereichs  
- [ ] Ja — Zugriff auf Dateien innerhalb des Web-Roots **ist möglich**  
- [ ] Ja — Zugriff auf sensible Systemdateien (z. B. `/etc/passwd`, `C:\Windows\win.ini`) **ist möglich** *(Kritisch)*  

### Erlaubt der Server das Einbinden von Remote-URLs (RFI)?
- [ ] Nein — Remote File Inclusion ist auf Server-/Anwendungsebene **deaktiviert**  
- [ ] Ja — Remote-Dateien **können** eingebunden werden, aber eine Ausführung **ist nicht möglich**  
- [ ] Ja — Remote-Dateien **können** eingebunden und auf dem Server ausgeführt werden *(Kritisch)*  

### Können Filter durch Kodierung oder Sonderzeichen umgangen werden?
- [ ] Nein — Filter verarbeiten verschiedene Kodierungen und Null-Bytes effektiv  
- [ ] Ja — Bypass **ist möglich** unter Verwendung von URL-Encoding, Double URL-Encoding oder 16-Bit Unicode  
- [ ] Ja — Bypass **ist möglich** unter Verwendung von Null Byte Injection (`%00`) oder Filesystem-Wrappern (z. B. `php://filter`)  

---