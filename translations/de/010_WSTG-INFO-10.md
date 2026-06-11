## WSTG-INFO-10 — Anwendungsarchitektur abbilden

Das Mapping der Anwendungsarchitektur umfasst die Identifizierung der zugrunde liegenden Technologien, Infrastrukturkomponenten und Datenflusspfade, welche die Webanwendung stützen. Durch die Analyse von HTTP-Response-Headern, Fehlermeldungen, Cookie-Formaten und Dateierweiterungen können Tester ein Schema der verwendeten Webserver, Applikationsserver, Datenbanken und Drittanbieter-Integrationen rekonstruieren. Dieses Wissen ist grundlegend für die Anpassung nachfolgender Angriffe, da es die Auswahl plattformspezifischer Exploits sowie die Identifizierung potenzieller Engpässe oder fehlkonfigurierter Zwischengeräte wie Load Balancer und WAFs ermöglicht. Ein Angreifer nutzt diese Strukturkarte, um vom generischen Scanning zur gezielten Exploitation des spezifischen Software-Stacks und seiner vernetzten Komponenten überzugehen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Informativ / Niedrig* |

> *Der Schweregrad wird als Mittel eingestuft, wenn die interne Netzwerktopologie oder private IP-Adressen offengelegt werden.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Können die Webserver- und Applikationsserver-Technologien genau identifiziert werden?
- [ ] Nein — keine Identifizierung **möglich** aufgrund von effektivem Hardening oder Obfuskation  
- [ ] Ja — der Technologietyp ist bekannt, aber spezifische Versionen **können nicht** bestimmt werden  
- [ ] Ja — Technologie und spezifische Versionen **sind** vollständig über Header oder Dateisignaturen offengelegt *(Informativ)*  

### Sind Zwischengeräte (WAFs, Load Balancer, Proxies) im Kommunikationspfad erkennbar?
- [ ] Nein — es werden keine Zwischengeräte erkannt oder sie agieren vollständig transparent  
- [ ] Ja — die Existenz wird aufgrund von Timing oder Verhalten vermutet, aber die Identität **ist nicht** bestätigt  
- [ ] Ja — spezifische Produkte (z. B. Cloudflare, Nginx, F5) **sind** über eindeutige Header oder Verhalten identifiziert  

### Gibt die Anwendung Informationen über ihre interne Verzeichnisstruktur oder Netzwerktopologie preis?
- [ ] Nein — interne Pfade und IPs werden **nicht** offengelegt  
- [ ] Ja — interne Pfade werden in Fehlermeldungen oder Stack Traces geleakt, aber interne IPs **nicht**  
- [ ] Ja — sowohl interne Verzeichnisstrukturen als auch private IP-Adressen **werden** offengelegt  

### Kann der Typ und die Version der Backend-Datenbank aus dem Anwendungsverhalten abgeleitet werden?
- [ ] Nein — Datenbankdetails **können nicht** bestimmt werden  
- [ ] Ja — der Datenbanktyp (z. B. PostgreSQL, MSSQL) wird über Fehlermeldungen oder Verhalten abgeleitet  
- [ ] Ja — Datenbanktyp und -version **sind** explizit in Response-Bodies oder Headern offengelegt  

### Wird die Anwendung bei identifizierbaren Cloud-Service-Providern oder spezifischen Drittanbieter-CDNs gehostet?
- [ ] Nein — die Hosting-Infrastruktur ist **nicht** identifizierbar  
- [ ] Ja — der Cloud-Anbieter (AWS, Azure, GCP) ist identifiziert, aber spezifische Dienste **sind es nicht**  
- [ ] Ja — spezifische Cloud-Dienste (z. B. S3-Buckets, Lambda, Azure Functions) **sind** abgebildet und erreichbar  

---