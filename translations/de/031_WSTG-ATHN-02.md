## WSTG-ATHN-02 — Testen auf Standard-Zugangsdaten

Das Testen auf Standard-Zugangsdaten (Default Credentials) umfasst die Identifizierung von Administrations-Schnittstellen, Diensten oder Anwendungskomponenten, die noch werkseitig voreingestellte oder vom Hersteller bereitgestellte Benutzernamen und Passwörter verwenden. Diese Schwachstelle ist ein hochpriorisiertes Ziel für Angreifer, da sie oft einen direkten Pfad zur vollständigen Systemkompromittierung, administrativem Zugriff oder Remote Code Execution (RCE) mit minimalem Aufwand bietet. Dies tritt typischerweise bei Standardsoftware, CMS-Plattformen, Datenbank-Management-Tools und integrierter Hardware wie IoT-Geräten oder Netzwerkgeräten auf. Die Ausnutzung erfolgt in der Regel durch den Abgleich identifizierter Softwareversionen mit öffentlichen Standard-Passwortdatenbanken oder durch den Einsatz automatisierter Credential Stuffing Tools während der ersten Reconnaissance- und Exploit-Phasen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Kritisch / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Sind Administrations- oder Management-Schnittstellen im Internet oder ungesicherten Netzwerksegmenten exponiert?
- [ ] Nein — es sind keine Management-Schnittstellen zugänglich  
- [ ] Ja — Schnittstellen sind vorhanden, aber durch netzwerkseitige Kontrollen eingeschränkt (z. B. VPN, IP Allowlist)  
- [ ] Ja — Schnittstellen **sind** öffentlich zugänglich und erfordern eine Authentifizierung  

### Wurden die vom Hersteller bereitgestellten Standard-Zugangsdaten für alle identifizierten Komponenten geändert?
- [ ] Ja — alle Standard-Zugangsdaten **wurden geändert** über alle Komponenten hinweg *(Am sichersten)*  
- [ ] Ja — die Zugangsdaten **wurden geändert** für die Hauptanwendung, aber sekundäre Komponenten (z. B. CMS, DB) verbleiben im Standardzustand  
- [ ] Nein — Standard-Zugangsdaten **sind aktiv** auf einer oder mehreren identifizierbaren Schnittstellen *(Kritisch)*  

### Erzwingt die Anwendung beim ersten Login für neue oder administrative Konten eine Passwortänderung?
- [ ] Ja — eine Passwortänderung **ist obligatorisch**, bevor administrative Aktionen durchgeführt werden können  
- [ ] Nein — die Anwendung **erlaubt** die dauerhafte Nutzung von Standard-Zugangsdaten  

### Sind Sperrmechanismen oder Rate Limiting Kontrollen aktiv, um automatisierte Tests auf Standard-Zugangsdaten zu verhindern?
- [ ] Ja — aggressives Rate Limiting oder Account Lockout **wird angewendet**  
- [ ] Ja — Kontrollen sind vorhanden, aber ein Bypass **ist möglich** durch Header-Manipulation oder IP-Rotation  
- [ ] Nein — es sind keine Rate Limiting oder Sperrmechanismen **aktiviert**  

### Verwenden Plugins von Drittanbietern, Middleware oder Administrations-Tools (z. B. phpMyAdmin, Tomcat Manager) eindeutige Zugangsdaten?
- [ ] Nein — in der Umgebung existieren keine Drittanbieter-Komponenten  
- [ ] Ja — alle Drittanbieter-Komponenten verwenden eindeutige, nicht-standardmäßige Zugangsdaten  
- [ ] Nein — mindestens eine Drittanbieter-Komponente **ist zugänglich** über bekannte Standard-Zugangsdaten  

---