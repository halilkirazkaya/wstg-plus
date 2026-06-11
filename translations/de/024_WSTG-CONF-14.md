## WSTG-CONF-14 — Test auf Fehlkonfigurationen anderer HTTP-Security-Header

Die Prüfung auf Fehlkonfigurationen von HTTP-Security-Headern umfasst die Verifizierung des Vorhandenseins und der korrekten Implementierung defensiver Header, die einen wesentlichen Schutz gegen gängige webbasierte Angriffsvektoren bieten. Obwohl diese Header zugrunde liegende Code-Schwachstellen nicht beheben, erhöht ihr Fehlen das Risiko von Man-in-the-Middle (MITM)-Angriffen, MIME-sniffing exploitation und unbeabsichtigtem Cross-Origin-Datenabfluss via Referrers erheblich. Aus der Sicht eines Angreifers sind fehlende Security-Header deutliche Indikatoren für eine unzureichend gehärtete Umgebung, was oft komplexere Exploitation-Chains wie Session Hijacking oder Information Exfiltration erleichtert. Diese Konfigurationen werden in der Regel auf Serverebene evaluiert und sollten konsistent auf alle Antworttypen angewendet werden, einschließlich API-Endpunkte und statische Ressourcen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audit zum Vorhandensein von Security-Headern
| Header | Vorhanden | Fehlend | Empfohlene Konfiguration |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` or `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Funktionsspezifisch, z. B. `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Wie werden Security-Header über den gesamten Anwendungsumfang hinweg erzwungen?
- [ ] Ja — Header werden global über einen zentralen Load Balancer oder Reverse Proxy angewendet und **können nicht** überschrieben werden  
- [ ] Ja — Header werden auf Antworten von Hauptdokumenten angewendet, **fehlen** jedoch bei API-Antworten oder statischen Assets  
- [ ] Nein — Header werden inkonsistent angewendet oder **fehlen** in der gesamten Anwendungsdomäne  

### Ist der Strict-Transport-Security (HSTS) Header korrekt konfiguriert?
- [ ] Ja — `max-age` ist auf eine lange Dauer (z. B. 2 Jahre) eingestellt und `includeSubDomains` ist **aktiviert**  
- [ ] Ja — `max-age` ist gesetzt, aber `includeSubDomains` ist **deaktiviert**, wodurch Subdomänen verwundbar bleiben  
- [ ] Nein — Der HSTS-Header ist **nicht vorhanden** oder `max-age` ist auf 0 gesetzt, was Protocol Downgrade Attacks ermöglicht  

### Verhindert die Referrer-Policy den Abfluss sensibler URL-Parameter?
- [ ] Ja — Die Policy ist auf `no-referrer` oder `same-origin` gesetzt *(Am sichersten)*  
- [ ] Ja — Die Policy ist auf `strict-origin-when-cross-origin` gesetzt  
- [ ] Nein — Die Policy ist **nicht gesetzt** oder auf `unsafe-url` eingestellt, was potenziell Tokens oder sensible PII in Referer-Headern preisgibt  

### Verhindert der X-Content-Type-Options Header MIME-sniffing?
- [ ] Ja — Die `nosniff`-Direktive ist **vorhanden** und korrekt implementiert  
- [ ] Nein — Der Header **fehlt**, was einem Browser potenziell ermöglichen könnte, nicht ausführbare Dateien (z. B. Bilder) als Skripte auszuführen  

---