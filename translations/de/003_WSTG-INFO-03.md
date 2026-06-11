## WSTG-INFO-03 — Überprüfung von Webserver-Metadateien auf Information Leakage

Webserver-Metadateien wie `robots.txt`, `sitemap.xml` und `security.txt` dienen dazu, Crawler zu steuern und administrative Metadaten bereitzustellen. Häufig legen sie jedoch unbeabsichtigt sensible Verzeichnisstrukturen oder versteckte Endpoints offen. Angreifer analysieren diese Dateien, um die Angriffsfläche (Attack Surface) der Anwendung zu enumerieren. Dabei identifizieren sie "Disallow"-Anweisungen, die häufig auf nicht verlinkte Administrations-Panels, Backup-Verzeichnisse oder Entwicklungsumgebungen hinweisen. Neben Anweisungen für Suchmaschinen können Dateien im Verzeichnis `.well-known/` oder Dateien wie `humans.txt` Informationen über den Technologiestack, Entwicklerdaten oder organisatorische Sicherheitskontakte offenlegen. Eine systematische Überprüfung dieser Dateien ermöglicht es einem Tester, strukturelle und technische Einblicke zu gewinnen, ohne eine aggressive Brute-Force-Suche durchzuführen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Severity** | Niedrig / Informativ* |

> *Die Kritikalität erhöht sich auf "Mittel" (Medium), wenn Metadateien nicht authentifizierte Administrations-Schnittstellen oder sensible Konfigurations-Backups offenlegen.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Existiert die Datei `robots.txt` und legt sie sensible Verzeichnisse offen?
- [ ] Nein — `robots.txt` existiert **nicht**  
- [ ] Ja — `robots.txt` existiert, enthält aber nur standardmäßige öffentliche Pfade  
- [ ] Ja — `robots.txt` legt sensible Verzeichnispfade oder Administrations-Schnittstellen offen  
- [ ] Ja — `robots.txt` legt Zugangsdaten oder spezifische Softwareversionen in Kommentaren offen  

### Ist eine `sitemap.xml`-Datei vorhanden und offenbart sie eine versteckte Anwendungsstruktur?
- [ ] Nein — `sitemap.xml` existiert **nicht**  
- [ ] Ja — `sitemap.xml` ist vorhanden, listet aber nur öffentlich zugängliche URLs auf  
- [ ] Ja — `sitemap.xml` listet nicht verlinkte Endpoints oder rein interne Ressourcen auf, auf die zugegriffen werden **kann**  

### Legen Sicherheits- und organisationsbezogene Metadateien technische Details offen?
- [ ] Nein — keine Metadateien im Root-Verzeichnis oder unter `.well-known/` gefunden  
- [ ] Ja — `security.txt` oder `humans.txt` sind vorhanden, enthalten aber nur Standardinformationen  
- [ ] Ja — Metadateien legen interne Hostnamen, Entwickleridentitäten oder Details zum Technologiestack offen, die Social Engineering oder weitere Angriffe begünstigen **können**  

### Sind veraltete oder Framework-spezifische Metadateien vorhanden?
- [ ] Nein — keine Framework-spezifischen Dateien (z. B. `info.php`, `composer.json`, `package.json`) gefunden  
- [ ] Ja — Dateien wie `README.md`, `CHANGELOG.txt` oder `.DS_Store` sind vorhanden, wurden aber bereinigt  
- [ ] Ja — Framework- oder versionsspezifische Dateien sind vorhanden und **ermöglichen** ein präzises Technologie-Fingerprinting  

---