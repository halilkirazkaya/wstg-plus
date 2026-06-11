## WSTG-SESS-08 — Session Puzzling

Session Puzzling, auch bekannt als Session Variable Overloading, tritt auf, wenn eine Anwendung dieselbe Session-Variable für mehrere Zwecke in verschiedenen Modulen verwendet oder es versäumt, Session-Zustände während Workflow-Übergängen ordnungsgemäß zu bereinigen. Angreifer nutzen dieses Verhalten aus, indem sie Session-Variablen in einem Kontext füllen – etwa in einer unauthentifizierten Profilvorschau oder einer mehrstufigen Registrierung – und anschließend zu einem geschützten Bereich navigieren, in dem die Anwendung fälschlicherweise diesen bereits existierenden Variablen vertraut. Dies kann zu kritischen Auswirkungen führen, einschließlich Authentication Bypass, Privilege Escalation oder unbefugtem Zugriff auf sensible Daten, indem die Anwendung getäuscht wird zu glauben, dass sich eine Session in einem höher privilegierten Zustand befindet, als es tatsächlich der Fall ist. Die Schwachstelle tritt am häufigsten in komplexen, zustandsabhängigen Anwendungen auf, in denen die Session-Management-Logik inkonsistent über verschiedene funktionale Komponenten hinweg angewendet wird.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### Verwendet die Anwendung dieselben Session-Variablennamen für unterschiedliche Zwecke in verschiedenen Modulen?
- [ ] Nein — Variablennamen sind eindeutig oder Scopes sind isoliert  
- [ ] Ja — Gemeinsame Variablennamen existieren, aber der Zustand wird zwischen Modulen bereinigt  
- [ ] Ja — Gemeinsame Variablennamen existieren und der Zustand **wird nicht** bereinigt  

### Können unauthentifizierte Aktionen Session-Variablen beeinflussen, die in authentifizierten Kontexten verwendet werden?
- [ ] Nein — Session-Variablen werden erst **nach** erfolgreicher Authentifizierung initialisiert  
- [ ] Ja — Session-Variablen können vor dem Login gesetzt werden, beeinflussen aber **nicht** den Zustand nach dem Login  
- [ ] Ja — Session-Variablen, die während der Pre-Authentication gesetzt wurden, **wird** nach der Authentifizierung vertraut *(Kritisch)*  

### Initialisiert die Anwendung den Session-Identifier und die zugehörigen Variablen bei einer Änderung der Berechtigungsstufe neu?
- [ ] Ja — Die Session wird vollständig zurückgesetzt und eine neue ID **wird ausgegeben**  
- [ ] Teilweise — Eine neue ID wird ausgegeben, aber einige Session-Variablen **bleiben bestehen**  
- [ ] Nein — Die Session-ID und alle Variablen **bleiben** unverändert  

### Können Administratorrechte durch Manipulation von Session-Variablen über ein nicht privilegiertes Konto erlangt werden?
- [ ] Nein — Berechtigungsvariablen **können nicht** durch Benutzer modifiziert werden  
- [ ] Ja — Variablen können modifiziert werden, aber die Anwendung **validiert** diese gegen die Backend-Datenbank  
- [ ] Ja — Variablen können modifiziert werden und die Anwendung **vertraut** dem Session-Zustand implizit *(Kritisch)*  

### Wird der Session-Zustand sofort nach Abschluss eines bestimmten Workflows (z. B. Passwort-Reset, Checkout) bereinigt?
- [ ] Ja — Session-Variablen für den Workflow werden nach Abschluss zerstört  
- [ ] Nein — Workflow-Variablen **bleiben bestehen** und können in anderen Anwendungsbereichen wiederverwendet werden  

---