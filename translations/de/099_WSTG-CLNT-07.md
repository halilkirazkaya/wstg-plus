## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) ist ein browserbasierter Mechanismus, der es einem Server ermöglicht, den Zugriff auf seine Ressourcen von verschiedenen Ursprüngen (Origins) explizit zu erlauben, wodurch die Same-Origin Policy (SOP) effektiv gelockert wird. Fehlkonfigurationen treten auf, wenn eine Anwendung willkürlichen Ursprüngen übermäßig vertraut, häufig durch Reflektieren des `Origin`-Headers im `Access-Control-Allow-Origin`-Antwort-Header oder durch die Verwendung schlecht implementierter Regex-Whitelists. Aus der Sicht eines Angreifers kann eine permissive CORS-Policy ausgenutzt werden, indem ein bösartiges Skript auf einer kontrollierten Domain gehostet wird, das authentifizierte Anfragen an die verwundbare Anwendung stellt. Dies erlaubt es dem Angreifer, sensible Daten wie CSRF-Token oder PII (Personally Identifiable Information) direkt aus dem Antwort-Body zu exfiltrieren. Diese Schwachstelle ist besonders kritisch bei API-Endpunkten, die authentifizierte Sitzungen verarbeiten und nicht-öffentliche Informationen zurückgeben.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Tools:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Implementiert die Anwendung CORS-Header auf authentifizierten Endpunkten?
- [ ] Nein — CORS ist **nicht aktiviert**; der Browser verwendet standardmäßig die strikte Same-Origin Policy  
- [ ] Ja — CORS-Header sind vorhanden und auf eine strikte **Whitelist** beschränkt  
- [ ] Ja — CORS-Header sind vorhanden, aber **permissiv** konfiguriert  

### Wie verarbeitet der Server den `Access-Control-Allow-Origin` (ACAO) Header?
- [ ] ACAO ist auf eine spezifische, **vertrauenswürdige** statische Domain festgelegt  
- [ ] ACAO ist auf `*` (Wildcard) gesetzt und Credentials **können nicht** gesendet werden  
- [ ] ACAO ist auf `*` (Wildcard) gesetzt und Credentials **können** gesendet werden *(Hinweis: Die meisten Browser blockieren dies)*  
- [ ] ACAO **reflektiert dynamisch** den Wert des `Origin`-Headers aus der Anfrage  

### Ist der `Access-Control-Allow-Credentials` (ACAC) Header auf `true` gesetzt?
- [ ] Nein — ACAC ist **nicht vorhanden** oder auf `false` gesetzt  
- [ ] Ja — ACAC ist auf `true` gesetzt, aber ACAO ist auf vertrauenswürdige Ursprünge **beschränkt**  
- [ ] Ja — ACAC ist auf `true` gesetzt und ACAO **reflektiert** beliebige Ursprünge *(Kritisch)*  

### Kann die Origin-Whitelist durch Subdomain- oder Null-Origin-Manipulation umgangen werden?
- [ ] Nein — die Whitelist wird strikt erzwungen und **kann nicht** umgangen werden  
- [ ] Ja — die Whitelist akzeptiert **jede** Subdomain des Ziels (z. B. `attacker.target.com`)  
- [ ] Ja — die Whitelist akzeptiert die `null` Origin über sandboxed Iframes  
- [ ] Ja — die Whitelist ist anfällig für Regex-Bypasses (z. B. `target.com.attacker.com` oder `target.com-safe.com`)  

### Ermöglicht die CORS-Konfiguration die Exfiltration sensibler Daten?
- [ ] Nein — die exponierten Endpunkte enthalten **keine** sensiblen oder sitzungsspezifischen Daten  
- [ ] Ja — authentifizierte Endpunkte geben PII, Credentials oder CSRF-Token zurück, die von einem nicht autorisierten Ursprung gelesen werden **können**  

---