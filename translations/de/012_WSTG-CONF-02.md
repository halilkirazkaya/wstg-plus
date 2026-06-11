## WSTG-CONF-02 — Test der Konfiguration der Anwendungsplattform

Der Test der Konfiguration der Anwendungsplattform umfasst die Prüfung des zugrunde liegenden Webservers, Anwendungsservers und der Framework-Einstellungen, um sicherzustellen, dass diese gegen gängige Exploits gehärtet sind. Angreifer suchen gezielt nach Standardzugangsdaten (Default Credentials), ungepatchten Plattform-Schwachstellen und exponierten administrativen Schnittstellen, um unbefugten Zugriff zu erlangen oder Code auszuführen. Fehlkonfigurationen führen häufig zur Offenlegung sensibler Umgebungsvariablen, interner Systempfade oder zum Vorhandensein unnötiger Beispielanwendungen, welche die Angriffsfläche (Attack Surface) vergrößern. Aus professioneller Sicht dient eine schlecht konfigurierte Plattform als Einstiegspunkt mit hoher Erfolgswahrscheinlichkeit, um initialen Zugriff auf die Zielumgebung zu erhalten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn administrative Schnittstellen mit Standardzugangsdaten zugänglich sind oder wenn Beispielskripte eine Remote Code Execution (RCE) ermöglichen.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Sind Standardzugangsdaten oder -konten auf der Plattform aktiv?
- [ ] Nein — alle Standardkonten sind **deaktiviert** oder die Passwörter wurden geändert  
- [ ] Ja — Standardkonten existieren, sind aber **nicht** aus dem externen Netzwerk **erreichbar**  
- [ ] Ja — Standardkonten sind **aktiv** und über das Internet zugänglich *(Kritisch)*  

### Gibt die Plattform sensible Versionsinformationen über Header oder Fehlerseiten preis?
- [ ] Nein — Versions-Strings sind **verborgen** oder generisch  
- [ ] Ja — detaillierte Versionsinformationen **werden** in HTTP-Headern **offengelegt** (z. B. `Server`, `X-Powered-By`)  
- [ ] Ja — vollständige Stack-Traces und interne Systempfade **werden** in ausführlichen Fehlermeldungen **exponiert**  

### Sind administrative oder Management-Schnittstellen ordnungsgemäß eingeschränkt?
- [ ] Nein — administrative Schnittstellen sind **nicht exponiert**  
- [ ] Ja — Schnittstellen existieren, sind aber durch IP-Allowlisting oder Multi-Faktor-Authentifizierung (MFA) **eingeschränkt**  
- [ ] Ja — Schnittstellen (z. B. `/admin`, `/manager`, `/console`) sind mit Single-Factor-Authentifizierung **öffentlich zugänglich**  

### Sind Beispieldateien, Skripte oder Dokumentationen auf dem Produktivserver vorhanden?
- [ ] Nein — alle nicht essenziellen Dateien wurden **entfernt**  
- [ ] Ja — Beispieldateien oder Dokumentationen **sind vorhanden**, legen aber keine sensiblen Informationen offen  
- [ ] Ja — Beispielskripte (z. B. `info.php`, `examples/`) **sind vorhanden** und stellen einen direkten Angriffsvektor dar  

### Ist der Server so konfiguriert, dass er unsichere HTTP-Methoden unterstützt?
- [ ] Nein — nur `GET` und `POST` sind **aktiviert**  
- [ ] Ja — potenziell riskante Methoden wie `PUT`, `DELETE` oder `TRACE` sind **aktiviert**, aber eingeschränkt  
- [ ] Ja — riskante Methoden sind **aktiviert** und ermöglichen unbefugte Dateimanipulationen oder den Diebstahl von Zugangsdaten  

---