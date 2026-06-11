## WSTG-CLNT-14 — Prüfung auf Reverse Tabnabbing

Reverse Tabnabbing ist eine clientseitige Schwachstelle, bei der eine über `target="_blank"` geöffnete Seite über das `window.opener`-Objekt eine funktionale Referenz auf die Ursprungsseite behält. Dies ermöglicht es einer vom Angreifer kontrollierten Zielseite, den ursprünglichen, vertrauenswürdigen Tab programmatisch auf eine bösartige URL umzuleiten, in der Regel eine Phishing-Seite, die darauf ausgelegt ist, Anmeldedaten zu entwenden. Der Angriff nutzt das inhärente Vertrauen des Benutzers in den ursprünglichen Tab aus, da es unwahrscheinlich ist, dass dieser bemerkt, dass die zuvor verlassene Seite zu einer anderen Domäne navigiert ist. Moderne Sicherheitspraktiken erfordern die Verwendung der Attribute `rel="noopener"` oder `rel="noreferrer"` in Anchor-Tags oder das Setzen der `opener`-Eigenschaft auf null in JavaScript, um diese fensterübergreifende Kommunikation zu verhindern.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel* |

> *Der Schweregrad ist in den meisten modernen Browsern niedrig, da diese `target="_blank"` mittlerweile standardmäßig wie `rel="noopener"` behandeln. Der Schweregrad erhöht sich auf Mittel, wenn die Anwendung auf veraltete Browser (Legacy Browser) abzielt oder `window.open()` verwendet, ohne die `opener`-Eigenschaft auf null zu setzen.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Sind Links vorhanden, die in einem neuen Fenster oder Tab geöffnet werden?
- [ ] Nein — keine Links verwenden `target="_blank"` oder eine äquivalente JavaScript-basierte Weiterleitung  
- [ ] Ja — `target="_blank"` wird ausschließlich für interne, vertrauenswürdige Links verwendet  
- [ ] Ja — `target="_blank"` wird für externe Links oder benutzerdefinierte URLs verwendet  

### Ist die `window.opener`-Beziehung bei Anchor-Tags eingeschränkt?
- [ ] Ja — `rel="noopener"` oder `rel="noreferrer"` wird **konsequent auf alle Links** mit `target="_blank"` angewendet *(Am sichersten)*  
- [ ] Ja — Attribute werden angewendet, **fehlen** jedoch bei Hochrisiko-Links oder benutzergenerierten Links  
- [ ] Nein — Attribute werden **nicht angewendet**, was der Unterseite den Zugriff auf `window.opener` ermöglicht  

### Ist die Anwendung anfällig für Reverse Tabnabbing über JavaScript `window.open()`?
- [ ] Nein — `window.open()` wird nicht verwendet oder setzt die `opener`-Eigenschaft explizit auf null  
- [ ] Ja — `window.open()` wird verwendet und die `opener`-Referenz **bleibt für das Unterfenster zugänglich**  

### Kann ein Angreifer das Ziel eines Links kontrollieren, der in einem neuen Tab geöffnet wird?
- [ ] Nein — alle Linkziele sind fest im Code hinterlegt (hardcoded) und verweisen auf vertrauenswürdige Domänen  
- [ ] Ja — Linkziele sind benutzerdefiniert (z. B. Social-Media-Profile, Bio-Links) und wurden **nicht** im Hinblick auf Schutzmaßnahmen gegen Tabnabbing bereinigt (sanitized)  

---