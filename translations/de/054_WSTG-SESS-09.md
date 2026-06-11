## WSTG-SESS-09 — Prüfung auf Session Hijacking

Session Hijacking tritt auf, wenn ein Angreifer eine gültige Sitzungskennung (Session Identifier, SID) abfängt, vorhersagt oder fixiert, um unbefugten Zugriff auf die aktive Sitzung eines Benutzers zu erhalten. Diese Schwachstelle resultiert typischerweise aus unzureichender Transportverschlüsselung, fehlenden Cookie-Security-Flags oder vorhersagbaren Algorithmen zur SID-Generierung, die es einem Angreifer ermöglichen, die Authentifizierung vollständig zu umgehen. Aus der Sicht eines Angreifers ermöglicht eine erfolgreiche Ausnutzung die vollständige Identitätsübernahme des Opfers, ohne dessen Zugangsdaten zu benötigen. Dies wird häufig durch Network Sniffing, Cross-Site Scripting (XSS) oder Session Fixation Angriffe erreicht.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Werden Sitzungskennungen während der Übertragung geschützt?
- [ ] Ja — Das `Secure`-Flag ist **aktiviert** und HSTS wird strikt erzwungen  
- [ ] Ja — Das `Secure`-Flag ist **aktiviert**, aber HSTS ist **deaktiviert**  
- [ ] Nein — Das `Secure`-Flag wird **nicht angewendet**, was das Abfangen der SID über unverschlüsselte Kanäle ermöglicht *(Hoch)*  

### Ist die Sitzungskennung vor dem Zugriff durch clientseitige Skripte geschützt?
- [ ] Ja — Das `HttpOnly`-Flag wird auf alle sitzungsbezogenen Cookies **angewendet**  
- [ ] Nein — Das `HttpOnly`-Flag wird **nicht angewendet**, was die Exfiltration der SID via XSS ermöglicht *(Kritisch)*  

### Implementiert die Anwendung eine Sitzungsbindung an Client-Attribute?
- [ ] Ja — Die Sitzung ist an Client-Attribute (IP/User-Agent) gebunden und eine Wiederverwendung ist **nicht möglich**  
- [ ] Ja — Eine Sitzungsbindung ist vorhanden, aber ein Bypass ist durch Header-Spoofing **möglich**  
- [ ] Nein — Es existiert keine Sitzungsbindung; die SID **ist gültig**, wenn sie von einer beliebigen Quelle aus verwendet wird  

### Ist die Sitzungskennung ausreichend zufällig, um Vorhersagbarkeit zu verhindern?
- [ ] Ja — Die SID verwendet einen kryptographisch sicheren Pseudozufallszahlengenerator (CSPRNG)  
- [ ] Nein — Die SID-Länge ist unzureichend oder folgt einer **vorhersagbaren** Sequenz bzw. einem Muster  

### Werden gleichzeitige Sitzungen verwaltet, um das Angriffsfenster zu begrenzen?
- [ ] Nein — Die Anwendung erzwingt strikt eine einzelne aktive Sitzung pro Benutzer  
- [ ] Ja — Mehrere gleichzeitige Sitzungen sind **aktiviert**, werden aber überwacht  
- [ ] Ja — Unbegrenzte gleichzeitige Sitzungen **sind** über verschiedene Geräte/Standorte hinweg **möglich**  

---