## WSTG-SESS-03 — Session Fixation

Session Fixation tritt auf, wenn eine Anwendung die Session-ID nach der erfolgreichen Authentifizierung eines Benutzers nicht entwertet oder rotiert. Dies ermöglicht es einem Angreifer, einem Opfer ein bekanntes Session-Token aufzuzwingen. Wenn die Anwendung die Sitzungs-ID aus der Phase vor der Authentifizierung nach dem Login beibehält, kann ein Angreifer dem Opfer einen speziell manipulierten Link senden, der eine festgelegte Session-ID enthält, und anschließend die authentifizierte Sitzung kapern (hijacken). Diese Schwachstelle manifestiert sich typischerweise in Login-Formularen oder durch Session-IDs, die über URL-Parameter und Cookies akzeptiert werden. Aus der Sicht eines Angreifers beinhaltet die Ausnutzung das „Fixieren“ der Sitzung über einen bösartigen Link oder eine Header-Injection-Schwachstelle und das Warten darauf, dass das Opfer seine Anmeldedaten eingibt, wodurch die Notwendigkeit, ein aktives Token zu stehlen, effektiv umgangen wird.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Ändert sich die Session-ID nach erfolgreicher Authentifizierung?
- [ ] Ja — eine neue Session-ID **wird vergeben** und die alte wird entwertet *(Am sichersten)*  
- [ ] Ja — eine neue Session-ID **wird vergeben**, aber die alte **bleibt gültig**  
- [ ] Nein — die Session-ID **bleibt vor und nach der Authentifizierung identisch** *(Kritisch)*  

### Akzeptiert die Anwendung Session-IDs, die über URL-Parameter übermittelt werden?
- [ ] Nein — Session-IDs werden **ausschließlich** über Cookies verwaltet  
- [ ] Ja — Session-IDs werden in der URL akzeptiert, aber beim Login **ignoriert** oder **rotiert**  
- [ ] Ja — Session-IDs in der URL werden **akzeptiert** und nach der Authentifizierung **beibehalten**  

### Kann eine vom Angreifer definierte Session-ID der Anwendung aufgezwungen werden?
- [ ] Nein — die Anwendung **weist ungültige oder nicht existierende Session-IDs zurück**, die vom Benutzer bereitgestellt werden  
- [ ] Ja — die Anwendung **akzeptiert** und initialisiert eine Sitzung unter Verwendung einer beliebigen ID, die in einem Cookie bereitgestellt wird  
- [ ] Ja — die Anwendung **akzeptiert** und initialisiert eine Sitzung unter Verwendung einer beliebigen ID, die in einem URL-Parameter bereitgestellt wird  

### Werden Sitzungen vor der Authentifizierung korrekt beendet?
- [ ] Ja — die anonyme Sitzung wird beim Login-Ereignis **vollständig zerstört**  
- [ ] Nein — die anonyme Sitzung wird **nicht zerstört**, was potenzielles Session-Harvesting ermöglicht  

### Ist das Session-Cookie gegen clientseitige Injection geschützt?
- [ ] Ja — die Flags `HttpOnly` und `Secure` werden **angewendet**, um eine skriptbasierte Fixierung zu verhindern  
- [ ] Nein — die Flags werden **nicht angewendet**, sodass Session-IDs über Cross-Site Scripting (XSS) gesetzt werden können  

---