## WSTG-CONF-09 — Berechtigungen von Dateien prüfen

Die Überprüfung von Dateiberechtigungen umfasst die Auditierung der Zugriffskontrollebenen, die Dateien und Verzeichnissen auf dem Webserver zugewiesen sind, um sicherzustellen, dass sensible Ressourcen nicht unbefugten Benutzern oder Prozessen gegenüber exponiert werden. Falsch konfigurierte Berechtigungen können es Angreifern ermöglichen, sensible Konfigurationsdateien mit Datenbank-Anmeldedaten (Credentials) zu lesen, proprietären Quellcode einzusehen oder serverseitige Skripte zu modifizieren, um eine Remote Code Execution zu erreichen. Diese Schwachstelle tritt typischerweise während der Deployment-Phase auf, wenn standardmäßige "world-readable"- oder "world-writable"-Flags auf kritischen Anwendungsverzeichnissen wie Konfigurationspfaden, Log-Ordnern oder Upload-Verzeichnissen belassen werden. Aus der Sicht eines Angreifers dient das Entdecken einer unzureichend gesicherten Datei oft als primärer Katalysator für eine Privilege Escalation oder eine vollständige Systemkompromittierung.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel / Hoch* |

> *Die Kritikalität wird als "Hoch" eingestuft, wenn sensible Konfigurationsdateien (z. B. `.env`, `web.config`) lesbar sind oder wenn Schreibzugriff auf das Web-Root möglich ist.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Werkzeuge:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Sind sensible Konfigurationsdateien der Anwendung vor unbefugtem Zugriff geschützt?
- [ ] Ja — Konfigurationsdateien (z. B. `.env`, `config.php`) sind **nicht** über Web-Anfragen zugänglich  
- [ ] Ja — Dateien sind vorhanden, aber der Zugriff ist **nur** auf autorisierte lokale Benutzer beschränkt  
- [ ] Nein — sensible Konfigurationsdateien **sind zugänglich** und legen Credentials oder Geheimnisse offen  

### Kann ein Angreifer Dateien oder Verzeichnisse innerhalb des Web-Roots modifizieren?
- [ ] Nein — alle Web-Root-Verzeichnisse sind für den Webserver-Benutzer **schreibgeschützt** (read-only)  
- [ ] Ja — beschreibbare Verzeichnisse existieren, aber die Skriptausführung ist **deaktiviert**  
- [ ] Ja — "world-writable" Verzeichnisse existieren und **ermöglichen** das Hochladen und Ausführen von Skripten *(Kritisch)*  

### Sind Metadaten der Versionsverwaltung oder Backup-Dateien aufgrund laxer Berechtigungen exponiert?
- [ ] Nein — `.git`, `.svn` und Backup-Dateien (z. B. `.bak`, `~`) sind **nicht vorhanden** oder geschützt  
- [ ] Ja — Metadaten-Verzeichnisse existieren, aber das Directory Listing ist **deaktiviert**  
- [ ] Ja — sensible Metadaten oder Backups sind **vollständig zugänglich**, was die Wiederherstellung des Quellcodes ermöglicht  

### Agiert der Webserver-Benutzer nach dem Prinzip der minimalen Berechtigung (Principle of Least Privilege)?
- [ ] Ja — der Webserver-Prozess läuft als ein **dedizierter Benutzer mit geringen Berechtigungen** (low-privilege)  
- [ ] Nein — der Webserver-Prozess läuft mit **unnötigen Berechtigungen** (z. B. `root` oder `Administrator`)  

### Sind Log-Dateien gegen unbefugtes Lesen oder Modifizieren gesichert?
- [ ] Ja — Log-Dateien sind auf administrative Benutzer **beschränkt**  
- [ ] Ja — Log-Dateien sind lesbar, enthalten aber **keine** Session-Tokens oder PII  
- [ ] Nein — Log-Dateien sind **öffentlich lesbar** und legen sensible Transaktionsdaten oder Benutzerinformationen offen  

---