## WSTG-CONF-03 — Überprüfung der Dateierweiterungsverarbeitung auf sensible Informationen

Die Überprüfung der Verarbeitung von Dateierweiterungen beinhaltet die Identifizierung, ob der Webserver oder Applikationsserver sensible Informationen preisgibt, indem er Dateien ausliefert, die eingeschränkt oder ausgeführt werden sollten. Angreifer suchen häufig nach Backup-Dateien, Konfigurationsdateien und Quellcode-Fragmenten (z. B. `.bak`, `.old`, `.env`, `.inc`), die im Web-Root verbleiben und aufgrund fehlender spezifischer Handler Mappings als Klartext ausgeliefert werden könnten. Diese Schwachstelle ist von Bedeutung, da sie zur Offenlegung von Datenbank-Anmeldedaten, API-Keys und interner Geschäftslogik führen kann, was eine tiefergehende Exploitation der Infrastruktur erleichtert. Diese Expositionen treten typischerweise in öffentlichen Verzeichnissen, Upload-Ordnern oder durch falsch konfigurierte MIME-Type-Einstellungen auf dem Server auf.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn Konfigurationsdateien mit Anmeldedaten oder der vollständige Quellcode zugänglich sind.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Sind sensible Backup- oder temporäre Dateierweiterungen (z. B. `.bak`, `.old`, `.swp`, `~`) zugänglich?
- [ ] Nein — Der Server gibt 403 Forbidden oder 404 Not Found für gängige Backup-Erweiterungen zurück  
- [ ] Ja — Der Server erlaubt das Herunterladen von Backup-Dateien, diese enthalten jedoch **keine** sensiblen Daten  
- [ ] Ja — Der Server erlaubt das Herunterladen von Backup-Dateien, die sensible Daten oder Quellcode enthalten *(Hoch)*  

### Gibt der Server Umgebungs- oder Systemkonfigurationsdateien preis (z. B. `.env`, `.config`, `.yml`, `.ini`)?
- [ ] Nein — Auf eingeschränkte Konfigurationsdateien **kann nicht** zugegriffen werden; es wird 403/404 zurückgegeben  
- [ ] Ja — Sensible Konfigurationsdateien **sind** zugänglich und legen interne Geheimnisse oder Anmeldedaten offen  

### Kann Quellcode durch das Anhängen alternativer Erweiterungen abgerufen werden (z. B. `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] Nein — Applikationscode wird **nicht** als Klartext gerendert, unabhängig von Manipulationen der Erweiterung  
- [ ] Ja — Quellcode wird offengelegt, da der Server **keinen** Handler für die angehängte Erweiterung besitzt  

### Sind administrative Verzeichnisse oder Metadaten-Verzeichnisse (z. B. `.git/`, `.svn/`, `.DS_Store`) vor öffentlichem Zugriff geschützt?
- [ ] Nein — Diese Verzeichnisse/Dateien existieren **nicht** im Web-Root  
- [ ] Ja — Ein Zugriff ist aufgrund von serverseitigen Rewrite-Rules oder Berechtigungen **nicht möglich**  
- [ ] Ja — Metadaten-Verzeichnisse **sind** zugänglich und ermöglichen eine vollständige Rekonstruktion des Quellcodes  

### Wie verarbeitet der Server Anfragen für Dateien mit mehreren Erweiterungen (z. B. `file.php.jpg`)?
- [ ] Nein — Der Server priorisiert die Sicherheit der internen Erweiterung korrekt oder blockiert die Anfrage  
- [ ] Ja — Der Server verarbeitet die Datei gemäß der ersten Erweiterung, aber ein Bypass zur Ausführung ist **nicht möglich**  
- [ ] Ja — Der Server ignoriert die letzte Erweiterung, wodurch Code Execution oder Source Disclosure **möglich ist**  

---