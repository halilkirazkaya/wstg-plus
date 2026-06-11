## WSTG-INFO-07 — Abbildung von Ausführungspfaden innerhalb der Anwendung

Die Abbildung von Ausführungspfaden umfasst die systematische Identifizierung aller erreichbaren Endpunkte, Funktionsabläufe und Entscheidungszweige innerhalb einer Webanwendung. Dieser Prozess ist entscheidend, um sicherzustellen, dass keine „verborgene“ Funktionalität – wie Legacy-Code, Debug-Schnittstellen oder undokumentierte API-Routen – der Sicherheitsüberprüfung entgeht, da diese häufig keine modernen Sicherheitskontrollen aufweisen. Angreifer nutzen eine Kombination aus automatisiertem Spidering und manueller Exploration, um die Struktur der Anwendung zu visualisieren. Dabei entdecken sie oft Schwachstellen wie unbefugten administrativen Zugriff oder Business-Logic-Fehler. Durch die Korrelation von Eingaben mit spezifischen serverseitigen Antworten können Tester sensible Verarbeitungslogiken lokalisieren, die gezielte Deep-Dive-Tests erfordern, um eine umfassende Abdeckung zu gewährleisten.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Informativ |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### Wurde die Anwendung durch automatisiertes und manuelles Crawling vollständig indiziert?
- [ ] Ja — eine umfassende Abbildung aller sichtbaren und verknüpften Ressourcen **ist abgeschlossen**  
- [ ] Ja — automatisiertes Crawling **ist abgeschlossen**, aber die manuelle Exploration steht noch aus  
- [ ] Nein — die Anwendung wurde **nicht** indiziert  

### Werden versteckte oder undokumentierte Endpunkte durch JS-Analyse oder Directory Brute-Forcing identifiziert?
- [ ] Nein — keine undokumentierten Pfade durch Seitenkanal-Analyse entdeckt  
- [ ] Ja — versteckte Pfade entdeckt, aber auf diese **kann nicht** ohne Autorisierung zugegriffen werden  
- [ ] Ja — undokumentierte Endpunkte **sind** zugänglich und bieten sensible Funktionalität *(Kritisch / Hoch)*  

### Können Ausführungspfade über nicht standardmäßige Parameter oder Header verändert werden?
- [ ] Nein — Parameter wie `debug`, `test` oder `admin` beeinflussen den Ausführungsfluss **nicht**  
- [ ] Ja — Parameter ändern die Ausgabe, umgehen aber **keine** Sicherheitskontrollen  
- [ ] Ja — pfadverändernde Parameter **werden angewendet** und ermöglichen das Umgehen der beabsichtigten Logik  

### Ist der Ablauf der mehrstufigen Business Logic (z. B. Multi-Faktor-Authentifizierung, Checkout) vollständig abgebildet?
- [ ] Ja — alle Zustände und Übergänge in mehrstufigen Prozessen **sind identifiziert**  
- [ ] Nein — komplexe Übergänge von Zustandsautomaten (State Machines) **können nicht** vollständig abgebildet werden  

### Sind API-Dokumentationsdateien (Swagger/OpenAPI) oder clientseitige Maps (Source Maps) exponiert?
- [ ] Nein — es sind keine Architekturkarten oder Dokumentationsdateien öffentlich exponiert  
- [ ] Ja — Dokumentation ist vorhanden, aber **ist durch** Authentifizierung geschützt  
- [ ] Ja — Swagger UI, OpenAPI JSON oder JS Source Maps **sind aktiviert** und öffentlich zugänglich  

---