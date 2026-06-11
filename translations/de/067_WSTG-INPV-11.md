## WSTG-INPV-11 — Code Injection

Code Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Code-Snippets innerhalb ihrer Laufzeitumgebung auswertet, typischerweise durch unsichere sprachspezifische Funktionen wie `eval()`, `exec()` oder `system()`. Diese Schwachstelle unterscheidet sich von Command Injection, da sie auf den Ausführungskontext der Programmiersprache (z. B. PHP, Python, Node.js) abzielt und nicht auf die zugrunde liegende Betriebssystem-Shell. Angreifer nutzen diese Stellen aus, um beliebige Logik auszuführen, Umgebungsvariablen zu exfiltrieren oder eine vollständige Remote Code Execution (RCE) auf dem Server zu erreichen. Pentesters sollten Funktionen untersuchen, die mathematische Ausdrücke, serverseitige Templates oder dynamische Konfigurationsparameter verarbeiten, bei denen Eingaben als ausführbarer Code interpretiert werden könnten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Werkzeuge:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Evaluieren Anwendungsfunktionen dynamische Eingaben über sprachspezifische Evaluationsfunktionen?
- [ ] Nein — dynamische Evaluationsfunktionen sind in der Codebasis nicht vorhanden  
- [ ] Ja — Funktionen wie `eval()`, `exec()` oder `include()` werden verwendet, akzeptieren jedoch keine Benutzereingaben  
- [ ] Ja — Funktionen werden verwendet und verarbeiten vom Benutzer bereitgestellte Eingaben  

### Werden Mechanismen zur Eingabevalidierung oder Bereinigung (Sanitization) vor der Evaluation auf Eingaben angewendet?
- [ ] Ja — die Eingabe wird strikt gegen eine hartkodierte Whitelist validiert  
- [ ] Ja — die Eingabe wird über Blacklisting bereinigt, jedoch ist ein Bypass möglich  
- [ ] Nein — die Eingabe wird direkt an die Evaluationsfunktion übergeben und es werden keine Kontrollen angewendet  

### Ist die Ausführungsumgebung in einer Sandbox isoliert oder eingeschränkt, um Zugriff auf Systemebene zu verhindern?
- [ ] Ja — die Ausführung erfolgt in einer stark eingeschränkten Sandbox, in der keine Systemaufrufe durchgeführt werden können  
- [ ] Ja — es existieren einige Einschränkungen, aber ein Sandbox Escape ist möglich  
- [ ] Nein — die Umgebung erlaubt vollen Zugriff auf die zugrunde liegenden Sprach-APIs und Betriebssystemfunktionen *(Kritisch)*  

### Kann die Code Injection zur Exfiltration sensibler Informationen genutzt werden?
- [ ] Nein — die Ausführung erfolgt blind und es ist keine Out-of-Band-Kommunikation möglich  
- [ ] Ja — Umgebungsvariablen oder lokale Dateien können über HTTP/DNS-Anfragen exfiltriert werden  
- [ ] Ja — Ergebnisse des ausgeführten Codes werden direkt in der Antwort der Anwendung reflektiert  

### Ermöglicht die Anwendung Remote File Inclusion oder das dynamische Laden von Bibliotheken?
- [ ] Nein — es können nur lokale, vordefinierte Codepfade ausgeführt werden  
- [ ] Ja — die Anwendung kann dazu gezwungen werden, Code von einer entfernten oder vom Angreifer kontrollierten Quelle zu laden und auszuführen  

---