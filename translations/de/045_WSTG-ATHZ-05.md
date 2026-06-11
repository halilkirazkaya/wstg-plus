## WSTG-ATHZ-05 — Testen auf OAuth-Schwachstellen

OAuth-Schwachstellen umfassen eine Vielzahl von Fehlern in der Implementierung der Protokolle OAuth 2.0 oder OpenID Connect (OIDC), die häufig zu einer vollständigen Kontoübernahme (Account Takeover) oder unbefugtem Datenzugriff führen. Diese Schwachstellen treten typischerweise während des Autorisierungsablaufs (Authorization Flow) auf, insbesondere bei der Validierung des `redirect_uri`, der Entropie und Verifizierung des `state`-Parameters sowie dem sicheren Umgang mit Access- oder ID-Tokens. Angreifer nutzen diese aus, indem sie Autorisierungscodes abfangen, Tokens über Open Redirects auf vom Angreifer kontrollierte Domänen umleiten oder Cross-Site Request Forgery (CSRF) durchführen, um die Sitzung eines Opfers mit dem Konto eines Angreifers zu verknüpfen. Da OAuth oft als primärer Authentifizierungsmechanismus dient, kann eine einzige Fehlkonfiguration die Integrität des gesamten Identitätssystems gefährden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Werkzeuge:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Wird der `state`-Parameter implementiert und validiert, um CSRF zu verhindern?
- [ ] Ja — `state` ist obligatorisch, pro Sitzung eindeutig und wird serverseitig streng validiert  
- [ ] Ja — `state` ist vorhanden, aber statisch oder über verschiedene Sitzungen hinweg vorhersehbar  
- [ ] Ja — `state` ist vorhanden, aber die Anwendung validiert ihn **nicht** beim Callback  
- [ ] Nein — der `state`-Parameter wird in der Autorisierungsanfrage **nicht** verwendet *(Kritisch)*  

### Wird der `redirect_uri` streng gegen eine Whitelist validiert?
- [ ] Ja — nur exakte Übereinstimmungen mit einer vorregistrierten Whitelist werden **akzeptiert**  
- [ ] Ja — die Validierung erfolgt, aber ein Bypass ist über Path Traversal oder Subdomain-Manipulation **möglich**  
- [ ] Ja — die Validierung erfolgt, aber ein Bypass ist über Parameter Pollution oder Fragment Injection **möglich**  
- [ ] Nein — die Anwendung **erlaubt** beliebige URLs im `redirect_uri`-Parameter *(Kritisch)*  

### Kann der `response_type` manipuliert werden, um den Ablauf zu ändern?
- [ ] Nein — die Anwendung akzeptiert nur den vorgesehenen Flow (z. B. `code`) und lehnt andere ab  
- [ ] Ja — ein Wechsel von `code` zu `token` (Implicit Flow) **ist möglich** und führt zum Leak von Tokens in der URL  
- [ ] Ja — Hybrid-Flows oder unbefugte `response_type`-Werte sind **aktiviert** und werden verarbeitet  

### Werden Access-Tokens oder Autorisierungscodes über Referer-Header oder den Browserverlauf geleakt?
- [ ] Nein — Tokens/Codes werden sicher verarbeitet und sensible Seiten verwenden eine angemessene `Referrer-Policy`  
- [ ] Ja — Tokens/Codes **werden** über den `Referer`-Header an Drittanbieter-Domänen geleakt  
- [ ] Ja — Tokens/Codes **sind** aufgrund von unsicherem Caching oder GET-Anfragen im Browserverlauf sichtbar  

### Setzt die Anwendung das "Least Privilege"-Prinzip für OAuth-Scopes durch?
- [ ] Ja — die Anwendung fordert nur die für die Funktionalität minimal erforderlichen Scopes an  
- [ ] Nein — die Anwendung fordert standardmäßig übermäßige oder administrative Scopes an  
- [ ] Nein — der Benutzer **kann** während der Zustimmungsphase (Consent Phase) keine spezifischen Scopes überprüfen oder ablehnen  

---