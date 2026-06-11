## WSTG-CLNT-10 — Testing WebSockets

WebSocket-Tests konzentrieren sich auf den Vollduplex-Kommunikationskanal (Full-Duplex), der zwischen Client und Server aufgebaut wird und herkömmliche HTTP-Request-Response-Muster umgeht. Pentester bewerten die Sicherheit des Handshake-Prozesses, das Vorhandensein von Schutzmechanismen gegen Cross-Site WebSocket Hijacking (CSWSH) und die Strenge der Eingabevalidierung für Message Payloads. Die Ausnutzung umfasst in der Regel das Hijacking einer aktiven Sitzung, um Daten in Echtzeit zu exfiltrieren, oder das Injizieren bösartiger Payloads, die serverseitige Schwachstellen wie Command Injection oder SQLi über den persistenten Socket auslösen. Da viele herkömmliche Web Application Firewalls (WAFs) den WebSocket-Verkehr nicht effektiv prüfen, bietet dieses Protokoll oft einen unauffälligen Vektor zur Umgehung von Perimeterschutzmaßnahmen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Tools:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### Wird die WebSocket-Verbindung über einen sicheren Kanal hergestellt?
- [ ] Ja — `wss://` (WebSocket Secure) wird für alle Verbindungen erzwungen  
- [ ] Nein — `ws://` wird verwendet, was Person-in-the-middle (PITM) Lauschangriffe ermöglicht  

### Ist die Anwendung gegen Cross-Site WebSocket Hijacking (CSWSH) geschützt?
- [ ] Nein — der Server validiert den `Origin`-Header während des Handshakes strikt  
- [ ] Ja — die Validierung des `Origin`-Headers ist schwach oder **kann** über Null- oder gefälschte Origins umgangen werden  
- [ ] Ja — es wird keine `Origin`-Header-Validierung durchgeführt, was Cross-Site-Angreifern ermöglicht, eine Verbindung zu initiieren *(Kritisch)*  

### Werden Eingabevalidierung und Bereinigung (Sanitization) auf WebSocket Message Payloads angewendet?
- [ ] Ja — alle eingehenden Nachrichten werden bereinigt und gegen ein Schema validiert  
- [ ] Ja — es existiert eine gewisse Validierung, aber eine Umgehung **ist möglich** durch kodierte oder nicht standardisierte Payloads  
- [ ] Nein — Nachrichten werden direkt verarbeitet, was Injection-Angriffe (XSS, SQLi, RCE) oder Logikmanipulation ermöglicht  

### Werden Authentifizierungs- und Autorisierungsprüfungen bei jeder WebSocket-Nachricht durchgeführt?
- [ ] Ja — die Sitzung wird für jede Nachricht überprüft oder das Protokoll ist zustandslos (stateless) mit Token  
- [ ] Ja — die Authentifizierung erfolgt beim Handshake, aber die Autorisierung für spezifische Aktionen wird pro Nachricht **nicht** erzwungen  
- [ ] Nein — die Authentifizierung wird nur beim Handshake geprüft, was Session Hijacking ermöglicht, um vollen Zugriff zu gewähren  

### Implementiert der Server Rate Limiting oder Ressourcenbeschränkungen für den WebSocket?
- [ ] Ja — Rate Limiting und maximale Nachrichtengrößen sind **aktiviert** und werden erzwungen  
- [ ] Nein — es existieren keine Beschränkungen, was Denial of Service (DoS) durch Message Flooding oder große Payload-Größen ermöglicht  

---