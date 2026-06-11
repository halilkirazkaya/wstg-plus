## WSTG-APIT-01 — API Reconnaissance

API Reconnaissance ist der systematische Prozess zur Identifizierung und Kartierung der API-Angriffsfläche, um Endpunkte, unterstützte Methoden und zugrunde liegende Datenstrukturen aufzudecken. Angreifer führen diese Phase durch, um undokumentierte (Shadow) APIs, veraltete Versionen mit Legacy-Schwachstellen sowie offenliegende Dokumentationsdateien wie Swagger- oder OpenAPI-Definitionen zu finden, die die interne Geschäftslogik offenbaren. Durch die Analyse vorhersehbarer URL-Muster, clientseitiger JavaScript-Dateien und der Ergebnisse von Directory Brute Force Angriffen kann ein Angreifer eine umfassende Karte der API erstellen, um Ziele für Broken Object Level Authorization (BOLA) oder Mass Assignment Angriffe zu identifizieren. Diese Reconnaissance zielt in der Regel auf gängige Pfade wie `/api/v1/`, `/swagger.json` oder `/graphql` ab, um einen Leitfaden für die Backend-Funktionalität der Anwendung zu erhalten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Informativ / Niedrig |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Werkzeuge:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Sind API-Dokumentationsdateien (Swagger, OpenAPI, WSDL) öffentlich zugänglich?
- [ ] Nein — Die API-Dokumentation ist **nicht** zugänglich oder durch Authentifizierung eingeschränkt  
- [ ] Ja — Die Dokumentation ist zugänglich, erfordert jedoch gültige Zugangsdaten  
- [ ] Ja — Die API-Dokumentation (z. B. `/swagger-ui.html`) ist ohne Authentifizierung **öffentlich zugänglich**  

### Können undokumentierte oder „Shadow“-API-Endpunkte mittels Fuzzing entdeckt werden?
- [ ] Nein — Directory- und Endpoint-Fuzzing ergaben keine undokumentierten Pfade  
- [ ] Ja — Es wurden undokumentierte Endpunkte gefunden, auf die jedoch **nicht** ohne Autorisierung zugegriffen werden kann  
- [ ] Ja — Es wurden undokumentierte Endpunkte gefunden, auf die ohne gültige Autorisierung zugegriffen werden **kann**  

### Offenbart die Anwendung mehrere API-Versionen (z. B. /v1/, /v2/, /beta/)?
- [ ] Nein — Nur die aktuelle, gehärtete API-Version ist zugänglich  
- [ ] Ja — Es existieren Legacy-Versionen, die jedoch **dieselben** Sicherheitskontrollen wie die aktuelle Version implementieren  
- [ ] Ja — Legacy-Versionen (z. B. `/v1/`) sind zugänglich und weisen **nicht** die Sicherheitskontrollen neuerer Versionen auf  

### Geben clientseitige Ressourcen (JavaScript/Mobile Apps) API-Endpunktstrukturen oder Schlüssel preis?
- [ ] Nein — Der clientseitige Code enthält **keine** hartcodierten API-Pfade oder sensiblen Schlüssel  
- [ ] Ja — Der clientseitige Code enthält API-Endpunkt-Mappings, aber **keine** sensiblen Schlüssel  
- [ ] Ja — API-Keys, Secrets oder rein interne Endpunkt-URLs **sind** in clientseitigen Ressourcen hartcodiert  

### Sind versteckte Parameter oder Header an bekannten Endpunkten auffindbar?
- [ ] Nein — Parameter-Fuzzing (z. B. via `Arjun`) ergab **keine** versteckten Eingaben  
- [ ] Ja — Es wurden versteckte Parameter (z. B. `debug=true`, `admin=1`) entdeckt, diese sind jedoch **nicht** funktionsfähig  
- [ ] Ja — Es wurden versteckte Parameter entdeckt, die verwendet werden **können**, um das Anwendungsverhalten zu ändern  

---