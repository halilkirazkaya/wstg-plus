## WSTG-CLNT-03 — HTML Injection

HTML-Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Eingaben nicht ordnungsgemäß bereinigt, bevor diese im Browser gerendert werden. Dies ermöglicht es einem Angreifer, beliebige HTML-Tags in das Document Object Model (DOM) der Webseite einzuschleusen. Obwohl sie dem Cross-Site Scripting (XSS) ähnelt, besteht das Hauptziel der HTML-Injection darin, die visuelle Darstellung der Seite zu manipulieren oder Phishing-Angriffe durch das Einschleusen bösartiger Formulare und täuschender Inhalte zu erleichtern. Angreifer zielen auf Parameter ab, die in der Benutzeroberfläche (UI) reflektiert werden, wie Suchbegriffe, Benutzerprofilfelder oder Fehlermeldungen, um Opfer zur Preisgabe sensibler Anmeldedaten (Credentials) oder zur Interaktion mit nicht autorisierten Drittanbieter-Links zu verleiten. Diese Schwachstelle stellt ein erhebliches Risiko für die Integrität der Benutzeroberfläche der Anwendung dar und kann ein Vorläufer für komplexere Social Engineering- oder sitzungsbezogene Angriffe sein.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Werden vom Benutzer kontrollierte Eingaben ohne ordnungsgemäße HTML-Kodierung im DOM reflektiert?
- [ ] Nein — alle reflektierten Eingaben werden vor dem Rendern strikt HTML-kodiert  
- [ ] Ja — einige Zeichen **werden** kodiert, aber Bypasses für bestimmte Tags **sind möglich**  
- [ ] Ja — die Eingabe wird roh reflektiert, und HTML-Injection **ist möglich**  

### Können strukturelle HTML-Elemente injiziert werden, um das Seitenlayout zu verändern?
- [ ] Nein — die Anwendung filtert oder entfernt strukturelle Tags wie `<div>`, `<table>` oder `<iframe>`  
- [ ] Ja — strukturelle Elemente **können** injiziert werden, haben aber **keinen** signifikanten Einfluss auf die UI  
- [ ] Ja — ein signifikantes UI-Defacement **ist möglich** über injizierte Tags  

### Ist es möglich, funktionale Phishing-Elemente wie Formulare oder bösartige Links zu injizieren?
- [ ] Nein — `<form>`, `<input>` und `<a>` Tags werden effektiv blockiert oder bereinigt  
- [ ] Ja — bösartige Links **können** injiziert werden, um Benutzer auf externe Seiten umzuleiten  
- [ ] Ja — funktionale `<form>` Elemente **können** injiziert werden, um Anmeldedaten abzugreifen *(Mittel)*  

### Mildern Sicherheits-Header oder Content Security Policies (CSP) die Auswirkungen der Injection ab?
- [ ] Ja — eine restriktive CSP ist implementiert und `form-action` oder `frame-src` **wird angewendet**  
- [ ] Nein — Sicherheits-Header sind vorhanden, aber die CSP **ist deaktiviert** oder enthält unsafe-inline/Wildcards  
- [ ] Nein — keine CSP oder relevanten Sicherheits-Header **sind aktiviert**  

---