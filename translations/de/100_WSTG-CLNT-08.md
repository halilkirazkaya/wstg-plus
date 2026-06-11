## WSTG-CLNT-08 — Testen auf Cross Site Flashing

Cross-Site Flashing (XSF) ist eine clientseitige Schwachstelle, die auftritt, wenn eine Flash-Datei (SWF) benutzergesteuerte Eingaben unsachgemäß verarbeitet, was es einem Angreifer ermöglicht, bösartigen ActionScript-Code auszuführen oder eine Verbindung zur JavaScript-Umgebung des Browsers herzustellen. Durch die Manipulation von Parametern wie `FlashVars` oder URL-Query-Strings, die an Sinks wie `ExternalInterface.call`, `getURL` oder `loadMovie` übergeben werden, kann ein Angreifer Aktionen im Kontext der verwundbaren Domäne durchführen, einschließlich Session Hijacking und Datenexfiltration. Diese Schwachstelle tritt primär in Legacy-Unternehmensanwendungen auf, die noch SWF-Dateien hosten, wobei eine unzureichende Eingabevalidierung es ermöglicht, den Flash-Film als Vektor für Cross-Site Scripting (XSS) umzufunktionieren oder die Same-Origin Policy (SOP) durch zu permissive Cross-Domain-Konfigurationen zu umgehen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Werden veraltete Adobe Flash-Dateien (.swf) in der Anwendung gehostet?
- [ ] Nein — es werden keine Flash-Dateien innerhalb des Anwendungsumfangs gehostet oder referenziert  
- [ ] Ja — SWF-Dateien sind vorhanden, dienen aber nur statischen Inhalten  
- [ ] Ja — SWF-Dateien sind vorhanden und akzeptieren dynamische Parameter über `FlashVars` oder URL-Strings  

### Werden Eingaben, die an ActionScript-Sinks übergeben werden, bereinigt?
- [ ] Ja — alle Eingaben werden streng validiert und ActionScript-Sinks **können nicht** manipuliert werden  
- [ ] Ja — eine Validierung wird angewendet, aber ein Bypass **ist möglich** über spezifische Kodierungstechniken  
- [ ] Nein — Eingaben werden direkt an kritische Sinks wie `ExternalInterface.call` oder `getURL` übergeben *(Kritisch)*  

### Kann die Flash-Datei zur Ausführung von beliebigem JavaScript (XSS) verwendet werden?
- [ ] Nein — `ExternalInterface` ist **deaktiviert** oder der Parameter `allowScriptAccess` ist auf `never` gesetzt  
- [ ] Ja — `allowScriptAccess` ist auf `sameDomain` gesetzt, aber die SWF-Datei wird auf der Zieldomäne gehostet  
- [ ] Ja — `allowScriptAccess` ist auf `always` gesetzt, was XSS von jeder Domäne aus ermöglicht  

### Verhindert die `crossdomain.xml`-Richtlinie unbefugte Cross-Origin-Anfragen?
- [ ] Ja — `crossdomain.xml` ist restriktiv und erlaubt nur vertrauenswürdige, spezifische Origins  
- [ ] Nein — die Datei `crossdomain.xml` **fehlt**  
- [ ] Ja — die Richtlinie ist zu permissiv (z. B. `<allow-access-from domain="*" />`)  

### Kann die SWF-Datei gezwungen werden, externe, vom Angreifer kontrollierte Filme zu laden?
- [ ] Nein — die Anwendung **kann nicht** gezwungen werden, externe SWF-Dateien zu laden  
- [ ] Ja — die Funktionen `loadMovie` oder `loadMovieNum` akzeptieren nicht validierte externe URLs  

---