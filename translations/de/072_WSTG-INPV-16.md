## WSTG-INPV-16 — HTTP Request Smuggling

HTTP Request Smuggling (HRS) tritt auf, wenn eine Kette von HTTP-Servern (wie z. B. ein Loadbalancer und ein Backend-Webserver) die Grenzen einer Anfrage unterschiedlich interpretiert, was in der Regel auf widersprüchliche `Content-Length`- und `Transfer-Encoding`-Header zurückzuführen ist. Ein Angreifer nutzt diese Desynchronisation aus, um einen Eintrag in den Anfrage-Puffer des Backend-Servers einzuschleusen („smuggling“), wodurch der nächsten legitimen Benutzeranfrage effektiv ein vom Angreifer kontrolliertes Segment vorangestellt wird. Diese Technik ermöglicht schwerwiegende Angriffe, einschließlich Session Hijacking, die Umgehung von Sicherheitskontrollen und Cache Poisoning. Die Ausnutzung erfolgt üblicherweise durch zeitbasierte Analysen (timing-based analysis) oder durch die Beobachtung unerwarteter Antworten vom Backend, wenn nachfolgende Anfragen an den Server gesendet werden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Kritisch / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Werkzeuge:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Verarbeiten die Frontend- und Backend-Server den `Transfer-Encoding: chunked`-Header konsistent?
- [ ] Ja — beide Server verarbeiten Chunked Encoding identisch und eine Desynchronisation ist **nicht möglich**  
- [ ] Nein — Server interpretieren Header unterschiedlich, aber eine Bereinigung/Normalisierung **wird angewendet**  
- [ ] Nein — CL.TE (Content-Length/Transfer-Encoding) Desynchronisation **ist möglich** *(Kritisch)*  
- [ ] Nein — TE.CL (Transfer-Encoding/Content-Length) Desynchronisation **ist möglich** *(Kritisch)*  

### Kann ein Angreifer den serverseitigen oder clientseitigen Cache mittels einer eingeschleusten Anfrage vergiften (Cache Poisoning)?
- [ ] Nein — Caching ist **deaktiviert** oder nicht anfällig für Poisoning  
- [ ] Ja — eingeschleuste Anfragen **können** Benutzer auf bösartige Domains umleiten oder Skripte via Cache Poisoning injizieren  

### Ist es möglich, Anfragen anderer Benutzer oder Session-Token durch Request Concatenation zu erfassen?
- [ ] Nein — Smuggling ermöglicht **keine** benutzerübergreifende Datenexposition  
- [ ] Ja — Smuggling ermöglicht es, die Anfrage des nächsten Benutzers an einen vom Angreifer kontrollierten `POST`-Body anzuhängen, wodurch eine Exfiltration **möglich ist**  

### Nutzt die Infrastruktur HTTP/2 und ist sie anfällig für H2.CL- oder H2.TE-Desynchronisation?
- [ ] Nein — die Anwendung nutzt nur HTTP/1.1 oder HTTP/2 ist sicher implementiert, ohne dass ein Downgrade stattfindet  
- [ ] Ja — HTTP/2-zu-HTTP/1.1-Cleartext-Downgrades treten auf und eine Desynchronisation **ist möglich**  

### Werden fehlerhafte Header (z. B. `Transfer-Encoding:  chunked` mit einem Leerzeichen) sicher gehandhabt?
- [ ] Ja — fehlerhafte Header werden über alle Ebenen hinweg abgelehnt oder normalisiert  
- [ ] Nein — TE.TE-Desynchronisation **ist möglich** aufgrund inkonsistenter Handhabung von fehlerhaften oder verschleierten Headern  

---