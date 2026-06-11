## WSTG-CONF-08 — Test RIA Cross Domain Policy

Cross-Domain-Richtlinien für Rich Internet Applications (RIA) umfassen Dateien wie `crossdomain.xml` und `clientaccesspolicy.xml`, die festlegen, wie Web-Clients – insbesondere Adobe Flash und Microsoft Silverlight – Cross-Origin-Anfragen verarbeiten. Diese Dateien sind sicherheitskritisch, da sie die Same-Origin Policy (SOP) explizit außer Kraft setzen und bestimmen, welche externen Domänen mit der Anwendung interagieren und deren Antworten lesen dürfen. Zu permissiv konfigurierte Richtlinien, wie die Verwendung von Wildcards im `domain`-Attribut, ermöglichen es Angreifern, bösartige RIA-Objekte auf einer Drittanbieter-Website zu hosten, die sensible Daten exfiltrieren oder Aktionen im Namen authentifizierter Benutzer durchführen können. Pentesters suchen diese Dateien in der Regel im Web-Root-Verzeichnis, um den Grad des Vertrauens gegenüber Drittanbieter-Origins zu bewerten und potenzielle Cross-Origin-Datenlecks zu identifizieren.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn die Anwendung sensible Benutzerdaten oder Session-Identifier verarbeitet, die über eine permissive Richtlinie exfiltriert werden können.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Sind Cross-Domain-Richtliniendateien (`crossdomain.xml` oder `clientaccesspolicy.xml`) im Web-Root vorhanden?
- [ ] Nein — Dateien können im Root-Verzeichnis **nicht** gefunden werden  
- [ ] Ja — `crossdomain.xml` (Flash) **ist** vorhanden  
- [ ] Ja — `clientaccesspolicy.xml` (Silverlight) **ist** vorhanden  
- [ ] Ja — beide RIA-Richtliniendateien **sind** vorhanden  

### Ist das Domain-Attribut `allow-access-from` auf vertrauenswürdige Origins beschränkt?
- [ ] Nein — Dateien existieren, enthalten aber keine `allow-access-from`-Einträge  
- [ ] Ja — spezifische, vertrauenswürdige Domänen sind explizit gewhitelistet und ein Bypass ist **nicht möglich**  
- [ ] Ja — Domänen sind gewhitelistet, enthalten aber nicht vertrauenswürdige Drittanbieter oder Subdomänen  
- [ ] Ja — ein Wildcard `*` **wird verwendet**, was den Zugriff von **jeder** Origin erlaubt *(Kritisch)*  

### Erlaubt die Richtlinie unsichere Kommunikation über HTTP für HTTPS-Ressourcen?
- [ ] Nein — `secure="true"` ist gesetzt (oder Standard), was HTTP-Origins daran hindert, auf HTTPS-Daten zuzugreifen  
- [ ] Ja — `secure="false"` ist explizit gesetzt, was MITM-Angreifern die Exfiltration sicherer Daten ermöglicht  

### Sind HTTP-Header in der Cross-Domain-Konfiguration zu permissiv?
- [ ] Nein — `allow-http-request-headers-from` ist eingeschränkt oder **nicht aktiviert**  
- [ ] Ja — spezifische Header sind für vertrauenswürdige Domänen erlaubt  
- [ ] Ja — ein Wildcard `*` wird für Header verwendet, was es Angreifern ermöglicht, benutzerdefinierte Header von jeder Domäne aus zu senden  

### Ist die `site-control` (Master-Policy) Konfiguration restriktiv?
- [ ] Nein — `permitted-cross-domain-policies` ist auf `none` oder `master-only` gesetzt  
- [ ] Ja — die Richtlinie erlaubt es anderen Dateien auf dem Server, eigene Regeln zu definieren (`all`)  

---