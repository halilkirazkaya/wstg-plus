## WSTG-CONF-12 — Content Security Policy (CSP)

Content Security Policy (CSP) ist ein entscheidender Defense-in-Depth-Mechanismus, der über HTTP-Response-Header implementiert wird, um Risiken wie Cross-Site Scripting (XSS), Clickjacking und Data Injection Attacks zu minimieren. Durch die Definition zulässiger dynamischer Ressourcen schränkt eine gut konfigurierte CSP die Fähigkeit eines Angreifers ein, nicht autorisierte Skripte auszuführen oder sensible Daten an externe Domänen zu exfiltrieren. Pentesters bewerten die Policy auf zu permissive Direktiven, die Verwendung unsicherer Schlüsselwörter wie `'unsafe-inline'` und die Abhängigkeit von unsicheren CDNs, die Bypasses ermöglichen könnten. Eine schwache oder fehlende CSP stellt nicht direkt eine Schwachstelle dar, erhöht jedoch die Auswirkungen erfolgreicher Injection-Angriffe erheblich, da eine zusätzliche Schutzschicht fehlt.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Tools:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Ist ein Content-Security-Policy (CSP) Header in den Anwendungsantworten vorhanden?
- [ ] Ja — `Content-Security-Policy`-Header ist **vorhanden** und wird **erzwungen**  
- [ ] Ja — `Content-Security-Policy-Report-Only` ist zu Testzwecken **vorhanden**  
- [ ] Nein — kein CSP-Header in den Antworten **vorhanden**  

### Sind die Direktiven `script-src` oder `default-src` ordnungsgemäß eingeschränkt?
- [ ] Ja — Direktiven verwenden striktes Whitelisting, Nonces oder Hashes; ein Bypass ist **nicht möglich**  
- [ ] Ja — Direktiven sind **korrekt konfiguriert**, aber die Whitelist enthält bekannte, umgehbare CDNs  
- [ ] Nein — Direktiven verwenden Wildcards (`*`) oder `data:`-Schemata, wodurch ein Bypass **möglich** ist  

### Erlaubt die Policy die Ausführung von unsicheren Inline-Skripten oder eval?
- [ ] Nein — `'unsafe-inline'` und `'unsafe-eval'` sind **nicht vorhanden**  
- [ ] Ja — `'unsafe-inline'` ist **vorhanden**, aber durch eine `nonce` oder einen `hash` geschützt  
- [ ] Ja — `'unsafe-inline'` oder `'unsafe-eval'` sind ohne zusätzliche Schutzmaßnahmen **aktiviert** *(Kritisch)*  

### Ist die Anwendung durch die Direktive `frame-ancestors` gegen Clickjacking geschützt?
- [ ] Ja — `frame-ancestors` ist auf `'none'` oder `'self'` gesetzt  
- [ ] Ja — `frame-ancestors` ist **aktiviert**, erlaubt jedoch spezifische vertrauenswürdige Drittanbieter-Domänen  
- [ ] Nein — `frame-ancestors` **fehlt**; es wird sich auf veraltete `X-Frame-Options` verlassen oder es besteht kein Schutz  

### Gibt es einen Mechanismus zur Meldung von CSP-Verletzungen?
- [ ] Ja — `report-uri` oder `report-to` ist **konfiguriert** und aktiv  
- [ ] Nein — Reporting von Verletzungen ist **deaktiviert** oder **nicht konfiguriert**  

---