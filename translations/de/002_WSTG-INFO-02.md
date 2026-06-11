## WSTG-INFO-02 — Fingerprint Web Server

Webserver-Fingerprinting ist der Prozess zur Identifizierung des spezifischen Softwaretyps, der Version und des zugrunde liegenden Betriebssystems eines Ziel-Webservers. Angreifer führen diese Reconnaissance durch, um bekannte Schwachstellen wie nicht gepatchte CVEs oder Konfigurationsfehler zu identifizieren, die spezifisch für bestimmte Versionen von Apache, Nginx, IIS oder andere Servertechnologien sind. Dies wird in der Regel durch die Analyse von HTTP-Response-Headern (z. B. `Server`, `X-Powered-By`), die Untersuchung eindeutiger Protokollverhaltensweisen oder die Beobachtung von Informationen erreicht, die über Standard-Fehlerseiten und -Dateien preisgegeben werden. Ein erfolgreiches Fingerprinting ermöglicht es einem Angreifer, seine Exploitation-Strategie anzupassen und von allgemeinem Scanning zu Angriffen mit hoher Erfolgswahrscheinlichkeit gegen bekannte Architekturmängel überzugehen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Informativ / Niedrig* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn das Fingerprinting eine veraltete oder End-of-Life (EOL) Serverversion mit bekannten kritischen Schwachstellen aufdeckt.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Sind identifizierende Zeichenfolgen in den HTTP-Response-Headern vorhanden?
- [ ] Nein — `Server`- und `X-Powered-By`-Header sind **nicht vorhanden** oder enthalten generische Werte  
- [ ] Ja — Header geben Aufschluss über die Serversoftware, aber **nicht** über spezifische Versionsnummern  
- [ ] Ja — Header geben Aufschluss über die spezifische Serversoftware und **exakte** Versionsnummern  

### Geben Standard-Fehlerseiten oder systemgenerierte Antworten Serverdetails preis?
- [ ] Nein — es werden benutzerdefinierte Fehlerseiten verwendet, die **keine** Serverinformationen offenlegen  
- [ ] Ja — Standard-Fehlerseiten geben den Namen der Serversoftware und/oder die Version preis  
- [ ] Ja — Fehlerseiten geben Serverdetails, die Version und das **zugrunde liegende Betriebssystem** preis  

### Wird die Serverversion über Standarddateien oder Verzeichnisstrukturen offengelegt?
- [ ] Nein — Standardinstallationsdateien, Handbücher und Testskripte wurden **entfernt**  
- [ ] Ja — Standarddateien (z. B. `info.php`, `manual/`, `test.html`) sind **vorhanden** und legen Versionsdaten offen  

### Kann der Servertyp durch die Analyse eindeutiger Verhaltensweisen genau abgeleitet werden?
- [ ] Nein — Serverantworten sind normalisiert und ein verhaltensbasiertes Fingerprinting ist **nicht möglich**  
- [ ] Ja — eindeutige Verhaltensweisen (z. B. Header-Reihenfolge, spezifische Fehlercodes oder Besonderheiten des TCP/IP-Stacks) **ermöglichen** die Serveridentifizierung  

### Wird die identifizierte Serverversion aktuell unterstützt und ist sie gepatcht?
- [ ] Ja — die identifizierte Version ist aktuell und wird vom Hersteller **unterstützt**  
- [ ] Nein — die identifizierte Version ist **veraltet** oder **End-of-Life (EOL)** mit bekannten Schwachstellen  

---