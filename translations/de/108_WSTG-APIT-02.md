## WSTG-APIT-02 — API Broken Object Level Authorization

Broken Object Level Authorization (BOLA), auch bekannt als Insecure Direct Object Reference (IDOR), tritt auf, wenn eine API nicht validiert, ob ein Benutzer über die entsprechenden Berechtigungen verfügt, um auf eine durch eine ID identifizierte Ressource zuzugreifen oder diese zu manipulieren. Angreifer nutzen dies aus, indem sie Identifikatoren – wie numerische IDs oder UUIDs – in Pfaden der Anfrage, Abfrageparametern oder JSON-Bodys systematisch aufzählen oder erraten, um auf Daten anderer Benutzer zuzugreifen. Diese Schwachstelle ist das am häufigsten auftretende und folgenschwerste Problem in der modernen API-Sicherheit und kann zu massivem Datenabfluss (Exfiltration), unbefugten Änderungen oder einer vollständigen Kontoübernahme (Account Takeover) führen. Aus der Sicht eines Angreifers besteht das Ziel darin, Endpunkte zu identifizieren, die Objekt-Identifikatoren akzeptieren, und zu prüfen, ob die serverseitige Autorisierungslogik fehlt oder durch Manipulation dieser IDs umgangen werden kann.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Werkzeuge:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Sind Objekt-Identifikatoren innerhalb von API-Anfragen vorhersehbar oder aufzählbar?
- [ ] Nein — Identifikatoren sind lang, zufällig und kryptografisch sicher (z. B. UUIDv4)  
- [ ] Ja — Identifikatoren sind fortlaufende Ganzzahlen (z. B. `101`, `102`)  
- [ ] Ja — Identifikatoren folgen einem vorhersehbaren Muster oder werden aus öffentlichen Informationen abgeleitet  

### Führt die API bei jeder Anfrage eine serverseitige Validierung des Objekteigentums durch?
- [ ] Ja — Autorisierungsprüfungen werden bei jeder Anfrage angewendet und eine Umgehung ist **nicht möglich**  
- [ ] Ja — Prüfungen werden angewendet, aber eine Umgehung **ist möglich** über Parameter Pollution oder Method Tunneling  
- [ ] Nein — Die Anwendung verlässt sich ausschließlich darauf, dass der Benutzer authentifiziert ist, ohne das Eigentum an der spezifischen Ressource zu prüfen  

### Ist es möglich, auf die Ressource eines anderen Benutzers zuzugreifen oder diese zu ändern, indem der Identifikator geändert wird?
- [ ] Nein — Unbefugter Zugriff auf Ressourcen anderer Benutzer ist **nicht möglich**  
- [ ] Ja — Unbefugter **Lesezugriff** (Horizontale BOLA) **ist möglich**  
- [ ] Ja — Unbefugte **Änderung** oder **Löschung** (Horizontale BOLA) **ist möglich**  

### Erlaubt die API den Zugriff auf Objekte der Administrations- oder Systemebene durch Ersetzen von IDs?
- [ ] Nein — Administrative Ressourcen sind durch sekundäre Autorisierungsebenen geschützt  
- [ ] Ja — Zugriff auf oder Änderung von Ressourcen auf Systemebene (Vertikale BOLA) **ist möglich**  

### Kann die Autorisierungsprüfung umgangen werden, indem der Identifikator in einen anderen Teil der Anfrage verschoben wird?
- [ ] Nein — Die Autorisierungslogik ist konsistent, unabhängig von der Position des Parameters  
- [ ] Ja — Eine Umgehung **ist möglich**, wenn die ID vom URL-Pfad in den JSON-Body oder die Header verschoben wird  
- [ ] Ja — Eine Umgehung **ist möglich**, wenn mehrere IDs bereitgestellt werden und der Server die unbefugte ID verarbeitet  

---