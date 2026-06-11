## WSTG-CONF-06 — Testen von HTTP-Methoden

Das Testen von HTTP-Methoden beinhaltet die Identifizierung der vom Webserver und der Anwendung unterstützten Verben, um sicherzustellen, dass nur notwendige Funktionen freigegeben sind. Über die Standardmethoden `GET` und `POST` hinaus können Server unbeabsichtigt gefährliche Methoden wie `PUT`, `DELETE` oder `TRACE` aktivieren. Diese können es Angreifern ermöglichen, bösartige Dateien hochzuladen, bestehende Inhalte zu löschen oder Session-Cookies via Cross-Site Tracing (XST) zu exfiltrieren. Pentesters untersuchen Antwort-Header wie `Allow` oder `Public` und versuchen, Beschränkungen mittels Method Overriding-Techniken oder Verb Tampering zu umgehen, um auf nicht autorisierte Funktionen zuzugreifen. Dieser Test ist entscheidend für die Härtung der Angriffsoberfläche und zur Vermeidung von Fehlkonfigurationen, die zu einer Kompromittierung des Servers oder unbefugter Datenmanipulation führen könnten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn `PUT` oder `DELETE` eine unbefugte Dateimanipulation ermöglichen oder wenn `TRACE` die Exfiltration von Session-Token erleichtert.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Sind gefährliche HTTP-Methoden wie `PUT` oder `DELETE` anwendungsweit deaktiviert?
- [ ] Ja — nur sichere Methoden (GET, POST, HEAD) **sind aktiviert**  
- [ ] Nein — `PUT` oder `DELETE` **sind aktiviert**, erfordern aber eine gültige Authentifizierung  
- [ ] Nein — `PUT` oder `DELETE` **sind aktiviert** und ohne Authentifizierung zugänglich *(Kritisch)*  

### Ist die `TRACE`-Methode deaktiviert, um Cross-Site Tracing (XST) zu verhindern?
- [ ] Ja — die `TRACE`-Methode **ist deaktiviert** oder gibt einen 405 Method Not Allowed zurück  
- [ ] Nein — die `TRACE`-Methode **ist aktiviert**, spiegelt aber keine sensiblen Header wider  
- [ ] Nein — die `TRACE`-Methode **ist aktiviert** und spiegelt `Cookie`- oder `Authorization`-Header wider *(Mittel)*  

### Wird HTTP Method Overriding (z. B. `X-HTTP-Method-Override`) vom Server unterstützt?
- [ ] Nein — der Server verarbeitet **keine** Method-Override-Header  
- [ ] Ja — der Server verarbeitet Override-Header, aber Zugriffskontrollen **werden erzwungen**  
- [ ] Ja — der Server verarbeitet Override-Header und ein Bypass der Zugriffskontrolle **ist möglich**  

### Reagiert der Server sicher auf beliebige oder falsch formatierte HTTP-Methoden?
- [ ] Ja — der Server gibt 405 Method Not Allowed oder 501 Not Implemented zurück  
- [ ] Nein — der Server gibt 200 OK oder 500 Error für beliebige Methoden wie `JEFF` oder `TEST` zurück  
- [ ] Nein — die Verwendung beliebiger Methoden ermöglicht das Umgehen von Authentifizierungs- oder Autorisierungsfiltern  

### Ist die `OPTIONS`-Methode eingeschränkt oder so konfiguriert, dass die Offenlegung von Informationen minimiert wird?
- [ ] Ja — `OPTIONS` ist deaktiviert oder gibt einen minimalen `Allow`-Header zurück  
- [ ] Nein — `OPTIONS` legt eine breite Palette aktivierter Methoden offen, einschließlich DAV oder Debugging-Verben  

---