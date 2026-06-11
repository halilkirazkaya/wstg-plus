## WSTG-CONF-01 — Konfiguration der Netzwerkinfrastruktur prüfen

Das Testen der Netzwerkinfrastruktur-Konfiguration umfasst die Identifizierung von Schwachstellen in den zugrunde liegenden Server- und Netzwerkkomponenten, welche die Webanwendung unterstützen. Angreifer zielen auf fehlkonfigurierte Dienste, veraltete Protokolle und unnötige offene Ports ab, um Fuß zu fassen, sich lateral zu bewegen oder Reconnaissance durchzuführen. Zu den häufigen Befunden gehören unsichere TLS-Versionen, ausführliche Service Banner und exponierte administrative Schnittstellen, die auf interne Netzwerke beschränkt sein sollten. Durch das Ausnutzen dieser Mängel auf Infrastrukturebene kann ein Angreifer die Vertraulichkeit von Daten während der Übertragung gefährden oder unbefugten Zugriff auf das Host-Betriebssystem erlangen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Tools:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Gibt es unnötige offene Ports oder Dienste, die auf dem Anwendungshost exponiert sind?
- [ ] Nein — nur erforderliche Ports (z. B. 443) sind **erreichbar**  
- [ ] Ja — zusätzliche Dienste sind exponiert, folgen jedoch einer **sicheren** Konfiguration  
- [ ] Ja — nicht essenzielle Dienste (z. B. FTP, Telnet, SMB) sind öffentlich **exponiert**  

### Geben Service Banner oder Header sensible Versionsinformationen preis?
- [ ] Nein — Banner sind entfernt oder generisch  
- [ ] Ja — Banner legen die Server-Software offen (z. B. Apache/nginx), aber **keine** Versionsnummern  
- [ ] Ja — ausführliche Banner legen spezifische Softwareversionen offen, was die **Exploitation** erleichtert  

### Folgt die TLS-Konfiguration modernen Security Best Practices?
- [ ] Ja — nur TLS 1.2/1.3 sind mit starken Cipher Suites **aktiviert**  
- [ ] Ja — veraltete Protokolle (TLS 1.0/1.1) sind **aktiviert**  
- [ ] Nein — schwache Cipher oder unsichere Protokolle (SSLv2/v3) sind **aktiviert**  

### Sind administrative oder Management-Schnittstellen auf autorisierte Netzwerke beschränkt?
- [ ] Ja — Schnittstellen (z. B. SSH, RDP, Control Panels) sind aus dem öffentlichen Internet **nicht erreichbar**  
- [ ] Nein — Schnittstellen sind öffentlich **erreichbar**, erfordern jedoch eine Multi-Faktor-Authentifizierung  
- [ ] Nein — Schnittstellen sind öffentlich **erreichbar** mit Ein-Faktor-Authentifizierung oder Standard-Zugangsdaten (Default Credentials)

---