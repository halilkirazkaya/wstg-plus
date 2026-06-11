## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Stored Cross Site Scripting (XSS), auch bekannt als Persistent XSS, tritt auf, wenn eine Anwendung Daten von einem Benutzer empfängt und diese in einer persistenten Datenbank, einem Dateisystem oder einem anderen Speichermedium speichert, ohne eine angemessene Validierung oder Kodierung vorzunehmen. Wenn ein Opfer anschließend eine Seite aufruft, die diese unbereinigten Daten abruft und anzeigt, wird das bösartige Skript im Kontext des Browsers unter der Herkunft (Origin) der Anwendung ausgeführt. Diese Schwachstelle ist besonders gefährlich, da es nicht erforderlich ist, dass ein Opfer auf einen speziell präparierten Link klickt; jeder Benutzer, der die betroffene Seite betrachtet, wird zum Ziel, was potenziell zu Session Hijacking, unbefugten Aktionen oder dem Diebstahl von Zugangsdaten (Credential Harvesting) führen kann. Angreifer zielen in der Regel auf Kommentarbereiche, Benutzerprofile, Foren und administrative Protokolle (Logs) ab, in denen Eingaben konsistent für andere Benutzer oder Administratoren gerendert werden.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Werkzeuge:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Gibt es Eingabepunkte, die Benutzereingaben zur späteren Anzeige für andere Benutzer speichern?
- [ ] Nein — die Anwendung speichert keine benutzergesteuerten Eingaben für ein späteres Rendering  
- [ ] Ja — die Eingabe wird gespeichert (z. B. Profile, Kommentare), aber **nicht** in einem HTML-Kontext gerendert  
- [ ] Ja — die Eingabe wird gespeichert und für andere Benutzer oder Administratoren gerendert  

### Wird eine Ausgabekodierung (Output Encoding) oder Bereinigung (Sanitization) angewendet, wenn gespeicherte Daten gerendert werden?
- [ ] Ja — kontextsensitive Ausgabekodierung **wird angewendet** und ein Bypass ist **nicht möglich** *(Sicherster Zustand)*  
- [ ] Ja — Bereinigung (z. B. DOMPurify) **wird angewendet**, aber ein Bypass ist aufgrund von Konfigurationsfehlern **möglich**  
- [ ] Nein — Daten werden direkt in das DOM gerendert, **ohne** Kodierung oder Bereinigung *(Hoch)*  

### Können die Eingabevalidierung oder die Bereinigung durch alternative Payloads umgangen werden?
- [ ] Nein — Filter verarbeiten verschiedene Kodierungen, nicht standardisierte Tags und Event-Handler sicher  
- [ ] Ja — ein Bypass ist mittels HTML-Entity-Kodierung oder speziellen Tags (z. B. `<svg>`, `<img>`) **möglich**  
- [ ] Ja — ein Bypass ist über Polyglot-Payloads oder mutationsbasiertes XSS (mXSS) **möglich**  

### Bietet eine Content Security Policy (CSP) eine zusätzliche Schutzebene?
- [ ] Ja — eine strikte CSP ist **aktiviert** und verhindert die Ausführung von Inline-Skripten und nicht autorisierten Quellen  
- [ ] Ja — eine CSP ist **aktiviert**, aber sie ist fehlerhaft konfiguriert (z. B. `unsafe-inline` oder eine weit gefasste `script-src`)  
- [ ] Nein — es ist **keine** CSP für die Anwendung aktiviert  

### Ist es möglich, "Stored XSS to RCE" oder andere Ketten mit hoher Auswirkung (Blind XSS) zu realisieren?
- [ ] Nein — die Auswirkungen sind auf den clientseitigen Browserkontext beschränkt  
- [ ] Ja — gespeichertes XSS **kann** verwendet werden, um Administratoren in internen Panels anzugreifen (Blind XSS)  
- [ ] Ja — gespeichertes XSS **kann** mit anderen Schwachstellen kombiniert werden (z. B. CSRF, Exfiltration sensibler Daten)  

---