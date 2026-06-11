## WSTG-CONF-04 — Überprüfung alter Backup-Dateien und nicht referenzierter Dateien auf sensible Informationen

Die Überprüfung alter Backup-Dateien und nicht referenzierter Dateien umfasst die Identifizierung vergessener, temporärer oder versteckter Dateien auf einem Webserver, die nicht für den öffentlichen Zugriff bestimmt sind. Diese Dateien enthalten häufig Quellcode-Backups (`.zip`, `.bak`), Texteditor-Swap-Dateien (`.swp`, `~`) oder Versionskontroll-Metadaten (`.git`, `.svn`), die sensible Informationen preisgeben können. Angreifer verwenden automatisierte Wordlists und Discovery-Tools, um gängige Namenskonventionen per Brute-Force anzugreifen, mit dem Ziel, Datenbank-Credentials, fest codierte API-Keys oder Logiken zu exfiltrieren, die weitere Angriffe (Exploitation) erleichtern. Diese Schwachstelle resultiert typischerweise aus manueller Serverwartung oder fehlerhaften CI/CD-Pipelines, bei denen die Bereinigung des Production Web-Root fehlschlägt.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel / Hoch* |

> *Die Kritikalität wird als „Hoch“ eingestuft, wenn Konfigurationsdateien, Quellcode-Archive oder Credentials entdeckt werden.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Ist das Directory Listing auf dem Webserver aktiviert?
- [ ] Nein — Directory Listing ist **deaktiviert** und gibt einen 403 Forbidden oder einen benutzerdefinierten Fehler zurück  
- [ ] Ja — Directory Listing ist in nicht sensiblen Verzeichnissen **aktiviert**  
- [ ] Ja — Directory Listing ist in Verzeichnissen mit Quellcode oder sensiblen Dateien **aktiviert** *(Kritisch)*  

### Sind Backup-Dateien mit gängigen Erweiterungen (z. B. .bak, .old, .save) auffindbar?
- [ ] Nein — gängige Backup-Erweiterungen wurden **nicht gefunden** oder werden durch die Serverkonfiguration blockiert  
- [ ] Ja — Backup-Dateien existieren, enthalten aber **keine** sensiblen Informationen  
- [ ] Ja — Backup-Dateien für Konfigurationen oder Quellcode **sind** zugänglich *(Hoch)*  

### Sind Versionskontrollverzeichnisse (z. B. .git, .svn) oder Metadaten exponiert?
- [ ] Nein — Versionskontroll-Metadaten sind **nicht vorhanden** oder korrekt blockiert  
- [ ] Ja — Metadaten existieren, aber das vollständige Repository **kann nicht** rekonstruiert werden  
- [ ] Ja — das gesamte Quellcode-Repository **kann** über exponierte Metadaten exfiltriert werden  

### Befinden sich komprimierte Archive (z. B. .zip, .tar.gz, .rar) der Anwendung im Web-Root?
- [ ] Nein — es wurden keine sensiblen Archivdateien durch Brute-Force oder Enumeration entdeckt  
- [ ] Ja — Archive wurden gefunden, sind aber passwortgeschützt oder enthalten öffentliche Assets  
- [ ] Ja — unverschlüsselte Archive mit Anwendungsquellcode oder Daten **sind** öffentlich zugänglich  

### Gibt die Anwendung temporäre Dateien preis, die von Texteditoren oder IDEs erstellt wurden?
- [ ] Nein — temporäre Dateien wie `.swp`, `~` oder `.DS_Store` sind **nicht vorhanden**  
- [ ] Ja — nicht sensible temporäre Dateien sind **vorhanden**  
- [ ] Ja — temporäre Dateien legen Quellcode-Ausschnitte oder interne Pfade offen  

---