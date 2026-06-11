## WSTG-ERRH-01 — Testing for Improper Error Handling

Eine unsachgemäße Fehlerbehandlung (Improper Error Handling) tritt auf, wenn eine Webanwendung sensible technische Details — wie Stack Traces, Datenbank-Schema-Informationen oder interne Dateipfade — über ihre Fehlermeldungen preisgibt. Angreifer lösen diese Fehler aus, indem sie fehlerhafte Eingaben (Malformed Input) übermitteln, auf nicht existierende Ressourcen zugreifen oder serverseitige Ausnahmen (Exceptions) provozieren, um die zugrunde liegende Architektur der Anwendung zu erfassen und potenzielle Vektoren für weitere Angriffe (Exploitation) zu identifizieren. Dieser Informationsabfluss (Information Leakage) dient oft als Vorstufe für schwerwiegendere Angriffe, einschließlich SQL Injection oder Path Traversal, indem dem Angreifer präzise Umgebungsspezifikationen geliefert werden. Eine sichere Implementierung muss generische, benutzerfreundliche Meldungen bereitstellen, während detaillierte Diagnosedaten nur in sicheren, serverseitigen Protokollen (Logs) erfasst werden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### Nutzt die Anwendung einen generischen, globalen Error Handler für nicht abgefangene Ausnahmen?
- [ ] Ja — generische Fehlerseiten sind **aktiviert** und geben **keine** technischen Details preis  
- [ ] Ja — benutzerdefinierte Fehlerseiten werden verwendet, aber ein Informationsabfluss **ist möglich** über spezifische Header  
- [ ] Nein — Standard-Fehlerseiten des Servers (z. B. Tomcat, IIS, Nginx) sind **sichtbar**  

### Können technische Informationen durch fehlerhafte Eingaben oder Grenzfälle enumeriert werden?
- [ ] Nein — Fehler werden angemessen behandelt, mit eindeutigen Referenz-IDs für die Protokollierung  
- [ ] Ja — Stack Traces oder Datenbankabfragen **werden in den Antworten offengelegt**  
- [ ] Ja — interne Dateipfade, Umgebungsvariablen oder Server-Versionsstrings **werden offengelegt**  

### Geben API-Antworten ausführliche Fehlerobjekte oder Debug-Informationen preis?
- [ ] Nein — APIs geben standardisierte Fehlercodes und bereinigte JSON-Meldungen zurück  
- [ ] Ja — APIs geben ausführliche Debug-Objekte oder vollständige Details zu Ausnahmen im Response-Body zurück  
- [ ] Ja — Stack Traces sind in den Feldern `details` oder `exception` der JSON-Antwort enthalten  

### Verhält sich die Anwendung unterschiedlich (zeitbasiert oder inhaltsbasiert), wenn Fehler auftreten?
- [ ] Nein — Antwort-Signaturen und Zeitverhalten sind konsistent, unabhängig vom Fehlertyp  
- [ ] Ja — differenzielle Antworten **können** genutzt werden, um valide vs. invalide Zustände zu enumerieren (z. B. User Enumeration)  

---