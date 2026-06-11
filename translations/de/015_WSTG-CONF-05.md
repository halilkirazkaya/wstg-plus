## WSTG-CONF-05 — Aufzählung von Administrationsschnittstellen für Infrastruktur und Anwendungen

Administrationsschnittstellen stellen die Verwaltungsebene einer Anwendung und der zugrunde liegenden Infrastruktur dar und gewähren häufig privilegierten Zugriff auf Systemkonfigurationen, Benutzerdaten und serverseitige Vorgänge. Angreifer priorisieren die Aufzählung dieser Schnittstellen durch Directory Brute-Forcing, die Analyse der `robots.txt` oder das Beobachten vorhersehbarer URL-Muster, um Einstiegspunkte zu finden, denen es an robuster Authentifizierung mangelt oder die auf Standard-Zugangsdaten (Default Credentials) basieren. Die Entdeckung eines exponierten Admin-Panels erhöht das Risiko einer vollständigen Systemkompromittierung, unbefugter Datenmanipulation oder Dienstunterbrechung erheblich. Diese Schnittstellen befinden sich häufig auf Nicht-Standard-Ports oder Subdomains, was sie zu primären Zielen für automatisierte Scanner und manuelle Rekonstruktion (Reconnaissance) macht.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn die Schnittstelle systemweite Konfigurationsänderungen, Benutzerverwaltung oder Datenexfiltration ohne Multi-Faktor-Authentifizierung (MFA) ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Sind Administrationsschnittstellen durch Directory Fuzzing oder Rekonstruktion auffindbar?
- [ ] Nein — es sind keine Administrationsschnittstellen exponiert oder auffindbar  
- [ ] Ja — Schnittstellen sind auffindbar, aber der Zugriff **ist auf interne IP-Bereiche beschränkt**  
- [ ] Ja — Schnittstellen **sind** auffindbar und öffentlich zugänglich  

### Wird die Authentifizierung an den entdeckten Administrationsportalen erzwungen?
- [ ] Ja — Multi-Faktor-Authentifizierung (MFA) **wird erzwungen**  
- [ ] Ja — Ein-Faktor-Authentifizierung (SFA) **wird erzwungen**  
- [ ] Nein — für den Zugriff auf die Schnittstelle ist keine Authentifizierung erforderlich *(Kritisch)*  

### Sind Administrationspfade versteckt oder verwenden sie Nicht-Standard-Bezeichnungen, um die Entdeckung zu verhindern?
- [ ] Ja — Pfade verwenden Nicht-Standard-Bezeichnungen, zufällige oder nicht vorhersehbare Namen  
- [ ] Nein — Pfade verwenden gängige Namenskonventionen (z. B. `/admin`, `/manager`, `/console`) und **können** leicht erraten werden  

### Bietet die Schnittstelle Zugriff auf sensible System- oder Anwendungsfunktionen?
- [ ] Nein — die Schnittstelle ist ein „schreibgeschütztes“ (Read-only) Status-Dashboard mit minimalen Auswirkungen  
- [ ] Ja — die Schnittstelle **kann** die Anwendungskonfiguration oder Benutzerdaten ändern  
- [ ] Ja — die Schnittstelle **kann** Befehle auf Systemebene ausführen oder die Datenbankverwaltung übernehmen  

---