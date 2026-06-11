## WSTG-ATHN-01 — Prüfung der Übertragung von Anmeldedaten über verschlüsselte Kanäle

Die Prüfung der über einen verschlüsselten Kanal übertragenen Anmeldedaten stellt sicher, dass sensible Authentifizierungsdaten wie Benutzernamen, Passwörter und Session Tokens vor dem Abfangen während der Übertragung geschützt sind. Angreifer im Netzwerk – beispielsweise durch Man-in-the-Middle (MitM)-Angriffe in öffentlichen WLANs oder kompromittierten internen Netzwerken – können Packet Sniffing-Tools verwenden, um Klartext-Anmeldedaten zu erfassen, wenn TLS/SSL nicht ordnungsgemäß erzwungen wird. Diese Schwachstelle tritt typischerweise bei Login-Endpoints, Passwort-Reset-Formularen und API-Authentifizierungs-Headern auf, bei denen die Anwendung HTTPS nicht vorschreibt oder eine fehlerhafte Weiterleitung von HTTP durchführt. Eine erfolgreiche Ausnutzung gewährt dem Angreifer vollen Zugriff auf Benutzerkonten, was zu einer vollständigen Kompromittierung der Benutzerdaten und potenzieller Lateral Movement innerhalb der Anwendung führt.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Wird HTTPS für den gesamten Authentifizierungsprozess erzwungen?
- [ ] Ja — HSTS ist **aktiviert** und der gesamte Datenverkehr wird strikt über HTTPS erzwungen  
- [ ] Ja — eine Weiterleitung zu HTTPS erfolgt, aber HSTS ist **nicht aktiviert**  
- [ ] Nein — Anmeldedaten **können** über unverschlüsseltes HTTP übermittelt werden  

### Werden Login-Seiten und -Formulare über einen verschlüsselten Kanal bereitgestellt?
- [ ] Ja — die Seite, die das Login-Formular enthält, wird über HTTPS ausgeliefert  
- [ ] Nein — die Login-Seite wird über HTTP bereitgestellt, was Form-Action Hijacking oder das Sniffing von Anmeldedaten ermöglicht  

### Ist das `Secure`-Flag für alle sensiblen Session-Cookies gesetzt?
- [ ] Ja — das `Secure`-Flag ist für alle Authentifizierungs- und Session-Cookies **gesetzt**  
- [ ] Ja — das `Secure`-Flag ist nur für einige Cookies **gesetzt**  
- [ ] Nein — das `Secure`-Flag ist **nicht gesetzt**, was dazu führen kann, dass Cookies über unverschlüsselte Anfragen exfiltriert werden  

### Unterstützt der Server schwache TLS-Versionen oder unsichere Cipher Suites?
- [ ] Nein — nur modernes TLS (1.2+) und starke Ciphers sind **aktiviert**  
- [ ] Ja — veraltete Versionen (TLS 1.0/1.1) oder schwache Ciphers (RC4, DES, CBC) werden **unterstützt**  

### Verhindert die Anwendung die Übertragung von Anmeldedaten an Drittanbieter-Domains über unverschlüsselte Kanäle?
- [ ] Ja — es werden keine sensiblen Daten an externe Domains gesendet oder alle externen Aufrufe werden über HTTPS erzwungen  
- [ ] Nein — Authentifizierungs-Header oder Anmeldedaten werden über HTTP an Drittanbieter-Endpoints (z. B. Analytics oder SSO) gesendet  

---