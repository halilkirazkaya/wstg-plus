## WSTG-INPV-04 — Prüfung auf HTTP Parameter Pollution

HTTP Parameter Pollution (HPP) tritt auf, wenn eine Anwendung mehrere HTTP-Parameter mit demselben Namen erhält und diese auf inkonsistente oder unsichere Weise verarbeitet. Durch die Bereitstellung doppelter Parameter kann ein Angreifer die interne Logik der Anwendung manipulieren und potenziell Web Application Firewalls (WAF), Eingabevalidierungsfilter oder Zugriffskontrollmechanismen umgehen. Diese Schwachstelle ist stark vom zugrunde liegenden Webserver oder Anwendungs-Framework abhängig, welches entweder das erste, das letzte oder eine verkettete Version der wiederholten Parameter wählen kann. Die Ausnutzung zielt in der Regel auf Endpunkte ab, die Parameter an Backend-APIs, Datenbankabfragen oder URL-Weiterleitungsmechanismen übergeben, was zu Auswirkungen von Cross-Site Scripting (XSS) bis hin zu unbefugter Datenexfiltration führen kann.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Wie verarbeitet die Anwendung mehrere HTTP-Parameter mit demselben Namen?
- [ ] Nein — doppelte Parameter werden vom Server **abgelehnt** oder ignoriert  
- [ ] Ja — der Server wählt konsistent nur das **erste** oder **letzte** Vorkommen aus  
- [ ] Ja — der Server **verkettet** Werte, was potenziell Injektionen ermöglicht  

### Kann HPP genutzt werden, um Sicherheitskontrollen wie eine WAF oder Eingabefilter zu umgehen?
- [ ] Nein — Sicherheitskontrollen prüfen korrekt **alle** Vorkommen eines Parameters  
- [ ] Ja — eine WAF oder ein Filter prüft nur das **erste** Vorkommen, wodurch ein bösartiges **zweites** Vorkommen passieren kann  
- [ ] Ja — die Verkettung ermöglicht es einem Angreifer, ein Payload über mehrere Parameter zu **verteilen**, um die Erkennung zu umgehen  

### Gibt die Anwendung manipulierte Parameter in der Antwort ohne ordnungsgemäße Bereinigung zurück?
- [ ] Nein — Parameter werden bereinigt oder kodiert, bevor sie in der UI ausgegeben werden  
- [ ] Ja — Pollution führt zu reflektierten Inhalten, aber eine Bereinigung **wird angewendet**  
- [ ] Ja — Pollution führt zu reflektierten Inhalten und eine Bereinigung **wird nicht angewendet**, was zu XSS oder Logikfehlern führt  

### Sind Parameter, die an interne API-Aufrufe oder Backend-Systeme übergeben werden, anfällig für Pollution?
- [ ] Nein — Backend-Anfragen verwenden sichere, nicht-dynamische Konstruktionsmethoden  
- [ ] Ja — HPP ermöglicht es einem Angreifer, Parameter in der Backend-Anfragestruktur zu **injizieren** oder zu **überschreiben**  

### Zeigt die Anwendung ein unterschiedliches Verhalten, wenn Parameter in GET- vs. POST-Anfragen manipuliert werden?
- [ ] Nein — das Verhalten ist über alle HTTP-Methoden hinweg konsistent  
- [ ] Ja — nur GET-Parameter sind anfällig für Pollution  
- [ ] Ja — nur POST-Parameter (Body) sind anfällig für Pollution  

---