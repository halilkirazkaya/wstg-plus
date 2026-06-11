## WSTG-INFO-09 — Fingerprint Web Application

Das Fingerprinting einer Webanwendung ist der Prozess der Identifizierung des spezifischen Frameworks, des Content Management Systems (CMS) oder des zugrunde liegenden Technologie-Stacks sowie der zugehörigen Versionen. Diese Aufklärungsphase (Reconnaissance) ist entscheidend, da sie es einem Angreifer ermöglicht, identifizierte Komponenten bekannten Schwachstellen (CVEs) und öffentlichen Exploits zuzuordnen, wodurch die Angriffsfläche erheblich eingegrenzt wird. Informationen werden in der Regel aus HTTP-Response-Headern, eindeutigen Dateipfaden, Cookie-Namen, HTML-Quellcode-Metadaten und kryptografischen Hashes statischer Assets gewonnen. Durch die genaue Bestimmung des Technologie-Stacks kann ein Angreifer von generischen automatisierten Scans zu einer gezielten Ausnutzung (Exploitation) versionsspezifischer Schwachstellen übergehen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Niedrig / Informativ |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Geben HTTP-Response-Header Aufschluss über den Technologie-Stack oder Versionsnummern?
- [ ] Nein — sensible Header wurden entfernt oder enthalten generische Werte  
- [ ] Ja — Standard-Header wie `X-Powered-By`, `Server` oder `X-AspNet-Version` **sind** vorhanden  
- [ ] Ja — Header legen spezifische, veraltete Versionen des Webservers oder Applikations-Frameworks offen  

### Ist das Applikations-Framework oder CMS durch eindeutige Dateipfade oder Namenskonventionen identifizierbar?
- [ ] Nein — Dateipfade sind verschleiert und lassen die zugrunde liegende Technologie **nicht** erkennen  
- [ ] Ja — gängige Verzeichnisstrukturen (z. B. `/wp-content/`, `/_next/`, `/node_modules/`) **sind** sichtbar  
- [ ] Ja — eindeutige Dateien wie `robots.txt`, `humans.txt` oder `sitemap.xml` enthalten technologiespezifische Metadaten  

### Werden spezifische Versionsnummern im HTML-Quellcode oder in statischen Assets offengelegt?
- [ ] Nein — es wurden keine Versionsinformationen in Kommentaren oder Asset-Parametern gefunden  
- [ ] Ja — HTML-Kommentare oder Meta-Tags (z. B. `<meta name="generator" content="...">`) **sind** vorhanden  
- [ ] Ja — Versions-Strings werden an CSS/JS-Dateinamen angehängt (z. B. `jquery.js?v=1.12.4`) und **können nicht** deaktiviert werden  

### Offenbaren Standard-Fehlerseiten oder Management-Schnittstellen die Software-Umgebung?
- [ ] Nein — die Anwendung verwendet benutzerdefinierte, generische Fehlerseiten für alle Statuscodes  
- [ ] Ja — Standard-Fehlerseiten für den Webserver oder das Framework **sind** aktiviert und geben Versionsdaten preis  
- [ ] Ja — administrative Login-Portale (z. B. `/wp-admin`, `/phpmyadmin`) **sind** zugänglich und identifizierbar  

### Geben Cookies Aufschluss über das Session-Management-Framework?
- [ ] Nein — Session-Cookies verwenden generische oder benutzerdefinierte Namen  
- [ ] Ja — technologiespezifische Cookie-Namen (z. B. `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) **werden** verwendet  

---