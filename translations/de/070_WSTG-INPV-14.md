## WSTG-INPV-14 — Prüfung auf inkubierte Schwachstellen

Inkubierte Schwachstellen (Incubated Vulnerabilities), auch als persistente oder Second-Order-Schwachstellen bezeichnet, treten auf, wenn ein bösartiger Payload von der Anwendung in einem „ruhenden“ Zustand gespeichert und später in einem anderen Kontext ausgeführt oder verarbeitet wird. Dieses Angriffsmuster zielt typischerweise auf Back-End-Systeme, Administrationskonsolen oder automatisierte Reporting-Tools ab, die Daten aus einer gemeinsamen Datenbank oder einem Dateisystem abrufen, ohne eine angemessene Re-Validierung oder ein Output-Encoding durchzuführen. Pentester müssen den Lebenszyklus von Benutzereingaben vom Eintrittspunkt (dem „Inkubator“) bis zu jedem Ort verfolgen, an dem diese Daten schließlich gerendert oder von anderen Komponenten oder sekundären Anwendungen genutzt werden. Eine erfolgreiche Ausnutzung führt häufig zu weitreichenden Folgen wie dem Hijacking von Administrationssitzungen über Stored XSS oder Datenexfiltration durch Second-Order SQL Injection in internen Reporting-Modulen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Tools:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Gibt es Endpunkte, an denen benutzergesteuerte Eingaben für den späteren Abruf durch andere Benutzer oder Prozesse gespeichert werden?
- [ ] Nein — Die Anwendung speichert **keine** Benutzereingaben für die spätere Verwendung  
- [ ] Ja — Eingaben **werden** in Datenbankfeldern, Logdateien oder Konfigurationseinstellungen gespeichert  

### Wird eine Eingabevalidierung oder Bereinigung (Sanitization) angewendet, bevor Daten in die persistente Speicherschicht geschrieben werden?
- [ ] Ja — Eine strikte Allow-List-Validierung **wird** am Eintrittspunkt angewendet  
- [ ] Ja — Eine generische Bereinigung **wird** angewendet, aber ein Bypass **ist möglich** durch Codierung oder Nicht-Standard-Zeichen  
- [ ] Nein — Eingaben werden in ihrer rohen, unvalidierten Form gespeichert  

### Werden gespeicherte Daten ordnungsgemäß codiert oder bereinigt, wenn sie in administrativen oder sekundären Schnittstellen gerendert werden?
- [ ] Ja — Kontextsensitives Output-Encoding **wird** überall dort angewendet, wo die Daten gerendert werden  
- [ ] Ja — Das Encoding **wird** an einigen Stellen angewendet, **fehlt** jedoch an anderen (z. B. im internen Admin-Dashboard)  
- [ ] Nein — Daten werden ohne jegliches Encoding oder Sanitization gerendert, was die Ausführung von Payloads ermöglicht  

### Können in der Datenbank gespeicherte Payloads Backend-Batch-Prozesse oder Out-of-Band-Monitoringsysteme beeinträchtigen?
- [ ] Nein — Backend-Prozesse verwenden sichere Parsing-Methoden oder Parameterized Queries  
- [ ] Ja — Gespeicherte Payloads **können** Interaktionen in der Backend-Logik auslösen (z. B. CSV Injection, Command Injection in Logs)  
- [ ] Ja — Gespeicherte Payloads **können** Out-of-Band (OOB) Anfragen an vom Angreifer kontrollierte Server auslösen  

---