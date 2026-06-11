## WSTG-INPV-09 — Testing for XPath Injection

Eine XPath Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Informationen verwendet, um eine XPath-Abfrage für XML-Daten zu konstruieren, was es einem Angreifer ermöglicht, die Logik der Abfrage zu manipulieren. Durch das Einschleusen spezifischer Syntax wie einfache Anführungszeichen, Klammern und logische Operatoren kann ein Angreifer in der XML-Dokumentstruktur navigieren, die Authentifizierung umgehen oder sensible Datenknoten exfiltrieren. Diese Schwachstelle tritt häufig in Web-Services, Konfigurationsmanagement-Schnittstellen und Altsystemen (Legacy Systems) auf, die auf XML-basierten Datenspeichern anstelle von traditionellen SQL-Datenbanken basieren. Aus der Sicht eines Angreifers wird dies oft unter Verwendung von Boolean-basierten oder Error-basierten Techniken ausgenutzt, um den XML-Baum systematisch abzubilden und verborgene Werte aus dem Backend abzurufen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Tools:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Werden Benutzereingaben, die zur Konstruktion von XPath-Abfragen verwendet werden, ordnungsgemäß bereinigt oder parametrisiert?
- [ ] Ja — Eingaben werden **nicht** in XPath-Abfragen verwendet oder sind strikt parametrisiert  
- [ ] Ja — eine robuste Eingabevalidierung/-kodierung **wird angewendet** und ein Bypass ist **nicht möglich**  
- [ ] Ja — eine gewisse Validierung **wird angewendet**, aber ein Bypass ist mittels kodierter Zeichen **möglich**  
- [ ] Nein — Benutzereingaben werden direkt in XPath-Abfragen verkettet *(Kritisch)*  

### Offenbart die Anwendung strukturelle XML/XPath-Details über Fehlermeldungen?
- [ ] Nein — es werden generische Fehlerseiten angezeigt und **keine** XML-Details preisgegeben  
- [ ] Ja — Fehlermeldungen offenbaren das Vorhandensein von XML-Verarbeitung, aber **keine** Pfaddetails  
- [ ] Ja — ausführliche Fehlermeldungen offenbaren XPath-Syntax, Knotennamen oder Dateipfade  

### Ist es möglich, die Authentifizierungs- oder Autorisierungslogik mittels XPath-Logik zu umgehen?
- [ ] Nein — die Authentifizierungslogik basiert **nicht** auf XML/XPath oder ist sicher implementiert  
- [ ] Ja — Login- oder Berechtigungsprüfungen können mittels Logik-basierten Payloads wie `' or 1=1 or '` umgangen werden  

### Kann die XML-Dokumentstruktur oder die darin enthaltenen Daten über Blind Injection enumeriert werden?
- [ ] Nein — die Anwendung ist **nicht** anfällig für Boolean-basierte oder Zeit-basierte Rückschlüsse (Inference)  
- [ ] Ja — Knotennamen und Daten **können** mittels Boolean-basierter Rückschlüsse exfiltriert werden  
- [ ] Ja — eine vollständige XML-Tree-Traversal und Datenextraktion **ist möglich**  

---