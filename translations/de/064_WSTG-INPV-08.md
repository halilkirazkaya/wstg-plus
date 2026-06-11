## WSTG-INPV-08 — Prüfung auf SSI Injection

Server-Side Includes (SSI) Injection tritt auf, wenn eine Anwendung Benutzereingaben nicht ausreichend bereinigt, bevor sie diese in HTML-Dateien einfügt, die von der SSI-Engine des Servers verarbeitet werden. Angreifer nutzen dies aus, indem sie SSI-Direktiven wie `<!--#exec cmd="id" -->` injizieren, um beliebige Shell-Befehle auszuführen, lokale Dateien zu lesen oder Umgebungsvariablen zu exfiltrieren. Diese Schwachstelle findet sich typischerweise in Anwendungen, die veraltete Dateierweiterungen wie `.shtml`, `.shtm` oder `.stm` verwenden, oder wenn der Webserver explizit so konfiguriert ist, dass er SSI innerhalb von Standard-HTML-Dateien parst. Eine erfolgreiche Ausnutzung gewährt dem Angreifer dieselben Berechtigungen wie dem Webserver-Prozess, was oft zu einer vollständigen Systemkompromittierung oder der Offenlegung sensibler Daten führt.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Unterstützt und verarbeitet der Webserver SSI-Direktiven?
- [ ] Nein — der Server unterstützt kein SSI oder Dateierweiterungen wie `.shtml` sind deaktiviert  
- [ ] Ja — SSI ist aktiviert, aber Benutzereingaben werden nicht in den verarbeiteten Seiten reflektiert  
- [ ] Ja — SSI ist aktiviert und Benutzereingaben werden in den verarbeiteten Seiten reflektiert  

### Können beliebige Systembefehle über die `#exec`-Direktive ausgeführt werden?
- [ ] Nein — die `#exec`-Direktive ist deaktiviert oder durch die Serverkonfiguration eingeschränkt  
- [ ] Ja — die Befehlsausführung ist über `#exec cmd` oder `#exec cgi` Payloads möglich  

### Ist die Exfiltration von sensiblen Dateien oder Umgebungsvariablen möglich?
- [ ] Nein — Direktiven wie `#include` oder `#config` sind deaktiviert  
- [ ] Ja — das Lesen lokaler Dateien (z. B. `/etc/passwd`) ist über `#include virtual` möglich  
- [ ] Ja — Server-Umgebungsvariablen können über `#printenv` oder `#echo` exfiltriert werden  

### Sind Eingabevalidierung oder WAF-Kontrollen gegen SSI-Payloads wirksam?
- [ ] Ja — eine strikte Eingabevalidierung wird angewendet und ein Bypass ist nicht möglich  
- [ ] Ja — Validierung/WAF wird angewendet, aber ein Bypass ist unter Verwendung von Zeichenkodierung oder unterschiedlicher SSI-Syntax möglich  
- [ ] Nein — es ist keine Validierung oder kein WAF-Schutz für SSI-Zeichen (`<`, `!`, `#`, `-`, `"`) vorhanden  

---