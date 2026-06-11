## WSTG-INPV-03 — Prüfung auf HTTP Verb Tampering

HTTP Verb Tampering nutzt Schwachstellen in der Art und Weise aus, wie Webserver und Application Frameworks den Zugriff auf spezifische Ressourcen basierend auf der verwendeten HTTP-Methode autorisieren. Angreifer versuchen, Sicherheitsbeschränkungen zu umgehen, indem sie Standardmethoden wie `GET` oder `POST` durch Alternativen wie `HEAD`, `PUT`, `OPTIONS` oder sogar beliebige, nicht standardisierte Zeichenfolgen ersetzen, die das Backend möglicherweise inkonsistent verarbeitet. Diese Schwachstelle tritt typischerweise auf, wenn Sicherheitskonfigurationen — wie Java EE web.xml Filter oder .NET Autorisierungsregeln — explizit erlaubte Methoden auflisten, aber andere nicht berücksichtigen, oder wenn ein „Deny-by-Method“-Ansatz anstelle von „Deny-all“ verwendet wird. Eine erfolgreiche Ausnutzung kann zu unbefugtem Zugriff auf administrative Funktionen, Datenänderungen oder der Offenlegung von Informationen über die interne Konfiguration des Servers führen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als Hoch eingestuft, wenn administrative Funktionen oder Datenänderungen (PUT/DELETE) ohne Authentifizierung durchgeführt werden können.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### Reagiert die Anwendung auf nicht standardisierte oder beliebige HTTP-Methoden?
- [ ] Nein — die Anwendung lehnt alle Methoden ab, außer jenen, die explizit erforderlich sind  
- [ ] Ja — die Anwendung reagiert auf Standardmethoden (OPTIONS, TRACE), aber Sicherheitsmechanismen können **nicht** umgangen werden  
- [ ] Ja — die Anwendung akzeptiert beliebige Zeichenfolgen (z. B. FOO, TEST) und verarbeitet diese als `GET`- oder `POST`-Anfragen  

### Kann die Authentifizierung/Autorisierung durch Ändern der HTTP-Methode umgangen werden?
- [ ] Nein — Sicherheitskontrollen werden global angewendet, unabhängig vom HTTP-Verb  
- [ ] Ja — Kontrollen sind vorhanden, aber ein Bypass ist mittels der `HEAD`-Methode **möglich**  
- [ ] Ja — Sicherheitskontrollen werden auf alternative Methoden **nicht angewendet**, was unbefugten Zugriff auf eingeschränkte Endpunkte ermöglicht  

### Sind gefährliche Methoden wie `PUT` oder `DELETE` auf dem Server aktiviert?
- [ ] Nein — gefährliche Methoden sind **deaktiviert** oder geben einen 405 Method Not Allowed zurück  
- [ ] Ja — Methoden sind aktiviert, erfordern jedoch eine gültige Authentifizierung mit hohen Privilegien  
- [ ] Ja — Methoden sind **aktiviert** und ermöglichen unbefugten Datei-Upload oder das Löschen von Ressourcen *(Kritisch)*  

### Gibt die `OPTIONS`-Methode sensible Informationen über erlaubte Verben preis?
- [ ] Nein — `OPTIONS` ist **deaktiviert** oder gibt eine generische Antwort zurück  
- [ ] Ja — `OPTIONS` gibt den `Allow`-Header zurück, legt jedoch keine eingeschränkten internen Methoden offen  
- [ ] Ja — `OPTIONS` legt interne oder administrative Methoden offen, die bei der weiteren Ausnutzung helfen  

### Ist die `TRACE`-Methode aktiviert, was potenziell Cross-Site Tracing (XST) ermöglicht?
- [ ] Nein — `TRACE`- und `TRACK`-Methoden sind **deaktiviert**  
- [ ] Ja — `TRACE` ist **aktiviert** und spiegelt HTTP-Header zurück, einschließlich sensibler Cookies/Tokens  

---