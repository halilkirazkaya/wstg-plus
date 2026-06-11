## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) ist eine Schwachstelle, bei der ein Angreifer den Browser eines Opfers dazu verleitet, eine unerwünschte Aktion auf einer anderen Website auszuführen, auf der das Opfer aktuell authentifiziert ist. Dieser Exploit nutzt das Verhalten des Browsers aus, automatisch „Ambient Credentials“ wie Session-Cookies oder Authorization-Header an ausgehende Anfragen anzuhängen. Angreifer zielen in der Regel auf zustandsändernde (state-changing) Operationen ab, wie das Ändern von Passwörtern, das Aktualisieren von E-Mail-Adressen oder die Durchführung von Finanztransaktionen, indem sie bösartige Skripte oder versteckte Formulare auf einer Drittanbieter-Seite hosten. Eine erfolgreiche Ausnutzung kann zu einer vollständigen Kontoübernahme (Account Takeover) oder unbefugten Datenänderung führen, ohne dass der Benutzer davon Kenntnis hat oder zustimmt.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel / Hoch* |

> *Die Kritikalität wird als Hoch eingestuft, wenn die anfällige Aktion eine Kontoübernahme (Account Takeover), Privilegieneskalation oder unbefugte Finanztransaktionen ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Werkzeuge:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Wurden Anti-CSRF-Token für zustandsändernde Anfragen implementiert?
- [ ] Ja — eindeutige, kryptografisch starke Token sind für alle zustandsändernden Aktionen erforderlich  
- [ ] Ja — Token sind vorhanden, aber **nicht** pro Session eindeutig oder sie sind vorhersehbar  
- [ ] Nein — Anti-CSRF-Token sind **nicht** implementiert  

### Ist die serverseitige Validierung des Anti-CSRF-Tokens robust?
- [ ] Ja — der Server validiert das Token strikt und ein Bypass ist **nicht möglich**  
- [ ] Ja — die Validierung wird durchgeführt, kann aber durch Entfernen des Token-Parameters **umgangen** werden  
- [ ] Ja — die Validierung wird durchgeführt, kann aber durch die Angabe eines leeren oder Dummy-Tokens **umgangen** werden  
- [ ] Ja — die Validierung wird durchgeführt, aber das Token ist **nicht** an die Session des Benutzers gebunden  

### Verlässt sich die Anwendung auf leicht umgehbare Methoden zum CSRF-Schutz?
- [ ] Nein — die Anwendung verlässt sich nicht allein auf schwache Header oder Origin-Prüfungen  
- [ ] Ja — der Schutz beruht ausschließlich auf dem `Referer`- oder `Origin`-Header, welche **gefälscht** (spoofed) oder entfernt werden können  
- [ ] Ja — der Schutz beruht auf der Prüfung des `X-Requested-With`-Headers, was über CORS-Fehlkonfigurationen **umgangen** werden kann  

### Sind Session-Cookies mit dem `SameSite`-Attribut konfiguriert?
- [ ] Ja — `SameSite` ist bei allen session-bezogenen Cookies auf `Strict` oder `Lax` gesetzt  
- [ ] Nein — das `SameSite`-Attribut **fehlt**, was zum browserspezifischen Standardverhalten führt  
- [ ] Nein — `SameSite` ist explizit auf `None` gesetzt, ohne das `Secure`-Flag oder zusätzliche Schutzmaßnahmen  

### Kann der CSRF-Schutz durch Ändern der HTTP-Anfragemethode umgangen werden?
- [ ] Nein — der Schutz wird unabhängig von der verwendeten HTTP-Methode erzwungen  
- [ ] Ja — Token werden nur bei `POST`-Anfragen validiert, aber die Aktion **kann** über `GET` durchgeführt werden  
- [ ] Ja — das Ändern der Methode (z. B. auf `PUT` oder `DELETE`) umgeht die Token-Prüfung  

---