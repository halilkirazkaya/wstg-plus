## WSTG-INPV-17 — Prüfung auf Host Header Injection

Host Header Injection tritt auf, wenn eine Anwendung dem vom Benutzer bereitgestellten `Host`-Header vertraut und diesen ohne ausreichende Validierung in die serverseitige Logik einbezieht, wie z. B. bei der Link-Generierung oder bei Weiterleitungen. Angreifer manipulieren diesen Header, um Web Cache Poisoning zu begünstigen, Password Reset Poisoning durch Umleitung sensibler Token an eine externe Domain zu ermöglichen oder Sicherheitskontrollen zu umgehen, die auf Header-basiertem Routing beruhen. Eine erfolgreiche Ausnutzung ermöglicht es einem Angreifer, Sitzungsdaten zu exfiltrieren, ein Account Takeover durchzuführen oder ahnungslose Benutzer auf bösartige Infrastrukturen umzuleiten. Diese Schwachstelle tritt am häufigsten in Umgebungen mit Load-Balancern oder in Anwendungen auf, die absolute URLs dynamisch basierend auf dem Kontext der eingehenden Anfrage generieren.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel / Hoch* |

> *Die Kritikalität wird als Hoch eingestuft, wenn der Host-Header zur Generierung sensibler Links in E-Mails verwendet wird, wie z. B. beim Zurücksetzen von Passwörtern.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Tools:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### Spiegelt die Anwendung den Wert des `Host`-Headers in Links, Weiterleitungen oder Skripten wider?
- [ ] Nein — Die Anwendung verwendet fest kodierte Basis-URLs oder eine streng validierte Konfiguration  
- [ ] Ja — Der `Host`-Header wird gespiegelt, aber streng gegen eine Whitelist validiert  
- [ ] Ja — Der `Host`-Header wird gespiegelt und ein Bypass **ist möglich** durch fehlerhafte Werte (z. B. `expected.com:attacker.com`)  
- [ ] Ja — Der `Host`-Header wird **ohne** Validierung gespiegelt  

### Werden alternative host-bezogene Header von der Anwendung oder Infrastruktur verarbeitet?
- [ ] Nein — Header wie `X-Forwarded-Host`, `X-Host` oder `Forwarded` werden ignoriert  
- [ ] Ja — Alternative Header werden verarbeitet, überschreiben jedoch **nicht** die interne Logik  
- [ ] Ja — Alternative Header **können** verwendet werden, um den primären `Host`-Header-Wert zu überschreiben  

### Kann der `Host`-Header manipuliert werden, um systemgenerierte E-Mails (z. B. Passwort-Resets) zu manipulieren?
- [ ] Nein — E-Mail-Links verwenden eine statische, vertrauenswürdige Domain aus der Serverkonfiguration  
- [ ] Ja — Der `Host`-Header beeinflusst E-Mail-Links, aber die Validierung **kann nicht** umgangen werden  
- [ ] Ja — Der `Host`-Header **kann** manipuliert werden, um Passwort-Reset-Token an eine vom Angreifer kontrollierte Domain umzuleiten *(Kritisch)*  

### Ist die Anwendung anfällig für Web Cache Poisoning über den `Host`-Header?
- [ ] Nein — Es ist kein Caching-Mechanismus vorhanden oder der `Host`-Header ist Teil des Cache Keys  
- [ ] Ja — Ein Cache ist vorhanden, aber er validiert den `Host`-Header streng oder ignoriert ihn bei Unkeyed Inputs  
- [ ] Ja — Ein Cache existiert und **kann** durch einen injizierten `Host`-Header manipuliert werden, um bösartige Inhalte an andere Benutzer auszuliefern  

### Akzeptiert der Server beliebige `Host`-Header für das Virtual Host Routing?
- [ ] Nein — Der Server gibt bei nicht erkannten `Host`-Headern einen Fehler oder eine Standardseite zurück  
- [ ] Ja — Der Server akzeptiert jeden `Host`-Header, was für Reconnaissance oder die Enumeration interner Dienste nützlich **ist**  

---