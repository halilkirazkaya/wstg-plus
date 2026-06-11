## WSTG-INPV-18 — Testing for Server-Side Template Injection

Server-Side Template Injection (SSTI) tritt auf, wenn eine Anwendung benutzerkontrollierte Eingaben in eine serverseitige Template-Engine einbettet, ohne eine ausreichende Bereinigung (Sanitization) oder Maskierung (Escaping) durchzuführen. Dies ermöglicht es einem Angreifer, bösartige Template-Direktiven zu injizieren, die dann auf dem Server ausgeführt werden, was potenziell zu einer vollständigen Systemkompromittierung durch Remote Code Execution (RCE) führen kann. SSTI manifestiert sich typischerweise in Funktionen wie der dynamischen E-Mail-Generierung, Profilseiten und Benachrichtigungssystemen, die Engines wie Jinja2, Twig oder FreeMarker verwenden. Aus der Sicht eines Angreifers wird diese Schwachstelle häufig ausgenutzt, um sensible Umgebungsvariablen auszuspähen, interne Dateien zu lesen oder Systembefehle unter Ausnutzung der nativen Objekte und Methoden der Template-Engine auszuführen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Tools:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### Verwendet die Anwendung eine serverseitige Template-Engine zur Verarbeitung von Benutzereingaben?
- [ ] Nein — Die Anwendung verwendet **keine** serverseitigen Templates für dynamische Inhalte  
- [ ] Ja — Templates werden verwendet, aber Benutzereingaben werden **nicht** in Template-Direktiven eingebettet  
- [ ] Ja — Benutzereingaben werden **ohne** ordnungsgemäße Bereinigung in Templates eingebettet  

### Werden grundlegende arithmetische Ausdrücke ausgewertet, wenn sie in Eingabefelder injiziert werden?
- [ ] Nein — Payloads wie `{{7*7}}` oder `${7*7}` werden als literale Zeichenfolgen reflektiert  
- [ ] Ja — Ausdrücke werden ausgewertet (z. B. Rückgabe von `49`), was bestätigt, dass die Template-Engine **anfällig ist**  

### Kann auf die integrierten Objekte oder Methoden der Template-Engine zugegriffen werden?
- [ ] Nein — Der Zugriff auf gefährliche Objekte, Methoden oder Konfigurationen ist **eingeschränkt** oder erfolgt in einer Sandbox  
- [ ] Ja — Auf integrierte Objekte (z. B. `self`, `config`, `__class__`) **kann** zugegriffen werden, um sensible Daten abfließen zu lassen  
- [ ] Ja — Remote Code Execution (RCE) über Systemaufrufe oder Betriebssystembibliotheken **ist möglich**  

### Gibt es eine Sandbox oder einen Allowlist-Mechanismus, der die Ausführung gefährlicher Direktiven verhindert?
- [ ] Ja — Eine strikte Allowlist oder eine sichere Sandbox ist vorhanden und ein Bypass **ist nicht möglich**  
- [ ] Ja — Eine Sandbox ist vorhanden, aber ein Bypass **ist möglich** über spezifische, engine-abhängige Payloads  
- [ ] Nein — Es wird **keine** Sandbox oder Allowlist auf die Template-Engine angewendet  

---