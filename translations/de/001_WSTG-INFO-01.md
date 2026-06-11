## WSTG-INFO-01 — Durchführung von Suchmaschinen-Discovery und Reconnaissance auf Informationslecks

Die Entdeckung über Suchmaschinen (Search Engine Discovery) umfasst die Nutzung öffentlicher Suchmaschinen, Cache-Seiten und Indizierungsdiensten, um Informationen zu identifizieren, die die Zielorganisation unbeabsichtigt offengelegt hat. Angreifer verwenden fortgeschrittene Suchoperatoren (Google Dorks) und Indizierungsdienste von Drittanbietern, um sensible Dateien, Verzeichnisse, Login-Portale, Fehlermeldungen und Metadaten zu entdecken, die nicht öffentlich zugänglich sein sollten. Diese Reconnaissance-Phase ist kritisch, da sie keine direkte Interaktion mit dem Ziel erfordert, was sie praktisch unentdeckbar macht, während sie potenziell hochwertige Einstiegspunkte wie exponierte Administrationspanels, Konfigurationsdateien, Datenbank-Dumps und interne Dokumentationen offenbart.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Werden sensible Dateien oder Verzeichnisse von Suchmaschinen indiziert?
- [ ] Nein — keine sensiblen Inhalte in den Suchmaschinenergebnissen gefunden  
- [ ] Ja — Konfigurationsdateien, Backups oder interne Dokumente **sind** indiziert *(Kritisch)*  

### Offenbart Google Dorking exponierte Administrations- oder Login-Schnittstellen?
- [ ] Nein — Admin-Panels und Login-Seiten sind **nicht** indiziert  
- [ ] Ja — Admin-Panels **sind** indiziert, erfordern jedoch eine starke Multi-Faktor-Authentifizierung  
- [ ] Ja — Admin-Panels **sind** indiziert und ohne ausreichende Kontrollen öffentlich zugänglich  

### Werden in Cache-Versionen von Seiten sensible Daten offengelegt, die inzwischen entfernt wurden?
- [ ] Nein — keine sensiblen Daten in Cache-Snapshots gefunden  
- [ ] Ja — Cache-Seiten enthalten Anmeldedaten, interne IP-Adressen oder sensible Informationen  

### Gibt die Datei `robots.txt` unbeabsichtigt sensible Pfade preis?
- [ ] Nein — `robots.txt` offenbart keine sensiblen Verzeichnisstrukturen  
- [ ] Ja — `robots.txt` listet sensible Pfade auf, die die Reconnaissance eines Angreifers unterstützen  
- [ ] Es existiert keine `robots.txt`-Datei  

### Offenbaren Dienste von Drittanbietern (Shodan, Censys, Wayback Machine) historische oder aktuelle Expositionen?
- [ ] Nein — keine signifikanten Funde durch Indizierungsdienste von Drittanbietern  
- [ ] Ja — historische Snapshots oder Dienst-Scans offenbaren sensible Metadaten oder Service-Banner  

---