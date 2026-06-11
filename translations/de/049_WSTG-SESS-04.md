## WSTG-SESS-04 — Prüfung auf offengelegte Session-Variablen

Offengelegte Session-Variablen treten auf, wenn sensible Session-Identifikatoren oder zustandsbezogene Daten über unsichere Kanäle wie URL Query Strings, Referer-Header oder Systemprotokolle übertragen werden. Diese Offenlegung erhöht das Risiko für Session Hijacking erheblich, da Identifikatoren im Browserverlauf zwischengespeichert, von zwischengeschalteten Proxies protokolliert oder über den Referer-Header an Drittanbieter-Seiten weitergegeben werden können. Pentester identifizieren diese Schwachstellen, indem sie alle ausgehenden Anfragen überwachen und Anwendungsprotokolle oder Serverkonfigurationen auf unbeabsichtigten Datenabfluss prüfen. In der Praxis nutzen Angreifer diese Leaks aus, indem sie gültige Session Tokens von gemeinsam genutzten Rechnern sammeln oder Traffic-Muster analysieren, um die Authentifizierung zu umgehen und unbefugten Zugriff auf Benutzerkonten zu erlangen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn die offengelegte Variable ein Session Token ist, das eine sofortige Kontoübernahme (Account Takeover) ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### Wird der Session-Identifikator im URL Query String übertragen?
- [ ] Nein — Session-Identifikatoren sind **nicht** im URL Query String vorhanden  
- [ ] Ja — Identifikatoren sind vorhanden, aber die Session ist kurzlebig oder das Risiko ist gering  
- [ ] Ja — eindeutige Session IDs **sind** im URL Query String vorhanden *(Kritisch)*  

### Gibt die Anwendung Session Tokens über den Referer-Header preis?
- [ ] Nein — die `Referrer-Policy` verhindert den Abfluss oder es existieren keine Links zu Drittanbietern  
- [ ] Ja — Session Tokens **werden** über den Referer-Header an Drittanbieter-Domains übertragen  

### Werden Session-Variablen in serverseitigen Protokollen oder Anwendungs-Logs gespeichert?
- [ ] Nein — Session-Variablen werden maskiert oder aus den Logs ausgeschlossen  
- [ ] Ja — Session-Variablen **werden** im Klartext in Anwendungs- oder Webserver-Logs aufgezeichnet  

### Verwendet die Anwendung GET-Anfragen für zustandsverändernde Operationen?
- [ ] Nein — die Anwendung verwendet `POST` oder andere nicht-idempotente Methoden für sensible Operationen  
- [ ] Ja — `GET` wird verwendet, was dazu führt, dass Session-Daten im Browserverlauf und in zwischengeschalteten Proxies zwischengespeichert werden  

### Ist der `Cache-Control`-Header so konfiguriert, dass das Caching von session-bezogenen Daten verhindert wird?
- [ ] Ja — `Cache-Control: no-store` (oder ähnlich) **wird** auf sensible Antworten angewendet  
- [ ] Ja — Kontrollen sind vorhanden, aber ein Bypass **ist** über Browser-Sonderfälle möglich  
- [ ] Nein — sensible Antworten, die Session-Daten enthalten, **können** vom Browser zwischengespeichert werden  

---