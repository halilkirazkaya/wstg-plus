## WSTG-CLNT-02 — Prüfung auf JavaScript-Ausführung

Die Prüfung auf JavaScript-Ausführung konzentriert sich auf die Identifizierung von Instanzen, in denen eine Anwendung vom Benutzer bereitgestellte Daten unsachgemäß verarbeitet, was zur Ausführung nicht autorisierter Skripte im Browserkontext des Opfers führt. Diese Schwachstelle resultiert in der Regel in Cross-Site Scripting (XSS), welches es Angreifern ermöglicht, Session-Cookies zu exfiltrieren, das Document Object Model (DOM) zu manipulieren oder Aktionen im Namen des Benutzers durchzuführen. Penetration Tester untersuchen Sinks wie `innerHTML`, `document.write()` und `eval()`, in denen unbereinigte Daten aus Quellen (Sources) wie URL-Fragmenten, `localStorage` oder `window.name` verarbeitet werden könnten. Eine erfolgreiche Ausnutzung beeinträchtigt die Integrität und Vertraulichkeit der Benutzersitzung und kann als Ausgangspunkt (Pivot) für komplexere clientseitige Angriffe dienen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Tools:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### Verarbeitet die Anwendung vom Benutzer bereitgestellte Daten in gefährlichen JavaScript-Sinks?
- [ ] Nein — alle Daten werden bereinigt (sanitized) oder in sicheren Sinks wie `textContent` verwendet  
- [ ] Ja — Daten werden in gefährlichen Sinks verarbeitet, aber eine Bereinigung/Kodierung **wird angewendet**  
- [ ] Ja — Daten werden in gefährlichen Sinks verarbeitet und eine Bereinigung **wird nicht angewendet**  

### Ist eine Content Security Policy (CSP) vorhanden, um die Skriptausführung zu unterbinden?
- [ ] Ja — eine restriktive CSP ist **aktiviert** und ein Bypass **ist nicht möglich**  
- [ ] Ja — eine CSP ist **aktiviert**, aber ein Bypass **ist möglich** über `unsafe-inline` oder unsichere CDNs  
- [ ] Nein — es ist keine CSP **aktiviert**  

### Kann JavaScript über URL-Parameter oder Fragmente ausgeführt werden (DOM-basiertes XSS)?
- [ ] Nein — die Eingabe wird korrekt kodiert und eine Ausführung **ist nicht möglich**  
- [ ] Ja — die Eingabe wird im DOM reflektiert, aber die Ausführung **wird verhindert** durch Schutzmechanismen moderner Frameworks  
- [ ] Ja — die Eingabe wird direkt im Browser ausgeführt und eine Ausnutzung (Exploitation) **ist möglich**  

### Sind Event-Handler oder URI-Schemata anfällig für Script Injection?
- [ ] Nein — die Eingabe wird bereinigt, um Event-Attribute und `javascript:`-Schemata zu entfernen  
- [ ] Ja — Event-Handler **können** injiziert werden, aber Filter begrenzen die Komplexität des Payloads  
- [ ] Ja — die Ausführung von beliebigem JavaScript über `onerror`-, `onload`- oder `href`-Attribute **ist möglich**  

---