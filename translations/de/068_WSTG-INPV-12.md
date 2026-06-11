## WSTG-INPV-12 — Command Injection

Command Injection tritt auf, wenn eine Anwendung unsichere, vom Benutzer bereitgestellte Daten, wie Formulareingaben, HTTP-Header oder Cookies, an eine System-Shell übergibt. Diese Schwachstelle ermöglicht es einem Angreifer, beliebige Betriebssystembefehle auf dem Server auszuführen, in der Regel mit den Berechtigungen des Webanwendungsprozesses. Aus der Sicht eines Angreifers wird dies durch die Verwendung von Shell-Metacharacters wie Semikolons, Pipes oder Backticks ausgenutzt, um bösartige Befehle an den beabsichtigten Ausführungsfluss zu koppeln. Die Auswirkungen sind häufig katastrophal und führen zu einer vollständigen Systemkompromittierung, unbefugter Datenexfiltration oder Lateral Movement innerhalb des internen Netzwerks.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Tools:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### Ruft die Anwendung Befehle auf Systemebene oder Shell-Executables auf?
- [ ] Nein — die Anwendung interagiert **nicht** mit der OS-Shell  
- [ ] Ja — die Anwendung verwendet interne APIs oder High-Level-Bibliotheken für Systemaufgaben  
- [ ] Ja — die Anwendung ruft Shell-Befehle direkt über Systemaufrufe wie `system()`, `exec()` oder `popen()` auf  

### Werden Benutzereingaben validiert oder bereinigt, bevor sie an Systemaufrufe übergeben werden?
- [ ] Ja — striktes Allow-Listing und Eingabevalidierung **werden angewendet**  
- [ ] Ja — Bereinigung (Sanitization) oder Escaping wird verwendet, aber ein Bypass **ist möglich**  
- [ ] Nein — Benutzereingaben werden **ohne** Validierung direkt an die Shell übergeben  

### Können Shell-Metacharacters verwendet werden, um den Ablauf der Befehlsausführung zu manipulieren?
- [ ] Nein — Metacharacters werden korrekt gefiltert oder escaped  
- [ ] Ja — In-Band-Ausführung, bei der die Befehlsausgabe in der Antwort reflektiert wird, **ist möglich**  
- [ ] Ja — Blind-Ausführung, bei der keine Ausgabe reflektiert wird, **ist möglich**  

### Ist eine Out-of-Band (OOB) Exfiltration vom Host aus möglich?
- [ ] Nein — der Server hat keinen Egress oder OOB-Signale (DNS/HTTP) schlagen fehl  
- [ ] Ja — Datenexfiltration über DNS- oder HTTP-basierte OOB-Signale **ist möglich**  

### Wird der Anwendungsprozess mit erhöhten Systemberechtigungen ausgeführt?
- [ ] Nein — die Anwendung läuft unter einem dedizierten Dienstkonto mit geringen Berechtigungen  
- [ ] Ja — die Anwendung läuft als `root`, `Administrator` oder als Benutzer mit hohen Berechtigungen *(Kritisch)*  

---