## WSTG-CLNT-05 — Prüfung auf CSS Injection

CSS Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Eingaben erlaubt, die Cascading Style Sheets (CSS) einer Webseite zu beeinflussen, ohne eine ordnungsgemäße Bereinigung (Sanitization) oder Maskierung (Escaping) durchzuführen. Während dies oft als kosmetisches Problem wahrgenommen wird, können Angreifer CSS Injection nutzen, um sensible Daten wie CSRF-Token oder Session IDs zu exfiltrieren, indem sie Attribut-Selektoren (Attribute Selectors) und `background-image`-Eigenschaften verwenden, um externe Anfragen auszulösen. Diese Schwachstelle tritt typischerweise an Endpunkten auf, an denen Benutzereinstellungen (z. B. benutzerdefinierte Themes, Schriftfarben) innerhalb von `<style>`-Blöcken oder Inline-`style`-Attributen reflektiert werden. Aus der Sicht eines Angreifers kann eine erfolgreiche Ausnutzung zu UI Redressing, Phishing durch unbefugte Layoutänderungen oder heimlicher Datenexfiltration in Umgebungen führen, in denen strenge Content Security Policies (CSP) ansonsten JavaScript-basierte Angriffe blockieren könnten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als Hoch eingestuft, wenn der Injektionspunkt die Exfiltration sensibler Attribute wie CSRF-Token ermöglicht oder wenn er für UI Redressing in sensiblen Workflows verwendet werden kann.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Reflektiert die Anwendung benutzergesteuerte Eingaben innerhalb von CSS-Kontexten?
- [ ] Nein — Eingaben werden **nicht** in Style-Tags, Attributen oder CSS-Dateien reflektiert  
- [ ] Ja — Eingaben werden reflektiert, sind aber strikt auf sichere alphanumerische Werte beschränkt  
- [ ] Ja — Eingaben werden innerhalb eines `style`-Attributs oder eines `<style>`-Blocks reflektiert  

### Werden CSS-spezifische Metazeichen und Funktionen ordnungsgemäß bereinigt?
- [ ] Ja — Zeichen wie `{`, `}`, `:` und Funktionen wie `url()` werden **ordnungsgemäß maskiert**  
- [ ] Ja — Bereinigung ist vorhanden, aber ein Bypass **ist möglich** über Kodierung oder CSS-Kommentare  
- [ ] Nein — es wird keine Bereinigung auf CSS-Metazeichen angewendet  

### Ist eine Datenexfiltration mittels CSS-Selektoren möglich?
- [ ] Nein — Selektoren **können keine** externen Anfragen auslösen oder Daten leaken  
- [ ] Ja — eine teilweise Datenexfiltration **ist möglich** über Attribut-Selektoren und `background-image`  
- [ ] Ja — die vollständige Exfiltration sensibler Token **ist möglich** unter Verwendung automatisierter Brute-Force-Techniken  

### Mildert eine Content Security Policy (CSP) das Risiko einer CSS Injection?
- [ ] Ja — `style-src` ist auf 'self' beschränkt und `img-src` oder `connect-src` **ist eingeschränkt**  
- [ ] Ja — eine CSP ist vorhanden, verwendet jedoch 'unsafe-inline' oder erlaubt Platzhalter für externe Domains  
- [ ] Nein — es ist keine CSP implementiert, um das Laden externer Ressourcen über CSS zu verhindern  

### Kann die Schwachstelle für UI Redressing oder Phishing genutzt werden?
- [ ] Nein — der Injektionsbereich ist zu begrenzt, um das Layout signifikant zu verändern  
- [ ] Ja — eine UI-Modifikation **ist möglich**, was das Überlagern mit bösartigen Elementen erlaubt  
- [ ] Ja — die vollständige Kontrolle über das Seitenlayout **ist möglich**, was hochgradig überzeugende Phishing-Angriffe erleichtert  

---