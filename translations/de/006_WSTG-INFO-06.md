## WSTG-INFO-06 — Identifizierung von Anwendungseinstiegspunkten

Die Identifizierung von Anwendungseinstiegspunkten umfasst das Mapping der gesamten Angriffsfläche durch die Enumerierung aller möglichen Wege, auf denen ein Benutzer oder ein externes System mit der Anwendung interagieren kann. Dieser Prozess beinhaltet die Entdeckung aller URLs, API-Endpunkte, Parameter (GET, POST, cookie-basiert), HTTP-Header und spezialisierten Protokolle wie WebSockets oder gRPC. Für einen Penetration Tester ist diese Phase entscheidend, da Schwachstellen häufig in undokumentierten „Shadow“-APIs, veralteten Endpunkten oder komplexen Parameterstrukturen gefunden werden, die während der Entwicklung übersehen wurden. Eine gründliche Enumerierung stellt sicher, dass keine Komponente der Anwendung eine ungetestete Blackbox bleibt, wodurch verhindert wird, dass Angreifer unüberwachte Routen in das System finden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Informativ |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Wurden alle GET- und POST-Parameter in der gesamten Anwendung enumeriert?
- [ ] Ja — die umfassende Enumerierung mittels automatisiertem und manuellem Crawling **ist abgeschlossen**  
- [ ] Ja — es wurden nur gängige Parameter durch Standard-Crawling identifiziert  
- [ ] Nein — Parameter sind weitgehend **unidentifiziert** oder ungetestet  

### Wird mittels Fuzzing nach undokumentierten oder versteckten Parametern (z. B. debug, admin) gesucht?
- [ ] Nein — die Suche nach versteckten Parametern ist für diesen spezifischen Scope **nicht erforderlich**  
- [ ] Ja — Parameter-Fuzzing (z. B. mit `Arjun`) **wurde durchgeführt**, ohne Befunde  
- [ ] Ja — versteckte Parameter **wurden entdeckt** und für weitere Tests erfasst  
- [ ] Nein — die Suche nach versteckten Parametern **wurde nicht** durchgeführt  

### Wurden für jeden Endpunkt alle unterstützten HTTP-Methoden (PUT, DELETE, PATCH usw.) identifiziert?
- [ ] Ja — die Methodenerkennung **wird auf alle** entdeckten Endpunkte angewendet  
- [ ] Ja — die Methodenerkennung **wird nur auf** hochrelevante oder sensible Endpunkte angewendet  
- [ ] Nein — es wurden nur die Standardmethoden GET und POST **identifiziert**  

### Sind nicht-standardmäßige Einstiegspunkte wie WebSockets, gRPC oder benutzerdefinierte Header erfasst?
- [ ] Nein — die Anwendung **verwendet keine** nicht-standardmäßigen Protokolle oder benutzerdefinierten Header  
- [ ] Ja — alle protokollspezifischen Einstiegspunkte und benutzerdefinierten Header **sind identifiziert**  
- [ ] Nein — nicht-standardmäßige Einstiegspunkte **existieren**, wurden aber nicht erfasst  

### Wurde die API-Oberfläche (einschließlich versionierter Endpunkte wie /v1/ oder /v2/) vollständig erfasst?
- [ ] Nein — die Anwendung **stellt keine** API bereit  
- [ ] Ja — alle API-Versionen und Endpunkte **sind identifiziert** und dokumentiert  
- [ ] Ja — nur die aktuelle API-Produktionsversion **ist identifiziert**  
- [ ] Nein — API-Endpunkte **bleiben unerfasst** oder wurden nur teilweise entdeckt  

---