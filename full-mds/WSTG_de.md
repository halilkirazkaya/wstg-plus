## WSTG-INFO-01 — Durchführung von Suchmaschinen-Discovery und Reconnaissance auf Informationslecks

Die Entdeckung über Suchmaschinen (Search Engine Discovery) umfasst die Nutzung öffentlicher Suchmaschinen, Cache-Seiten und Indizierungsdiensten, um Informationen zu identifizieren, die die Zielorganisation unbeabsichtigt offengelegt hat. Angreifer verwenden fortgeschrittene Suchoperatoren (Google Dorks) und Indizierungsdienste von Drittanbietern, um sensible Dateien, Verzeichnisse, Login-Portale, Fehlermeldungen und Metadaten zu entdecken, die nicht öffentlich zugänglich sein sollten. Diese Reconnaissance-Phase ist kritisch, da sie keine direkte Interaktion mit dem Ziel erfordert, was sie praktisch unentdeckbar macht, während sie potenziell hochwertige Einstiegspunkte wie exponierte Administrationspanels, Konfigurationsdateien, Datenbank-Dumps und interne Dokumentationen offenbart.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Werden sensible Dateien oder Verzeichnisse von Suchmaschinen indiziert?
- [ ] Nein — keine sensiblen Inhalte in den Suchmaschinenergebnissen gefunden  
- [ ] Ja — Konfigurationsdateien, Backups oder interne Dokumente **sind** indiziert *(Kritisch)*  

### Offenbart Google Dorking exponierte Administrations- oder Login-Schnittstellen?
- [ ] Nein — Admin-Panels und Login-Seiten sind **nicht** indiziert  
- [ ] Ja — Admin-Panels **sind** indiziert, erfordern jedoch eine starke Multi-Faktor-Authentifizierung  
- [ ] Ja — Admin-Panels **sind** indiziert und ohne ausreichende Kontrollen öffentlich zugänglich  

### Werden in Cache-Versionen von Seiten sensible Daten offengelegt, die inzwischen entfernt wurden?
- [ ] Nein — keine sensiblen Daten in Cache-Snapshots gefunden  
- [ ] Ja — Cache-Seiten enthalten Anmeldedaten, interne IP-Adressen oder sensible Informationen  

### Gibt die Datei `robots.txt` unbeabsichtigt sensible Pfade preis?
- [ ] Nein — `robots.txt` offenbart keine sensiblen Verzeichnisstrukturen  
- [ ] Ja — `robots.txt` listet sensible Pfade auf, die die Reconnaissance eines Angreifers unterstützen  
- [ ] Es existiert keine `robots.txt`-Datei  

### Offenbaren Dienste von Drittanbietern (Shodan, Censys, Wayback Machine) historische oder aktuelle Expositionen?
- [ ] Nein — keine signifikanten Funde durch Indizierungsdienste von Drittanbietern  
- [ ] Ja — historische Snapshots oder Dienst-Scans offenbaren sensible Metadaten oder Service-Banner

---

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

## WSTG-INFO-03 — Überprüfung von Webserver-Metadateien auf Information Leakage

Webserver-Metadateien wie `robots.txt`, `sitemap.xml` und `security.txt` dienen dazu, Crawler zu steuern und administrative Metadaten bereitzustellen. Häufig legen sie jedoch unbeabsichtigt sensible Verzeichnisstrukturen oder versteckte Endpoints offen. Angreifer analysieren diese Dateien, um die Angriffsfläche (Attack Surface) der Anwendung zu enumerieren. Dabei identifizieren sie "Disallow"-Anweisungen, die häufig auf nicht verlinkte Administrations-Panels, Backup-Verzeichnisse oder Entwicklungsumgebungen hinweisen. Neben Anweisungen für Suchmaschinen können Dateien im Verzeichnis `.well-known/` oder Dateien wie `humans.txt` Informationen über den Technologiestack, Entwicklerdaten oder organisatorische Sicherheitskontakte offenlegen. Eine systematische Überprüfung dieser Dateien ermöglicht es einem Tester, strukturelle und technische Einblicke zu gewinnen, ohne eine aggressive Brute-Force-Suche durchzuführen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Severity** | Niedrig / Informativ* |

> *Die Kritikalität erhöht sich auf "Mittel" (Medium), wenn Metadateien nicht authentifizierte Administrations-Schnittstellen oder sensible Konfigurations-Backups offenlegen.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Existiert die Datei `robots.txt` und legt sie sensible Verzeichnisse offen?
- [ ] Nein — `robots.txt` existiert **nicht**  
- [ ] Ja — `robots.txt` existiert, enthält aber nur standardmäßige öffentliche Pfade  
- [ ] Ja — `robots.txt` legt sensible Verzeichnispfade oder Administrations-Schnittstellen offen  
- [ ] Ja — `robots.txt` legt Zugangsdaten oder spezifische Softwareversionen in Kommentaren offen  

### Ist eine `sitemap.xml`-Datei vorhanden und offenbart sie eine versteckte Anwendungsstruktur?
- [ ] Nein — `sitemap.xml` existiert **nicht**  
- [ ] Ja — `sitemap.xml` ist vorhanden, listet aber nur öffentlich zugängliche URLs auf  
- [ ] Ja — `sitemap.xml` listet nicht verlinkte Endpoints oder rein interne Ressourcen auf, auf die zugegriffen werden **kann**  

### Legen Sicherheits- und organisationsbezogene Metadateien technische Details offen?
- [ ] Nein — keine Metadateien im Root-Verzeichnis oder unter `.well-known/` gefunden  
- [ ] Ja — `security.txt` oder `humans.txt` sind vorhanden, enthalten aber nur Standardinformationen  
- [ ] Ja — Metadateien legen interne Hostnamen, Entwickleridentitäten oder Details zum Technologiestack offen, die Social Engineering oder weitere Angriffe begünstigen **können**  

### Sind veraltete oder Framework-spezifische Metadateien vorhanden?
- [ ] Nein — keine Framework-spezifischen Dateien (z. B. `info.php`, `composer.json`, `package.json`) gefunden  
- [ ] Ja — Dateien wie `README.md`, `CHANGELOG.txt` oder `.DS_Store` sind vorhanden, wurden aber bereinigt  
- [ ] Ja — Framework- oder versionsspezifische Dateien sind vorhanden und **ermöglichen** ein präzises Technologie-Fingerprinting

---

## WSTG-INFO-04 — Enumerierung von Anwendungen auf dem Webserver

Die Enumerierung von Anwendungen ist der Prozess der Identifizierung aller Webanwendungen und Dienste, die auf einem einzelnen Webserver oder einer IP-Adresse gehostet werden. Da moderne Webserver häufig mehrere Virtual Hosts, veraltete Staging-Umgebungen oder administrative Schnittstellen auf nicht standardmäßigen Ports und Pfaden hosten, bleibt ein erheblicher Teil der Angriffsfläche (Attack Surface) ungeprüft, wenn diese verborgenen Assets nicht identifiziert werden. Angreifer nutzen Techniken wie DNS Zone Transfers, Virtual Host Brute-Forcing und Reverse IP Lookups, um diese vergessenen oder verschleierten Einstiegspunkte zu entdecken, die häufig weniger sicher sind als die primäre Produktionsanwendung.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Informativ / Niedrig |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Werkzeuge:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Sind der Zielinfrastruktur mehrere IP-Adressen oder DNS-Aliase zugeordnet?
- [ ] Nein — es existieren nur eine einzelne IP und ein A-Record  
- [ ] Ja — mehrere IP-Adressen oder Aliase wurden identifiziert, was den Scope erweitert  

### Hostet der Webserver mehrere Virtual Hosts (vhosts) auf derselben IP?
- [ ] Nein — nur die primäre Domain wird vom Server bedient  
- [ ] Ja — zusätzliche Virtual Hosts wurden identifiziert, aber der Zugriff ist nicht möglich (z. B. 403 Forbidden)  
- [ ] Ja — verborgene, Entwicklungs- oder Staging-Virtual-Hosts wurden identifiziert und sind zugänglich  

### Werden Anwendungen auf nicht standardmäßigen Ports gehostet?
- [ ] Nein — es sind nur Standard-Ports (80/443) offen  
- [ ] Ja — nicht standardmäßige Ports sind offen, hosten aber keine Webanwendungen  
- [ ] Ja — zusätzliche Webanwendungen wurden auf nicht standardmäßigen Ports identifiziert (z. B. 8080, 8443, 9000)  

### Sind verschiedene Anwendungen bestimmten URI-Pfaden zugeordnet?
- [ ] Nein — eine einzelne Anwendung bedient das gesamte Root-Verzeichnis  
- [ ] Ja — mehrere Anwendungen wurden durch Verzeichnis-Enumerierung (Directory Enumeration) entdeckt (z. B. `/api`, `/portal`, `/backup`)  

### Ergeben Reconnaissance-Dienste von Drittanbietern historische oder sekundäre Anwendungen?
- [ ] Nein — keine signifikanten Ergebnisse von Shodan, Censys oder der Wayback Machine  
- [ ] Ja — historische Snapshots oder Service-Scans zeigen Anwendungen auf, die aktuell aktiv, aber nicht verlinkt sind

---

## WSTG-INFO-05 — Überprüfung von Webseiteninhalten auf Informationsabfluss (Information Leakage)

Die Überprüfung von Webseiteninhalten umfasst die manuelle und automatisierte Inspektion von Anwendungsantworten, einschließlich HTML-, JavaScript- und CSS-Dateien, um unbeabsichtigt gegenüber Benutzern offengelegte sensible Informationen zu identifizieren. Angreifer untersuchen den clientseitigen Quellcode nach Entwicklerkommentaren, versteckten Formularfeldern, internen Serverpfaden, fest codierten API-Schlüsseln (API Keys) und Debugging-Informationen, die in weiteren Exploit-Phasen hilfreich sein können. Ein solcher Informationsabfluss (Information Leakage) tritt häufig auf, wenn Entwickler vergessen, Metadaten oder Debugging-Code vor dem Wechsel in die Produktion zu entfernen, was potenziell Logikfehler oder Details der Backend-Infrastruktur preisgibt. Aus Sicht eines Angreifers bietet dies eine aufwandsarme Methode, um die interne Struktur der Anwendung abzubilden und übersehene Einstiegspunkte (Entry Points) zu entdecken, ohne Sicherheitsalarme auszulösen oder direkt mit der Backend-Logik des Servers zu interagieren.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn sensible fest codierte Anmeldedaten (Credentials), private Schlüssel (Private Keys) oder interne Umgebungsvariablen entdeckt werden.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Werkzeuge:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Befinden sich Entwicklerkommentare mit sensiblen Informationen im Quellcode?
- [ ] Nein — keine Kommentare oder nur allgemeine, nicht-sensible Kommentare gefunden  
- [ ] Ja — Kommentare existieren, enthalten aber **keine** sensible Logik oder Daten  
- [ ] Ja — Kommentare **offenbaren** interne Pfade, SQL-Abfragen oder administrative Anweisungen  
- [ ] Ja — Kommentare **offenbaren** Anmeldedaten oder sensible Entwicklernotizen *(Hoch)*  

### Enthalten versteckte HTML-Eingabefelder sensible Daten oder Statusinformationen?
- [ ] Nein — keine versteckten Felder gefunden oder sie enthalten nur nicht-sensible Token  
- [ ] Ja — versteckte Felder werden für den Status verwendet, sind aber **verschlüsselt** oder **signiert**  
- [ ] Ja — versteckte Felder enthalten **Klartext-Passwörter**, Benutzerrollen oder interne IDs  

### Sind fest codierte Geheimnisse, API-Schlüssel oder interne IP-Adressen in JavaScript-Dateien vorhanden?
- [ ] Nein — keine sensiblen Zeichenfolgen oder Geheimnisse in JS-Dateien gefunden  
- [ ] Ja — JS-Dateien enthalten öffentliche API-Schlüssel mit **angemessenen** Beschränkungen  
- [ ] Ja — JS-Dateien enthalten **ungeschützte** API-Schlüssel, interne IPs oder private Endpunkte  
- [ ] Ja — JS-Dateien enthalten **fest codierte** administrative Anmeldedaten oder private Schlüssel *(Kritisch)*  

### Gibt die Anwendung Framework-Versionen oder interne Dateipfade im Quellcode preis?
- [ ] Nein — Framework-Informationen und Dateipfade sind **minimiert** (minified) oder **obfuskierte**  
- [ ] Ja — Framework-Namen und Versionen sind **sichtbar**, aber gepatcht  
- [ ] Ja — interne Dateipfade oder absolute Serverpfade sind im Quellcode **exponiert**  

### Ist der Quellcode aufgrund fehlender Minimierung oder Obfuskierung anfällig für Informationsabfluss?
- [ ] Nein — Produktionscode ist **vollständig minimiert** und von Kommentaren befreit  
- [ ] Ja — Code ist minimiert, aber Kommentare **verbleiben** im Quellcode  
- [ ] Ja — Source Maps (`.map` Dateien) sind **öffentlich zugänglich**, was eine vollständige Rekonstruktion des Quellcodes ermöglicht

---

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

## WSTG-INFO-07 — Abbildung von Ausführungspfaden innerhalb der Anwendung

Die Abbildung von Ausführungspfaden umfasst die systematische Identifizierung aller erreichbaren Endpunkte, Funktionsabläufe und Entscheidungszweige innerhalb einer Webanwendung. Dieser Prozess ist entscheidend, um sicherzustellen, dass keine „verborgene“ Funktionalität – wie Legacy-Code, Debug-Schnittstellen oder undokumentierte API-Routen – der Sicherheitsüberprüfung entgeht, da diese häufig keine modernen Sicherheitskontrollen aufweisen. Angreifer nutzen eine Kombination aus automatisiertem Spidering und manueller Exploration, um die Struktur der Anwendung zu visualisieren. Dabei entdecken sie oft Schwachstellen wie unbefugten administrativen Zugriff oder Business-Logic-Fehler. Durch die Korrelation von Eingaben mit spezifischen serverseitigen Antworten können Tester sensible Verarbeitungslogiken lokalisieren, die gezielte Deep-Dive-Tests erfordern, um eine umfassende Abdeckung zu gewährleisten.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Informativ |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### Wurde die Anwendung durch automatisiertes und manuelles Crawling vollständig indiziert?
- [ ] Ja — eine umfassende Abbildung aller sichtbaren und verknüpften Ressourcen **ist abgeschlossen**  
- [ ] Ja — automatisiertes Crawling **ist abgeschlossen**, aber die manuelle Exploration steht noch aus  
- [ ] Nein — die Anwendung wurde **nicht** indiziert  

### Werden versteckte oder undokumentierte Endpunkte durch JS-Analyse oder Directory Brute-Forcing identifiziert?
- [ ] Nein — keine undokumentierten Pfade durch Seitenkanal-Analyse entdeckt  
- [ ] Ja — versteckte Pfade entdeckt, aber auf diese **kann nicht** ohne Autorisierung zugegriffen werden  
- [ ] Ja — undokumentierte Endpunkte **sind** zugänglich und bieten sensible Funktionalität *(Kritisch / Hoch)*  

### Können Ausführungspfade über nicht standardmäßige Parameter oder Header verändert werden?
- [ ] Nein — Parameter wie `debug`, `test` oder `admin` beeinflussen den Ausführungsfluss **nicht**  
- [ ] Ja — Parameter ändern die Ausgabe, umgehen aber **keine** Sicherheitskontrollen  
- [ ] Ja — pfadverändernde Parameter **werden angewendet** und ermöglichen das Umgehen der beabsichtigten Logik  

### Ist der Ablauf der mehrstufigen Business Logic (z. B. Multi-Faktor-Authentifizierung, Checkout) vollständig abgebildet?
- [ ] Ja — alle Zustände und Übergänge in mehrstufigen Prozessen **sind identifiziert**  
- [ ] Nein — komplexe Übergänge von Zustandsautomaten (State Machines) **können nicht** vollständig abgebildet werden  

### Sind API-Dokumentationsdateien (Swagger/OpenAPI) oder clientseitige Maps (Source Maps) exponiert?
- [ ] Nein — es sind keine Architekturkarten oder Dokumentationsdateien öffentlich exponiert  
- [ ] Ja — Dokumentation ist vorhanden, aber **ist durch** Authentifizierung geschützt  
- [ ] Ja — Swagger UI, OpenAPI JSON oder JS Source Maps **sind aktiviert** und öffentlich zugänglich

---

## WSTG-INFO-08 — Fingerprint Web Application Framework

Das Fingerprinting eines Web-Application-Frameworks umfasst die Identifizierung des spezifischen Technologie-Stacks und der Softwareversionen, die zum Erstellen und Bereitstellen der Anwendung verwendet werden. Angreifer analysieren HTTP-Response-Header, framework-spezifische Cookies, vorhersehbare Dateistrukturen und eindeutige JavaScript-Artefakte, um festzustellen, ob das Ziel auf Plattformen wie React, Angular, Django oder Spring läuft. Diese Reconnaissance-Phase ist entscheidend, da sie es einem Tester ermöglicht, die Angriffsfläche auf bekannte Schwachstellen (CVEs) und häufige framework-spezifische Konfigurationsfehlern einzugrenzen. Durch das Verständnis der zugrunde liegenden Architektur kann ein Angreifer von generischen Tests zu hochgradig gezielten Exploitation-Strategien übergehen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Informativ (Informational) |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Geben HTTP-Response-Header den Namen oder die Version des Frameworks preis?
- [ ] Nein — Header wie `X-Powered-By` oder `Server` sind **nicht** vorhanden oder wurden bereinigt (sanitized)  
- [ ] Ja — Der Name des Frameworks ist vorhanden, aber die Version **ist verborgen**  
- [ ] Ja — Sowohl der Name des Frameworks als auch die Version **werden offengelegt**  

### Sind framework-spezifische Cookies oder Verzeichnisstrukturen identifizierbar?
- [ ] Nein — Gängige Framework-Artefakte (z. B. `.js`-Pfade, Cookie-Namen) sind verschleiert (obfuscated) oder umbenannt  
- [ ] Ja — Framework-spezifische Cookies (z. B. `JSESSIONID`, `PHPSESSID`, `csrftoken`) **sind** identifizierbar  
- [ ] Ja — Verzeichnisstrukturen (z. B. `/wp-content/`, `_next/static/`) **sind** sichtbar und bestätigen das Framework  

### Geben Fehlermeldungen oder Quellcode-Kommentare Details zum Framework preis?
- [ ] Nein — Die Fehlerbehandlung ist generisch und Kommentare wurden entfernt  
- [ ] Ja — Framework-spezifische Stack-Traces **sind aktiviert** und in den Antworten sichtbar  
- [ ] Ja — Der HTML-Quellcode enthält Kommentare oder Metadaten-Tags, die das Framework identifizieren  

### Gibt es bekannte Schwachstellen in Verbindung mit der identifizierten Framework-Version?
- [ ] Nein — Das identifizierte Framework ist aktuell und weist **keine bekannten** öffentlichen Schwachstellen auf  
- [ ] Ja — Die Framework-Version ist veraltet und spezifische CVEs **können** identifiziert werden

---

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

## WSTG-INFO-10 — Anwendungsarchitektur abbilden

Das Mapping der Anwendungsarchitektur umfasst die Identifizierung der zugrunde liegenden Technologien, Infrastrukturkomponenten und Datenflusspfade, welche die Webanwendung stützen. Durch die Analyse von HTTP-Response-Headern, Fehlermeldungen, Cookie-Formaten und Dateierweiterungen können Tester ein Schema der verwendeten Webserver, Applikationsserver, Datenbanken und Drittanbieter-Integrationen rekonstruieren. Dieses Wissen ist grundlegend für die Anpassung nachfolgender Angriffe, da es die Auswahl plattformspezifischer Exploits sowie die Identifizierung potenzieller Engpässe oder fehlkonfigurierter Zwischengeräte wie Load Balancer und WAFs ermöglicht. Ein Angreifer nutzt diese Strukturkarte, um vom generischen Scanning zur gezielten Exploitation des spezifischen Software-Stacks und seiner vernetzten Komponenten überzugehen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Informativ / Niedrig* |

> *Der Schweregrad wird als Mittel eingestuft, wenn die interne Netzwerktopologie oder private IP-Adressen offengelegt werden.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Können die Webserver- und Applikationsserver-Technologien genau identifiziert werden?
- [ ] Nein — keine Identifizierung **möglich** aufgrund von effektivem Hardening oder Obfuskation  
- [ ] Ja — der Technologietyp ist bekannt, aber spezifische Versionen **können nicht** bestimmt werden  
- [ ] Ja — Technologie und spezifische Versionen **sind** vollständig über Header oder Dateisignaturen offengelegt *(Informativ)*  

### Sind Zwischengeräte (WAFs, Load Balancer, Proxies) im Kommunikationspfad erkennbar?
- [ ] Nein — es werden keine Zwischengeräte erkannt oder sie agieren vollständig transparent  
- [ ] Ja — die Existenz wird aufgrund von Timing oder Verhalten vermutet, aber die Identität **ist nicht** bestätigt  
- [ ] Ja — spezifische Produkte (z. B. Cloudflare, Nginx, F5) **sind** über eindeutige Header oder Verhalten identifiziert  

### Gibt die Anwendung Informationen über ihre interne Verzeichnisstruktur oder Netzwerktopologie preis?
- [ ] Nein — interne Pfade und IPs werden **nicht** offengelegt  
- [ ] Ja — interne Pfade werden in Fehlermeldungen oder Stack Traces geleakt, aber interne IPs **nicht**  
- [ ] Ja — sowohl interne Verzeichnisstrukturen als auch private IP-Adressen **werden** offengelegt  

### Kann der Typ und die Version der Backend-Datenbank aus dem Anwendungsverhalten abgeleitet werden?
- [ ] Nein — Datenbankdetails **können nicht** bestimmt werden  
- [ ] Ja — der Datenbanktyp (z. B. PostgreSQL, MSSQL) wird über Fehlermeldungen oder Verhalten abgeleitet  
- [ ] Ja — Datenbanktyp und -version **sind** explizit in Response-Bodies oder Headern offengelegt  

### Wird die Anwendung bei identifizierbaren Cloud-Service-Providern oder spezifischen Drittanbieter-CDNs gehostet?
- [ ] Nein — die Hosting-Infrastruktur ist **nicht** identifizierbar  
- [ ] Ja — der Cloud-Anbieter (AWS, Azure, GCP) ist identifiziert, aber spezifische Dienste **sind es nicht**  
- [ ] Ja — spezifische Cloud-Dienste (z. B. S3-Buckets, Lambda, Azure Functions) **sind** abgebildet und erreichbar

---

## WSTG-CONF-01 — Konfiguration der Netzwerkinfrastruktur prüfen

Das Testen der Netzwerkinfrastruktur-Konfiguration umfasst die Identifizierung von Schwachstellen in den zugrunde liegenden Server- und Netzwerkkomponenten, welche die Webanwendung unterstützen. Angreifer zielen auf fehlkonfigurierte Dienste, veraltete Protokolle und unnötige offene Ports ab, um Fuß zu fassen, sich lateral zu bewegen oder Reconnaissance durchzuführen. Zu den häufigen Befunden gehören unsichere TLS-Versionen, ausführliche Service Banner und exponierte administrative Schnittstellen, die auf interne Netzwerke beschränkt sein sollten. Durch das Ausnutzen dieser Mängel auf Infrastrukturebene kann ein Angreifer die Vertraulichkeit von Daten während der Übertragung gefährden oder unbefugten Zugriff auf das Host-Betriebssystem erlangen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Tools:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Gibt es unnötige offene Ports oder Dienste, die auf dem Anwendungshost exponiert sind?
- [ ] Nein — nur erforderliche Ports (z. B. 443) sind **erreichbar**  
- [ ] Ja — zusätzliche Dienste sind exponiert, folgen jedoch einer **sicheren** Konfiguration  
- [ ] Ja — nicht essenzielle Dienste (z. B. FTP, Telnet, SMB) sind öffentlich **exponiert**  

### Geben Service Banner oder Header sensible Versionsinformationen preis?
- [ ] Nein — Banner sind entfernt oder generisch  
- [ ] Ja — Banner legen die Server-Software offen (z. B. Apache/nginx), aber **keine** Versionsnummern  
- [ ] Ja — ausführliche Banner legen spezifische Softwareversionen offen, was die **Exploitation** erleichtert  

### Folgt die TLS-Konfiguration modernen Security Best Practices?
- [ ] Ja — nur TLS 1.2/1.3 sind mit starken Cipher Suites **aktiviert**  
- [ ] Ja — veraltete Protokolle (TLS 1.0/1.1) sind **aktiviert**  
- [ ] Nein — schwache Cipher oder unsichere Protokolle (SSLv2/v3) sind **aktiviert**  

### Sind administrative oder Management-Schnittstellen auf autorisierte Netzwerke beschränkt?
- [ ] Ja — Schnittstellen (z. B. SSH, RDP, Control Panels) sind aus dem öffentlichen Internet **nicht erreichbar**  
- [ ] Nein — Schnittstellen sind öffentlich **erreichbar**, erfordern jedoch eine Multi-Faktor-Authentifizierung  
- [ ] Nein — Schnittstellen sind öffentlich **erreichbar** mit Ein-Faktor-Authentifizierung oder Standard-Zugangsdaten (Default Credentials)

---

## WSTG-CONF-02 — Test der Konfiguration der Anwendungsplattform

Der Test der Konfiguration der Anwendungsplattform umfasst die Prüfung des zugrunde liegenden Webservers, Anwendungsservers und der Framework-Einstellungen, um sicherzustellen, dass diese gegen gängige Exploits gehärtet sind. Angreifer suchen gezielt nach Standardzugangsdaten (Default Credentials), ungepatchten Plattform-Schwachstellen und exponierten administrativen Schnittstellen, um unbefugten Zugriff zu erlangen oder Code auszuführen. Fehlkonfigurationen führen häufig zur Offenlegung sensibler Umgebungsvariablen, interner Systempfade oder zum Vorhandensein unnötiger Beispielanwendungen, welche die Angriffsfläche (Attack Surface) vergrößern. Aus professioneller Sicht dient eine schlecht konfigurierte Plattform als Einstiegspunkt mit hoher Erfolgswahrscheinlichkeit, um initialen Zugriff auf die Zielumgebung zu erhalten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn administrative Schnittstellen mit Standardzugangsdaten zugänglich sind oder wenn Beispielskripte eine Remote Code Execution (RCE) ermöglichen.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Sind Standardzugangsdaten oder -konten auf der Plattform aktiv?
- [ ] Nein — alle Standardkonten sind **deaktiviert** oder die Passwörter wurden geändert  
- [ ] Ja — Standardkonten existieren, sind aber **nicht** aus dem externen Netzwerk **erreichbar**  
- [ ] Ja — Standardkonten sind **aktiv** und über das Internet zugänglich *(Kritisch)*  

### Gibt die Plattform sensible Versionsinformationen über Header oder Fehlerseiten preis?
- [ ] Nein — Versions-Strings sind **verborgen** oder generisch  
- [ ] Ja — detaillierte Versionsinformationen **werden** in HTTP-Headern **offengelegt** (z. B. `Server`, `X-Powered-By`)  
- [ ] Ja — vollständige Stack-Traces und interne Systempfade **werden** in ausführlichen Fehlermeldungen **exponiert**  

### Sind administrative oder Management-Schnittstellen ordnungsgemäß eingeschränkt?
- [ ] Nein — administrative Schnittstellen sind **nicht exponiert**  
- [ ] Ja — Schnittstellen existieren, sind aber durch IP-Allowlisting oder Multi-Faktor-Authentifizierung (MFA) **eingeschränkt**  
- [ ] Ja — Schnittstellen (z. B. `/admin`, `/manager`, `/console`) sind mit Single-Factor-Authentifizierung **öffentlich zugänglich**  

### Sind Beispieldateien, Skripte oder Dokumentationen auf dem Produktivserver vorhanden?
- [ ] Nein — alle nicht essenziellen Dateien wurden **entfernt**  
- [ ] Ja — Beispieldateien oder Dokumentationen **sind vorhanden**, legen aber keine sensiblen Informationen offen  
- [ ] Ja — Beispielskripte (z. B. `info.php`, `examples/`) **sind vorhanden** und stellen einen direkten Angriffsvektor dar  

### Ist der Server so konfiguriert, dass er unsichere HTTP-Methoden unterstützt?
- [ ] Nein — nur `GET` und `POST` sind **aktiviert**  
- [ ] Ja — potenziell riskante Methoden wie `PUT`, `DELETE` oder `TRACE` sind **aktiviert**, aber eingeschränkt  
- [ ] Ja — riskante Methoden sind **aktiviert** und ermöglichen unbefugte Dateimanipulationen oder den Diebstahl von Zugangsdaten

---

## WSTG-CONF-03 — Überprüfung der Dateierweiterungsverarbeitung auf sensible Informationen

Die Überprüfung der Verarbeitung von Dateierweiterungen beinhaltet die Identifizierung, ob der Webserver oder Applikationsserver sensible Informationen preisgibt, indem er Dateien ausliefert, die eingeschränkt oder ausgeführt werden sollten. Angreifer suchen häufig nach Backup-Dateien, Konfigurationsdateien und Quellcode-Fragmenten (z. B. `.bak`, `.old`, `.env`, `.inc`), die im Web-Root verbleiben und aufgrund fehlender spezifischer Handler Mappings als Klartext ausgeliefert werden könnten. Diese Schwachstelle ist von Bedeutung, da sie zur Offenlegung von Datenbank-Anmeldedaten, API-Keys und interner Geschäftslogik führen kann, was eine tiefergehende Exploitation der Infrastruktur erleichtert. Diese Expositionen treten typischerweise in öffentlichen Verzeichnissen, Upload-Ordnern oder durch falsch konfigurierte MIME-Type-Einstellungen auf dem Server auf.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn Konfigurationsdateien mit Anmeldedaten oder der vollständige Quellcode zugänglich sind.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Sind sensible Backup- oder temporäre Dateierweiterungen (z. B. `.bak`, `.old`, `.swp`, `~`) zugänglich?
- [ ] Nein — Der Server gibt 403 Forbidden oder 404 Not Found für gängige Backup-Erweiterungen zurück  
- [ ] Ja — Der Server erlaubt das Herunterladen von Backup-Dateien, diese enthalten jedoch **keine** sensiblen Daten  
- [ ] Ja — Der Server erlaubt das Herunterladen von Backup-Dateien, die sensible Daten oder Quellcode enthalten *(Hoch)*  

### Gibt der Server Umgebungs- oder Systemkonfigurationsdateien preis (z. B. `.env`, `.config`, `.yml`, `.ini`)?
- [ ] Nein — Auf eingeschränkte Konfigurationsdateien **kann nicht** zugegriffen werden; es wird 403/404 zurückgegeben  
- [ ] Ja — Sensible Konfigurationsdateien **sind** zugänglich und legen interne Geheimnisse oder Anmeldedaten offen  

### Kann Quellcode durch das Anhängen alternativer Erweiterungen abgerufen werden (z. B. `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] Nein — Applikationscode wird **nicht** als Klartext gerendert, unabhängig von Manipulationen der Erweiterung  
- [ ] Ja — Quellcode wird offengelegt, da der Server **keinen** Handler für die angehängte Erweiterung besitzt  

### Sind administrative Verzeichnisse oder Metadaten-Verzeichnisse (z. B. `.git/`, `.svn/`, `.DS_Store`) vor öffentlichem Zugriff geschützt?
- [ ] Nein — Diese Verzeichnisse/Dateien existieren **nicht** im Web-Root  
- [ ] Ja — Ein Zugriff ist aufgrund von serverseitigen Rewrite-Rules oder Berechtigungen **nicht möglich**  
- [ ] Ja — Metadaten-Verzeichnisse **sind** zugänglich und ermöglichen eine vollständige Rekonstruktion des Quellcodes  

### Wie verarbeitet der Server Anfragen für Dateien mit mehreren Erweiterungen (z. B. `file.php.jpg`)?
- [ ] Nein — Der Server priorisiert die Sicherheit der internen Erweiterung korrekt oder blockiert die Anfrage  
- [ ] Ja — Der Server verarbeitet die Datei gemäß der ersten Erweiterung, aber ein Bypass zur Ausführung ist **nicht möglich**  
- [ ] Ja — Der Server ignoriert die letzte Erweiterung, wodurch Code Execution oder Source Disclosure **möglich ist**

---

## WSTG-CONF-04 — Überprüfung alter Backup-Dateien und nicht referenzierter Dateien auf sensible Informationen

Die Überprüfung alter Backup-Dateien und nicht referenzierter Dateien umfasst die Identifizierung vergessener, temporärer oder versteckter Dateien auf einem Webserver, die nicht für den öffentlichen Zugriff bestimmt sind. Diese Dateien enthalten häufig Quellcode-Backups (`.zip`, `.bak`), Texteditor-Swap-Dateien (`.swp`, `~`) oder Versionskontroll-Metadaten (`.git`, `.svn`), die sensible Informationen preisgeben können. Angreifer verwenden automatisierte Wordlists und Discovery-Tools, um gängige Namenskonventionen per Brute-Force anzugreifen, mit dem Ziel, Datenbank-Credentials, fest codierte API-Keys oder Logiken zu exfiltrieren, die weitere Angriffe (Exploitation) erleichtern. Diese Schwachstelle resultiert typischerweise aus manueller Serverwartung oder fehlerhaften CI/CD-Pipelines, bei denen die Bereinigung des Production Web-Root fehlschlägt.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel / Hoch* |

> *Die Kritikalität wird als „Hoch“ eingestuft, wenn Konfigurationsdateien, Quellcode-Archive oder Credentials entdeckt werden.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Ist das Directory Listing auf dem Webserver aktiviert?
- [ ] Nein — Directory Listing ist **deaktiviert** und gibt einen 403 Forbidden oder einen benutzerdefinierten Fehler zurück  
- [ ] Ja — Directory Listing ist in nicht sensiblen Verzeichnissen **aktiviert**  
- [ ] Ja — Directory Listing ist in Verzeichnissen mit Quellcode oder sensiblen Dateien **aktiviert** *(Kritisch)*  

### Sind Backup-Dateien mit gängigen Erweiterungen (z. B. .bak, .old, .save) auffindbar?
- [ ] Nein — gängige Backup-Erweiterungen wurden **nicht gefunden** oder werden durch die Serverkonfiguration blockiert  
- [ ] Ja — Backup-Dateien existieren, enthalten aber **keine** sensiblen Informationen  
- [ ] Ja — Backup-Dateien für Konfigurationen oder Quellcode **sind** zugänglich *(Hoch)*  

### Sind Versionskontrollverzeichnisse (z. B. .git, .svn) oder Metadaten exponiert?
- [ ] Nein — Versionskontroll-Metadaten sind **nicht vorhanden** oder korrekt blockiert  
- [ ] Ja — Metadaten existieren, aber das vollständige Repository **kann nicht** rekonstruiert werden  
- [ ] Ja — das gesamte Quellcode-Repository **kann** über exponierte Metadaten exfiltriert werden  

### Befinden sich komprimierte Archive (z. B. .zip, .tar.gz, .rar) der Anwendung im Web-Root?
- [ ] Nein — es wurden keine sensiblen Archivdateien durch Brute-Force oder Enumeration entdeckt  
- [ ] Ja — Archive wurden gefunden, sind aber passwortgeschützt oder enthalten öffentliche Assets  
- [ ] Ja — unverschlüsselte Archive mit Anwendungsquellcode oder Daten **sind** öffentlich zugänglich  

### Gibt die Anwendung temporäre Dateien preis, die von Texteditoren oder IDEs erstellt wurden?
- [ ] Nein — temporäre Dateien wie `.swp`, `~` oder `.DS_Store` sind **nicht vorhanden**  
- [ ] Ja — nicht sensible temporäre Dateien sind **vorhanden**  
- [ ] Ja — temporäre Dateien legen Quellcode-Ausschnitte oder interne Pfade offen

---

## WSTG-CONF-05 — Aufzählung von Administrationsschnittstellen für Infrastruktur und Anwendungen

Administrationsschnittstellen stellen die Verwaltungsebene einer Anwendung und der zugrunde liegenden Infrastruktur dar und gewähren häufig privilegierten Zugriff auf Systemkonfigurationen, Benutzerdaten und serverseitige Vorgänge. Angreifer priorisieren die Aufzählung dieser Schnittstellen durch Directory Brute-Forcing, die Analyse der `robots.txt` oder das Beobachten vorhersehbarer URL-Muster, um Einstiegspunkte zu finden, denen es an robuster Authentifizierung mangelt oder die auf Standard-Zugangsdaten (Default Credentials) basieren. Die Entdeckung eines exponierten Admin-Panels erhöht das Risiko einer vollständigen Systemkompromittierung, unbefugter Datenmanipulation oder Dienstunterbrechung erheblich. Diese Schnittstellen befinden sich häufig auf Nicht-Standard-Ports oder Subdomains, was sie zu primären Zielen für automatisierte Scanner und manuelle Rekonstruktion (Reconnaissance) macht.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn die Schnittstelle systemweite Konfigurationsänderungen, Benutzerverwaltung oder Datenexfiltration ohne Multi-Faktor-Authentifizierung (MFA) ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Sind Administrationsschnittstellen durch Directory Fuzzing oder Rekonstruktion auffindbar?
- [ ] Nein — es sind keine Administrationsschnittstellen exponiert oder auffindbar  
- [ ] Ja — Schnittstellen sind auffindbar, aber der Zugriff **ist auf interne IP-Bereiche beschränkt**  
- [ ] Ja — Schnittstellen **sind** auffindbar und öffentlich zugänglich  

### Wird die Authentifizierung an den entdeckten Administrationsportalen erzwungen?
- [ ] Ja — Multi-Faktor-Authentifizierung (MFA) **wird erzwungen**  
- [ ] Ja — Ein-Faktor-Authentifizierung (SFA) **wird erzwungen**  
- [ ] Nein — für den Zugriff auf die Schnittstelle ist keine Authentifizierung erforderlich *(Kritisch)*  

### Sind Administrationspfade versteckt oder verwenden sie Nicht-Standard-Bezeichnungen, um die Entdeckung zu verhindern?
- [ ] Ja — Pfade verwenden Nicht-Standard-Bezeichnungen, zufällige oder nicht vorhersehbare Namen  
- [ ] Nein — Pfade verwenden gängige Namenskonventionen (z. B. `/admin`, `/manager`, `/console`) und **können** leicht erraten werden  

### Bietet die Schnittstelle Zugriff auf sensible System- oder Anwendungsfunktionen?
- [ ] Nein — die Schnittstelle ist ein „schreibgeschütztes“ (Read-only) Status-Dashboard mit minimalen Auswirkungen  
- [ ] Ja — die Schnittstelle **kann** die Anwendungskonfiguration oder Benutzerdaten ändern  
- [ ] Ja — die Schnittstelle **kann** Befehle auf Systemebene ausführen oder die Datenbankverwaltung übernehmen

---

## WSTG-CONF-06 — Testen von HTTP-Methoden

Das Testen von HTTP-Methoden beinhaltet die Identifizierung der vom Webserver und der Anwendung unterstützten Verben, um sicherzustellen, dass nur notwendige Funktionen freigegeben sind. Über die Standardmethoden `GET` und `POST` hinaus können Server unbeabsichtigt gefährliche Methoden wie `PUT`, `DELETE` oder `TRACE` aktivieren. Diese können es Angreifern ermöglichen, bösartige Dateien hochzuladen, bestehende Inhalte zu löschen oder Session-Cookies via Cross-Site Tracing (XST) zu exfiltrieren. Pentesters untersuchen Antwort-Header wie `Allow` oder `Public` und versuchen, Beschränkungen mittels Method Overriding-Techniken oder Verb Tampering zu umgehen, um auf nicht autorisierte Funktionen zuzugreifen. Dieser Test ist entscheidend für die Härtung der Angriffsoberfläche und zur Vermeidung von Fehlkonfigurationen, die zu einer Kompromittierung des Servers oder unbefugter Datenmanipulation führen könnten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn `PUT` oder `DELETE` eine unbefugte Dateimanipulation ermöglichen oder wenn `TRACE` die Exfiltration von Session-Token erleichtert.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Sind gefährliche HTTP-Methoden wie `PUT` oder `DELETE` anwendungsweit deaktiviert?
- [ ] Ja — nur sichere Methoden (GET, POST, HEAD) **sind aktiviert**  
- [ ] Nein — `PUT` oder `DELETE` **sind aktiviert**, erfordern aber eine gültige Authentifizierung  
- [ ] Nein — `PUT` oder `DELETE` **sind aktiviert** und ohne Authentifizierung zugänglich *(Kritisch)*  

### Ist die `TRACE`-Methode deaktiviert, um Cross-Site Tracing (XST) zu verhindern?
- [ ] Ja — die `TRACE`-Methode **ist deaktiviert** oder gibt einen 405 Method Not Allowed zurück  
- [ ] Nein — die `TRACE`-Methode **ist aktiviert**, spiegelt aber keine sensiblen Header wider  
- [ ] Nein — die `TRACE`-Methode **ist aktiviert** und spiegelt `Cookie`- oder `Authorization`-Header wider *(Mittel)*  

### Wird HTTP Method Overriding (z. B. `X-HTTP-Method-Override`) vom Server unterstützt?
- [ ] Nein — der Server verarbeitet **keine** Method-Override-Header  
- [ ] Ja — der Server verarbeitet Override-Header, aber Zugriffskontrollen **werden erzwungen**  
- [ ] Ja — der Server verarbeitet Override-Header und ein Bypass der Zugriffskontrolle **ist möglich**  

### Reagiert der Server sicher auf beliebige oder falsch formatierte HTTP-Methoden?
- [ ] Ja — der Server gibt 405 Method Not Allowed oder 501 Not Implemented zurück  
- [ ] Nein — der Server gibt 200 OK oder 500 Error für beliebige Methoden wie `JEFF` oder `TEST` zurück  
- [ ] Nein — die Verwendung beliebiger Methoden ermöglicht das Umgehen von Authentifizierungs- oder Autorisierungsfiltern  

### Ist die `OPTIONS`-Methode eingeschränkt oder so konfiguriert, dass die Offenlegung von Informationen minimiert wird?
- [ ] Ja — `OPTIONS` ist deaktiviert oder gibt einen minimalen `Allow`-Header zurück  
- [ ] Nein — `OPTIONS` legt eine breite Palette aktivierter Methoden offen, einschließlich DAV oder Debugging-Verben

---

## WSTG-CONF-07 — Test HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) ist ein Sicherheitsmechanismus, der es einem Webserver ermöglicht, Browsern mitzuteilen, dass der Zugriff nur über HTTPS erfolgen soll. Dies verhindert Angriffe durch Protokoll-Downgrades und Cookie-Hijacking. Durch die Erzwingung einer verschlüsselten Verbindung mildert HSTS Man-in-the-Middle (MitM)-Angriffe wie SSLStrip ab, bei denen ein Angreifer eine anfängliche unsichere HTTP-Anfrage abfängt, um den Übergang zu TLS zu verhindern. Aus der Sicht eines Angreifers bietet das Fehlen dieses Headers oder eine kurze `max-age`-Dauer ein Zeitfenster, um sensible Session-Token oder Anmeldedaten während des initialen Klartext-Handshake abzufangen. Dieser Test konzentriert sich auf die Überprüfung des Vorhandenseins, der Dauer und des Geltungsbereichs des `Strict-Transport-Security`-Headers über alle sensiblen Endpunkte der Anwendung hinweg.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel* |

> *Der Schweregrad wird als "Mittel" eingestuft, wenn die Anwendung PII (personenbezogene Daten), Session-Token oder Anmeldedaten verarbeitet, da das Fehlen von HSTS die Erfolgsquote von MitM-Angriffen erhöht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### Ist der `Strict-Transport-Security`-Header in HTTPS-Antworten vorhanden?
- [ ] Ja — Header **ist auf allen sensiblen Endpunkten vorhanden**  
- [ ] Ja — Header **ist vorhanden**, aber nur auf der Startseite  
- [ ] Nein — Header **fehlt** in der gesamten Anwendung vollständig  

### Ist die `max-age`-Direktive auf eine ausreichende Dauer eingestellt?
- [ ] Ja — `max-age` ist auf ein Jahr oder länger eingestellt (z. B. `31536000`)  
- [ ] Ja — `max-age` ist eingestellt, aber **zu kurz** (z. B. weniger als 6 Monate)  
- [ ] Nein — `max-age`-Direktive **fehlt** oder ist auf `0` gesetzt *(Kritisch)*  

### Deckt die HSTS-Richtlinie alle Subdomains ab?
- [ ] Ja — `includeSubDomains`-Direktive **wird angewendet**  
- [ ] Nein — `includeSubDomains`-Direktive **fehlt**, wodurch Subdomains anfällig für Downgrades bleiben  
- [ ] Nein — Funktion trifft nicht zu, da keine Subdomains existieren  

### Ist die Anwendung für HSTS Preloading registriert?
- [ ] Ja — `preload`-Direktive **ist vorhanden** und die Domain befindet sich in der HSTS-Preload-Liste  
- [ ] Ja — `preload`-Direktive **ist vorhanden**, aber die Domain ist **noch nicht** in der Preload-Liste  
- [ ] Nein — `preload`-Direktive **fehlt**, was ein Zeitfenster für MitM beim ersten Besuch ermöglicht  

### Wird der HSTS-Header fälschlicherweise über Klartext-HTTP gesendet?
- [ ] Nein — Header wird **nur** über sichere HTTPS-Verbindungen gesendet  
- [ ] Ja — Header **wird über HTTP gesendet**, was von Browsern ignoriert wird und Konfigurationsdetails preisgeben kann

---

## WSTG-CONF-08 — Test RIA Cross Domain Policy

Cross-Domain-Richtlinien für Rich Internet Applications (RIA) umfassen Dateien wie `crossdomain.xml` und `clientaccesspolicy.xml`, die festlegen, wie Web-Clients – insbesondere Adobe Flash und Microsoft Silverlight – Cross-Origin-Anfragen verarbeiten. Diese Dateien sind sicherheitskritisch, da sie die Same-Origin Policy (SOP) explizit außer Kraft setzen und bestimmen, welche externen Domänen mit der Anwendung interagieren und deren Antworten lesen dürfen. Zu permissiv konfigurierte Richtlinien, wie die Verwendung von Wildcards im `domain`-Attribut, ermöglichen es Angreifern, bösartige RIA-Objekte auf einer Drittanbieter-Website zu hosten, die sensible Daten exfiltrieren oder Aktionen im Namen authentifizierter Benutzer durchführen können. Pentesters suchen diese Dateien in der Regel im Web-Root-Verzeichnis, um den Grad des Vertrauens gegenüber Drittanbieter-Origins zu bewerten und potenzielle Cross-Origin-Datenlecks zu identifizieren.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn die Anwendung sensible Benutzerdaten oder Session-Identifier verarbeitet, die über eine permissive Richtlinie exfiltriert werden können.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Sind Cross-Domain-Richtliniendateien (`crossdomain.xml` oder `clientaccesspolicy.xml`) im Web-Root vorhanden?
- [ ] Nein — Dateien können im Root-Verzeichnis **nicht** gefunden werden  
- [ ] Ja — `crossdomain.xml` (Flash) **ist** vorhanden  
- [ ] Ja — `clientaccesspolicy.xml` (Silverlight) **ist** vorhanden  
- [ ] Ja — beide RIA-Richtliniendateien **sind** vorhanden  

### Ist das Domain-Attribut `allow-access-from` auf vertrauenswürdige Origins beschränkt?
- [ ] Nein — Dateien existieren, enthalten aber keine `allow-access-from`-Einträge  
- [ ] Ja — spezifische, vertrauenswürdige Domänen sind explizit gewhitelistet und ein Bypass ist **nicht möglich**  
- [ ] Ja — Domänen sind gewhitelistet, enthalten aber nicht vertrauenswürdige Drittanbieter oder Subdomänen  
- [ ] Ja — ein Wildcard `*` **wird verwendet**, was den Zugriff von **jeder** Origin erlaubt *(Kritisch)*  

### Erlaubt die Richtlinie unsichere Kommunikation über HTTP für HTTPS-Ressourcen?
- [ ] Nein — `secure="true"` ist gesetzt (oder Standard), was HTTP-Origins daran hindert, auf HTTPS-Daten zuzugreifen  
- [ ] Ja — `secure="false"` ist explizit gesetzt, was MITM-Angreifern die Exfiltration sicherer Daten ermöglicht  

### Sind HTTP-Header in der Cross-Domain-Konfiguration zu permissiv?
- [ ] Nein — `allow-http-request-headers-from` ist eingeschränkt oder **nicht aktiviert**  
- [ ] Ja — spezifische Header sind für vertrauenswürdige Domänen erlaubt  
- [ ] Ja — ein Wildcard `*` wird für Header verwendet, was es Angreifern ermöglicht, benutzerdefinierte Header von jeder Domäne aus zu senden  

### Ist die `site-control` (Master-Policy) Konfiguration restriktiv?
- [ ] Nein — `permitted-cross-domain-policies` ist auf `none` oder `master-only` gesetzt  
- [ ] Ja — die Richtlinie erlaubt es anderen Dateien auf dem Server, eigene Regeln zu definieren (`all`)

---

## WSTG-CONF-09 — Berechtigungen von Dateien prüfen

Die Überprüfung von Dateiberechtigungen umfasst die Auditierung der Zugriffskontrollebenen, die Dateien und Verzeichnissen auf dem Webserver zugewiesen sind, um sicherzustellen, dass sensible Ressourcen nicht unbefugten Benutzern oder Prozessen gegenüber exponiert werden. Falsch konfigurierte Berechtigungen können es Angreifern ermöglichen, sensible Konfigurationsdateien mit Datenbank-Anmeldedaten (Credentials) zu lesen, proprietären Quellcode einzusehen oder serverseitige Skripte zu modifizieren, um eine Remote Code Execution zu erreichen. Diese Schwachstelle tritt typischerweise während der Deployment-Phase auf, wenn standardmäßige "world-readable"- oder "world-writable"-Flags auf kritischen Anwendungsverzeichnissen wie Konfigurationspfaden, Log-Ordnern oder Upload-Verzeichnissen belassen werden. Aus der Sicht eines Angreifers dient das Entdecken einer unzureichend gesicherten Datei oft als primärer Katalysator für eine Privilege Escalation oder eine vollständige Systemkompromittierung.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel / Hoch* |

> *Die Kritikalität wird als "Hoch" eingestuft, wenn sensible Konfigurationsdateien (z. B. `.env`, `web.config`) lesbar sind oder wenn Schreibzugriff auf das Web-Root möglich ist.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Werkzeuge:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Sind sensible Konfigurationsdateien der Anwendung vor unbefugtem Zugriff geschützt?
- [ ] Ja — Konfigurationsdateien (z. B. `.env`, `config.php`) sind **nicht** über Web-Anfragen zugänglich  
- [ ] Ja — Dateien sind vorhanden, aber der Zugriff ist **nur** auf autorisierte lokale Benutzer beschränkt  
- [ ] Nein — sensible Konfigurationsdateien **sind zugänglich** und legen Credentials oder Geheimnisse offen  

### Kann ein Angreifer Dateien oder Verzeichnisse innerhalb des Web-Roots modifizieren?
- [ ] Nein — alle Web-Root-Verzeichnisse sind für den Webserver-Benutzer **schreibgeschützt** (read-only)  
- [ ] Ja — beschreibbare Verzeichnisse existieren, aber die Skriptausführung ist **deaktiviert**  
- [ ] Ja — "world-writable" Verzeichnisse existieren und **ermöglichen** das Hochladen und Ausführen von Skripten *(Kritisch)*  

### Sind Metadaten der Versionsverwaltung oder Backup-Dateien aufgrund laxer Berechtigungen exponiert?
- [ ] Nein — `.git`, `.svn` und Backup-Dateien (z. B. `.bak`, `~`) sind **nicht vorhanden** oder geschützt  
- [ ] Ja — Metadaten-Verzeichnisse existieren, aber das Directory Listing ist **deaktiviert**  
- [ ] Ja — sensible Metadaten oder Backups sind **vollständig zugänglich**, was die Wiederherstellung des Quellcodes ermöglicht  

### Agiert der Webserver-Benutzer nach dem Prinzip der minimalen Berechtigung (Principle of Least Privilege)?
- [ ] Ja — der Webserver-Prozess läuft als ein **dedizierter Benutzer mit geringen Berechtigungen** (low-privilege)  
- [ ] Nein — der Webserver-Prozess läuft mit **unnötigen Berechtigungen** (z. B. `root` oder `Administrator`)  

### Sind Log-Dateien gegen unbefugtes Lesen oder Modifizieren gesichert?
- [ ] Ja — Log-Dateien sind auf administrative Benutzer **beschränkt**  
- [ ] Ja — Log-Dateien sind lesbar, enthalten aber **keine** Session-Tokens oder PII  
- [ ] Nein — Log-Dateien sind **öffentlich lesbar** und legen sensible Transaktionsdaten oder Benutzerinformationen offen

---

## WSTG-CONF-10 — Subdomain Takeover

Ein Subdomain Takeover tritt auf, wenn ein DNS-Eintrag (in der Regel ein CNAME, gelegentlich aber auch A- oder MX-Einträge) auf eine stillgelegte oder nicht existierende Ressource bei einem Drittanbieter von Cloud- oder Hosting-Diensten verweist. Angreifer identifizieren diese „dangling“ (verwaisten) DNS-Einträge, indem sie nach spezifischen Fehlermeldungen von Providern wie AWS, GitHub oder Azure suchen, die darauf hinweisen, dass eine Ressource nicht mehr beansprucht wird. Durch die Registrierung des aufgegebenen Ressourcennamens auf der Plattform des Anbieters erlangt der Angreifer die volle Kontrolle über die Subdomain. Dies ermöglicht es ihm, schädliche Inhalte zu hosten, Sitzungscookies (Session Cookies), die für die Basisdomain (Base Domain) gültig sind, zu exfiltrieren und Content Security Policy (CSP) oder CORS-Schutzmechanismen zu umgehen. Diese Schwachstelle ist besonders gefährlich, da sie das Vertrauen in die legitime Domain-Struktur der Organisation ausnutzt, um Phishing mit hoher Auswirkung und Datendiebstahl zu erleichtern.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch* |

> *Der Schweregrad wird als Kritisch eingestuft, wenn die Subdomain zum Hijacking von Sitzungscookies der Basisdomain oder zum Umgehen von SSO/OAuth-Redirects verwendet werden kann.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Tools:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### Nutzt die Anwendung Drittanbieter-Hosting oder Cloud-Dienste über Subdomains?
- [ ] Nein — alle Subdomains verweisen auf IP-Bereiche, die von der Organisation kontrolliert werden  
- [ ] Ja — Subdomains verweisen auf Drittanbieter (S3, GitHub Pages, Heroku, etc.)  

### Gibt es „dangling“ DNS-Einträge, die auf nicht existierende Ressourcen verweisen?
- [ ] Nein — alle identifizierten DNS-Einträge lösen auf aktive, kontrollierte Ressourcen auf  
- [ ] Ja — CNAME- oder A-Einträge existieren für Ressourcen, die beim Anbieter **nicht mehr existieren**  
- [ ] Ja — DNS-Einträge lösen zu NXDOMAIN oder anbieterspezifischen Fehlermeldungen auf *(z. B. „NoSuchBucket“)*  

### Kann die identifizierte verwaiste Ressource erfolgreich beansprucht werden?
- [ ] Nein — der Anbieter verfügt über Schutzmechanismen gegen die Beanspruchung aufgegebener Namen oder eine Kontoverifizierung ist erforderlich  
- [ ] Ja — der Ressourcenname **kann** von jedem Benutzer auf der Plattform des Drittanbieters registriert werden  

### Verwendet die Basisdomain Cookies mit „Domain“-Scope, die kompromittiert werden könnten?
- [ ] Nein — Cookies sind strikt auf spezifische Subdomains beschränkt oder verwenden `Host-Only`-Flags  
- [ ] Ja — sensible Cookies (Sitzungs-IDs, JWTs) sind für die übergeordnete Domain gültig und **können** über eine übernommenen Subdomain exfiltriert werden  

### Kann die Subdomain zur Umgehung von Security-Headern oder Authentifizierungsabläufen verwendet werden?
- [ ] Nein — keine Sicherheitsabhängigkeiten von den identifizierten Subdomains  
- [ ] Ja — die Subdomain steht auf der Whitelist in CSP `script-src` oder `connect-src` Direktiven  
- [ ] Ja — die Subdomain ist eine gültige Redirect-URI für OAuth- oder SSO-Konfigurationen

---

## WSTG-CONF-11 — Cloud-Speicher prüfen

Die Prüfung auf Fehlkonfigurationen von Cloud-Speichern umfasst die Identifizierung und Überprüfung öffentlich zugänglicher Object Storage Services wie Amazon S3, Azure Blobs oder Google Cloud Storage. Diese Ressourcen enthalten häufig sensible Daten wie Backups, Quellcode, PII (Personally Identifiable Information) oder Konfigurationsdateien, die aufgrund zu permissiver Access Control Lists (ACLs) oder Identity and Access Management (IAM) Policies offengelegt werden. Angreifer enumerieren Bucket-Namen durch Reconnaissance von JavaScript-Dateien, HTML-Quellcode und Brute-Force-Techniken, um Einstiegspunkte für Data Exfiltration oder unbefugte Dateiänderungen zu finden. Die Ausnutzung (Exploitation) kann erhebliche Auswirkungen haben, einschließlich vollständiger Datenschutzverletzungen (Data Breaches) oder der Möglichkeit, bösartige Dateien über eine vertrauenswürdige Domain bereitzustellen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch* |

> *Der Schweregrad wird als Kritisch eingestuft, wenn PII, Anmeldedaten oder Produktions-Backups zugänglich sind oder wenn ein nicht authentifizierter Schreibzugriff aktiviert ist.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Werden Cloud-Storage-Endpunkte (Buckets/Container) identifiziert und enumeriert?
- [ ] Nein — es werden keine Cloud-Storage-Endpunkte verwendet oder diese sind nicht auffindbar  
- [ ] Ja — Storage-Endpunkte werden durch Code-Analyse oder Traffic-Monitoring identifiziert  
- [ ] Ja — Storage-Endpunkte werden durch Brute-Force auf Namenskonventionen entdeckt  

### Können nicht authentifizierte Benutzer den Inhalt des Cloud-Speichers auflisten (Listing)?
- [ ] Nein — Bucket-Listing ist **deaktiviert** und gibt 403 Forbidden oder 404 Not Found zurück  
- [ ] Ja — Listing ist **aktiviert**, aber es sind keine sensiblen Dateien vorhanden  
- [ ] Ja — Listing ist **aktiviert** und sensible Dateinamen/Metadaten sind sichtbar  

### Ist das Herunterladen sensibler Dateien von identifizierten Buckets möglich?
- [ ] Nein — Dateien sind durch IAM-Policies geschützt oder eine gültige Authentifizierung ist erforderlich  
- [ ] Ja — Dateien sind zugänglich, erfordern jedoch eine Signed URL, die **nicht** einfach erraten werden kann  
- [ ] Ja — sensible Dateien (Backups, `.env`, PII) sind für den Download **öffentlich zugänglich** *(Kritisch)*  

### Ist ein nicht authentifizierter Dateiupload oder eine Dateiänderung zulässig?
- [ ] Nein — nicht authentifizierte Uploads oder Änderungen sind **nicht möglich**  
- [ ] Ja — ein authentifizierter Upload ist erforderlich, aber ein Bypass **ist möglich** über schwache Policy-Bedingungen  
- [ ] Ja — nicht authentifiziertes Schreiben, Überschreiben oder Löschen von Dateien ist **möglich** *(Kritisch)*  

### Sind die Cross-Origin Resource Sharing (CORS) Policies am Storage-Endpunkt restriktiv?
- [ ] Ja — CORS ist **deaktiviert** oder auf bestimmte vertrauenswürdige Ursprünge (Origins) beschränkt  
- [ ] Nein — CORS ist mit einer Wildcard `*` Origin **aktiviert**, was unbefugten webbasierten Zugriff ermöglicht

---

## WSTG-CONF-12 — Content Security Policy (CSP)

Content Security Policy (CSP) ist ein entscheidender Defense-in-Depth-Mechanismus, der über HTTP-Response-Header implementiert wird, um Risiken wie Cross-Site Scripting (XSS), Clickjacking und Data Injection Attacks zu minimieren. Durch die Definition zulässiger dynamischer Ressourcen schränkt eine gut konfigurierte CSP die Fähigkeit eines Angreifers ein, nicht autorisierte Skripte auszuführen oder sensible Daten an externe Domänen zu exfiltrieren. Pentesters bewerten die Policy auf zu permissive Direktiven, die Verwendung unsicherer Schlüsselwörter wie `'unsafe-inline'` und die Abhängigkeit von unsicheren CDNs, die Bypasses ermöglichen könnten. Eine schwache oder fehlende CSP stellt nicht direkt eine Schwachstelle dar, erhöht jedoch die Auswirkungen erfolgreicher Injection-Angriffe erheblich, da eine zusätzliche Schutzschicht fehlt.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Tools:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Ist ein Content-Security-Policy (CSP) Header in den Anwendungsantworten vorhanden?
- [ ] Ja — `Content-Security-Policy`-Header ist **vorhanden** und wird **erzwungen**  
- [ ] Ja — `Content-Security-Policy-Report-Only` ist zu Testzwecken **vorhanden**  
- [ ] Nein — kein CSP-Header in den Antworten **vorhanden**  

### Sind die Direktiven `script-src` oder `default-src` ordnungsgemäß eingeschränkt?
- [ ] Ja — Direktiven verwenden striktes Whitelisting, Nonces oder Hashes; ein Bypass ist **nicht möglich**  
- [ ] Ja — Direktiven sind **korrekt konfiguriert**, aber die Whitelist enthält bekannte, umgehbare CDNs  
- [ ] Nein — Direktiven verwenden Wildcards (`*`) oder `data:`-Schemata, wodurch ein Bypass **möglich** ist  

### Erlaubt die Policy die Ausführung von unsicheren Inline-Skripten oder eval?
- [ ] Nein — `'unsafe-inline'` und `'unsafe-eval'` sind **nicht vorhanden**  
- [ ] Ja — `'unsafe-inline'` ist **vorhanden**, aber durch eine `nonce` oder einen `hash` geschützt  
- [ ] Ja — `'unsafe-inline'` oder `'unsafe-eval'` sind ohne zusätzliche Schutzmaßnahmen **aktiviert** *(Kritisch)*  

### Ist die Anwendung durch die Direktive `frame-ancestors` gegen Clickjacking geschützt?
- [ ] Ja — `frame-ancestors` ist auf `'none'` oder `'self'` gesetzt  
- [ ] Ja — `frame-ancestors` ist **aktiviert**, erlaubt jedoch spezifische vertrauenswürdige Drittanbieter-Domänen  
- [ ] Nein — `frame-ancestors` **fehlt**; es wird sich auf veraltete `X-Frame-Options` verlassen oder es besteht kein Schutz  

### Gibt es einen Mechanismus zur Meldung von CSP-Verletzungen?
- [ ] Ja — `report-uri` oder `report-to` ist **konfiguriert** und aktiv  
- [ ] Nein — Reporting von Verletzungen ist **deaktiviert** oder **nicht konfiguriert**

---

## WSTG-CONF-13 — Path Confusion

Path Confusion entsteht durch Diskrepanzen in der Art und Weise, wie verschiedene Web-Komponenten, wie zum Beispiel Reverse Proxies, Load Balancer und Backend-Applikationsserver, URL-Pfade parsen und interpretieren. Angreifer nutzen diese Inkonsistenzen aus, indem sie spezifische Zeichen wie Semikolons, kodierte Slashes oder Dot-Segmente injizieren, um Sicherheitsfilter zu täuschen und auf eingeschränkte Endpunkte oder statische Ressourcen zuzugreifen. Diese Schwachstelle kann zu kritischen Sicherheitsfehlern führen, einschließlich Authentication Bypass, unbefugtem Datenzugriff und Web Cache Poisoning, da das Front-End Sicherheitsregeln auf einen Pfad anwenden kann, den das Back-End anders interpretiert. Eine erfolgreiche Ausnutzung erfolgt in der Regel an der Architektur-Grenze, an der Request-Routing- und Normalisierungslogik innerhalb des Tech-Stacks divergieren.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Test Status** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn Path Confusion zu einem Bypass der Authentifizierung oder Zugriffskontrolle für sensible administrative Endpunkte führt.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Interpretieren verschiedene Architekturschichten Pfadtrennzeichen (z. B. `;`, `#`, `?`) konsistent?
- [ ] Ja — alle Schichten normalisieren und interpretieren Pfadtrennzeichen identisch  
- [ ] Nein — es existieren Inkonsistenzen, diese können jedoch **nicht** für den Zugriff auf eingeschränkte Ressourcen genutzt werden  
- [ ] Nein — Diskrepanzen ermöglichen es einem Angreifer, Front-End-Filter zu umgehen und die Back-End-Logik zu erreichen (**möglich**)  

### Können Zugriffskontrollen mittels Path Traversal Sequenzen oder kodierten Zeichen umgangen werden?
- [ ] Nein — die Normalisierung wird vor den Sicherheitsprüfungen konsistent angewendet  
- [ ] Ja — ein Bypass ist über Dot-Segment-Sequenzen (z. B. `/admin/..;/`) **möglich**  
- [ ] Ja — ein Bypass ist über URL-kodierte Zeichen (z. B. `%2f`, `%2e%2e%2f`) **möglich**  

### Ist die Anwendung anfällig für Web Cache Poisoning durch Path Confusion?
- [ ] Nein — die Caching-Logik wird nicht durch Pfad-Ambiguität oder zusätzliche Pfadinformationen beeinflusst  
- [ ] Ja — bösartige Inhalte **können** aufgrund von Mapping-Diskrepanzen für legitime Pfade zwischengespeichert werden  
- [ ] Ja — sensible Informationen **können** über `RCD` (Relative Path Overwrite) Techniken in öffentlichen Verzeichnissen zwischengespeichert werden  

### Verarbeitet der Server „zusätzliche Pfadinformationen“ (PathInfo) sicher?
- [ ] Nein — die Funktion ist **deaktiviert** oder nicht vorhanden  
- [ ] Ja — `PathInfo` wird verarbeitet und beeinträchtigt die Sicherheitsfilter **nicht**  
- [ ] Nein — `PathInfo` ermöglicht es einem Angreifer, Daten anzuhängen, welche die Routing-Logik der Anwendung korrumpieren

---

## WSTG-CONF-14 — Test auf Fehlkonfigurationen anderer HTTP-Security-Header

Die Prüfung auf Fehlkonfigurationen von HTTP-Security-Headern umfasst die Verifizierung des Vorhandenseins und der korrekten Implementierung defensiver Header, die einen wesentlichen Schutz gegen gängige webbasierte Angriffsvektoren bieten. Obwohl diese Header zugrunde liegende Code-Schwachstellen nicht beheben, erhöht ihr Fehlen das Risiko von Man-in-the-Middle (MITM)-Angriffen, MIME-sniffing exploitation und unbeabsichtigtem Cross-Origin-Datenabfluss via Referrers erheblich. Aus der Sicht eines Angreifers sind fehlende Security-Header deutliche Indikatoren für eine unzureichend gehärtete Umgebung, was oft komplexere Exploitation-Chains wie Session Hijacking oder Information Exfiltration erleichtert. Diese Konfigurationen werden in der Regel auf Serverebene evaluiert und sollten konsistent auf alle Antworttypen angewendet werden, einschließlich API-Endpunkte und statische Ressourcen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audit zum Vorhandensein von Security-Headern
| Header | Vorhanden | Fehlend | Empfohlene Konfiguration |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` or `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Funktionsspezifisch, z. B. `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Wie werden Security-Header über den gesamten Anwendungsumfang hinweg erzwungen?
- [ ] Ja — Header werden global über einen zentralen Load Balancer oder Reverse Proxy angewendet und **können nicht** überschrieben werden  
- [ ] Ja — Header werden auf Antworten von Hauptdokumenten angewendet, **fehlen** jedoch bei API-Antworten oder statischen Assets  
- [ ] Nein — Header werden inkonsistent angewendet oder **fehlen** in der gesamten Anwendungsdomäne  

### Ist der Strict-Transport-Security (HSTS) Header korrekt konfiguriert?
- [ ] Ja — `max-age` ist auf eine lange Dauer (z. B. 2 Jahre) eingestellt und `includeSubDomains` ist **aktiviert**  
- [ ] Ja — `max-age` ist gesetzt, aber `includeSubDomains` ist **deaktiviert**, wodurch Subdomänen verwundbar bleiben  
- [ ] Nein — Der HSTS-Header ist **nicht vorhanden** oder `max-age` ist auf 0 gesetzt, was Protocol Downgrade Attacks ermöglicht  

### Verhindert die Referrer-Policy den Abfluss sensibler URL-Parameter?
- [ ] Ja — Die Policy ist auf `no-referrer` oder `same-origin` gesetzt *(Am sichersten)*  
- [ ] Ja — Die Policy ist auf `strict-origin-when-cross-origin` gesetzt  
- [ ] Nein — Die Policy ist **nicht gesetzt** oder auf `unsafe-url` eingestellt, was potenziell Tokens oder sensible PII in Referer-Headern preisgibt  

### Verhindert der X-Content-Type-Options Header MIME-sniffing?
- [ ] Ja — Die `nosniff`-Direktive ist **vorhanden** und korrekt implementiert  
- [ ] Nein — Der Header **fehlt**, was einem Browser potenziell ermöglichen könnte, nicht ausführbare Dateien (z. B. Bilder) als Skripte auszuführen

---

## WSTG-IDNT-01 — Prüfung der Rollendefinitionen

Die Prüfung der Rollendefinitionen umfasst die Identifizierung und Dokumentation der verschiedenen innerhalb der Anwendung definierten Benutzerrollen und Berechtigungsstufen, um sicherzustellen, dass diese logisch getrennt sind und dem Prinzip der geringsten Berechtigung (Principle of Least Privilege) entsprechen. Ein Angreifer analysiert die Anwendung, um Rollen mit hohen Privilegien gegenüber Standardbenutzerrollen abzubilden und nach Unklarheiten in den Berechtigungsgrenzen oder undokumentierten administrativen Funktionen zu suchen. Die Identifizierung dieser Rollen ist der erste Schritt zur Entdeckung von Wegen zur Privilegieneskalation (Privilege Escalation), da Inkonsistenzen in den Rollendefinitionen häufig zu unbefugtem Zugriff auf sensible Daten oder administrative Funktionen führen. Diese Phase findet in der Regel während der initialen Reconnaissance und des Mappings der Geschäftslogik (Business Logic Mapping) statt, wobei der Fokus auf Benutzerprofilen, administrativen Dashboards und API-Berechtigungsstrukturen liegt.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Sind Rollen in der Anwendung oder deren Dokumentation klar definiert und dokumentiert?
- [ ] Ja — Rollen sind vollständig dokumentiert und folgen dem Prinzip der geringsten Berechtigung (**Least Privilege**)  
- [ ] Ja — Rollen sind dokumentiert, enthalten jedoch **überlappende Berechtigungen**  
- [ ] Nein — es existieren keine formalen Rollendokumentationen oder -definitionen  

### Gibt es eine klare Trennung zwischen administrativen Funktionen und Standardbenutzerfunktionen?
- [ ] Ja — administrative Funktionen sind **vollständig isoliert** von Standardbenutzeransichten  
- [ ] Ja — eine Trennung ist vorhanden, aber administrative Endpunkte sind **vorhersehbar oder erratbar**  
- [ ] Nein — administrative und Standardfunktionen sind innerhalb derselben Schnittstellen **vermischt**  

### Verwendet die Anwendung einen zentralisierten Mechanismus für rollenbasierte Zugriffskontrolle (RBAC)?
- [ ] Ja — zentralisiertes **RBAC** oder **ABAC** ist **implementiert** und wird konsistent erzwungen  
- [ ] Ja — dezentrale Kontrollen existieren, werden jedoch über Module hinweg **inkonsistent angewendet**  
- [ ] Nein — die Zugriffskontrolllogik ist in einzelnen Seiten oder Komponenten **hartkodiert**  

### Sind undokumentierte oder versteckte Rollen im System vorhanden?
- [ ] Nein — alle entdeckten Rollen entsprechen der dokumentierten Funktionalität  
- [ ] Ja — versteckte oder "Schatten"-Rollen (z. B. `super_admin`, `support`, `test`) **sind vorhanden**  

### Kann ein Benutzer mit niedrigen Privilegien die Berechtigungen oder Rollen anderer Benutzer aufzählen (Enumerate)?
- [ ] Nein — Benutzer **können keine** Rollen/Berechtigungen anderer Entitäten einsehen oder aufzählen  
- [ ] Ja — Benutzer **können** Rollen über Profilseiten, Metadaten oder API-Antworten aufzählen *(Mittel)*

---

## WSTG-IDNT-02 — Prüfung des Benutzerregistrierungsprozesses

Die Prüfung des Benutzerregistrierungsprozesses umfasst die Bewertung des Workflows, über den neue Identitäten erstellt werden, um sicherzustellen, dass dieser nicht für unbefugten Zugriff oder automatisierte Ausnutzung missbraucht werden kann. Schwachstellen in diesem Prozess äußern sich häufig durch fehlende Identitätsverifizierung (E-Mail/SMS), Anfälligkeit für Massenerstellung von Konten durch Bots oder Offenlegung von Informationen durch ausführliche Fehlermeldungen, die existierende Benutzernamen preisgeben. Angreifer nutzen diese Mängel aus, um User Enumeration durchzuführen, großflächige Spam-Kampagnen zu betreiben oder Registrierungsparameter zu manipulieren, um während der Phase der Kontoerstellung erweiterte Privilegien zu erlangen. Dieser Test ist entscheidend für den Schutz der Integrität der Anwendung und um zu verhindern, dass Angreifer das System mit betrügerischen oder bösartigen Konten füllen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Ist eine Identitätsverifizierung erforderlich, bevor ein neues Konto aktiv wird?
- [ ] Ja — die Registrierung erfordert ein eindeutiges, zeitlich begrenztes Token, das per E-Mail oder SMS versendet wird  
- [ ] Ja — eine Verifizierung ist erforderlich, aber die Token sind **schwach** oder **vorhersehbar**  
- [ ] Nein — Konten werden **sofort** ohne Verifizierungsschritt aktiviert  

### Verhindert der Registrierungsprozess User Enumeration?
- [ ] Ja — die Anwendung gibt **identische** Antworten sowohl für neue als auch für bereits existierende E-Mails/Benutzernamen zurück  
- [ ] Nein — Fehlermeldungen (z. B. „E-Mail bereits vergeben“) **ermöglichen** es einem Angreifer, registrierte Benutzer zuzuordnen  
- [ ] Nein — Zeitunterschiede (Timing differences) in den Serverantworten **geben Aufschluss** darüber, ob ein Benutzer existiert  

### Sind Anti-Automatisierungs-Mechanismen implementiert, um Massenregistrierungen zu verhindern?
- [ ] Ja — CAPTCHA- oder Proof-of-Work-Mechanismen sind **vorhanden** und **effektiv**  
- [ ] Ja — Kontrollen existieren, aber ein Bypass **ist möglich** über API-Endpunkte oder Header-Manipulation  
- [ ] Nein — am Registrierungs-Endpunkt wird kein Rate Limiting oder CAPTCHA **angewendet**  

### Können Registrierungsparameter manipuliert werden, um Privilegien zu eskalieren?
- [ ] Nein — Benutzerrollen werden **strikt** durch die serverseitige Logik zugewiesen  
- [ ] Ja — Parameter wie `role`, `admin` oder `group_id` **können** in der Anfrage geändert werden, um höheren Zugriff zu erlangen  

### Wird die Passwortrichtlinie (Password Policy) während der Registrierungsphase erzwungen?
- [ ] Ja — Anforderungen an starke Passwörter werden serverseitig **erzwungen**  
- [ ] Nein — schwache Passwörter (z. B. „password123“) werden vom System **akzeptiert**

---

## WSTG-IDNT-03 — Prüfung des Account-Bereitstellungsprozesses

Der Account-Bereitstellungsprozess (Account Provisioning Process) regelt, wie neue Identitäten innerhalb einer Anwendungsumgebung erstellt, verifiziert und mit Berechtigungen ausgestattet werden. Angreifer zielen auf diesen Workflow ab, um automatisierte Massenregistrierungen für Spam oder Denial-of-Service (DoS) durchzuführen, Identitätsverifizierungsschritte zu umgehen, um betrügerische Konten zu erstellen, oder Parameter zu manipulieren, um sich während der Registrierungsphase erweiterte Berechtigungen zu verschaffen. Schwachstellen zeigen sich häufig in Self-Service-Registrierungsformularen, Systemen mit Einladungszwang (Invitation-only) oder administrativen Bereitstellungskonsolen, in denen Business Logic Flaws eine unbefugte Account-Erstellung oder eine Privilege Escalation ermöglichen. Aus der Sicht eines Angreifers bietet die Kompromittierung des Bereitstellungsflusses einen legitimen Zugangspunkt, der als Ausgangsbasis für tiefgreifendere Exploits, Datenexfiltration oder Social Engineering Angriffe genutzt werden kann.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Critical" eingestuft, wenn ein Angreifer administrative Konten bereitstellen oder globale Registrierungsbeschränkungen umgehen kann.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### Wird die Selbstregistrierung streng kontrolliert oder auf autorisierte Benutzer beschränkt?
- [ ] Ja — die Registrierung ist **deaktiviert** oder erfordert eine manuelle administrative Genehmigung  
- [ ] Ja — die Registrierung ist offen, aber auf **autorisierte** E-Mail-Domains oder Invitation-Token beschränkt  
- [ ] Nein — die Registrierung ist für jeden Benutzer **offen**, ohne Domain- oder Token-Beschränkungen  

### Ist ein robuster Mechanismus zur Identitätsverifizierung (z. B. E-Mail/SMS) vor der Account-Aktivierung erforderlich?
- [ ] Ja — eine Verifizierung ist **erforderlich** und **kann nicht** umgangen werden  
- [ ] Ja — eine Verifizierung ist **erforderlich**, kann aber durch Parameter Tampering oder vorhersehbare Token **umgangen** werden  
- [ ] Nein — Konten sind nach der Übermittlung **sofort aktiv**, ohne jegliche Verifizierung  

### Kann der Benutzer während der Registrierungsanfrage Rollen- oder Berechtigungsparameter manipulieren?
- [ ] Nein — Rollen werden **serverseitig zugewiesen** und es existieren keine clientseitigen Parameter  
- [ ] Ja — Rollenparameter existieren, aber eine Manipulation ist aufgrund serverseitiger Validierung **nicht möglich**  
- [ ] Ja — Rollen-/Berechtigungsparameter (z. B. `role=admin`, `is_staff=true`) **können** modifiziert werden, um erweiterten Zugriff zu erhalten *(Critical)*  

### Sind Rate Limiting oder Anti-Automatisierungskontrollen implementiert, um Massenerstellungen von Konten zu verhindern?
- [ ] Ja — Rate Limiting und CAPTCHAs sind **aktiv** und effektiv  
- [ ] Ja — Kontrollen existieren, aber ein Bypass **ist möglich** über Header-Spoofing oder CAPTCHA-Lösungsdienste  
- [ ] Nein — es werden **keine** Mechanismen für Rate Limiting oder Anti-Automatisierung angewendet  

### Gibt der Bereitstellungsprozess Informationen über bestehende Konten preis (User Enumeration)?
- [ ] Nein — Antwortnachrichten sind für existierende und nicht existierende Benutzer **identisch**  
- [ ] Ja — unterschiedliche Antworten oder Zeitabweichungen (Timing Variations) **ermöglichen** die Enumeration existierender Benutzer

---

## WSTG-IDNT-04 — Prüfung auf Account-Enumeration und erratbare Benutzerkonten

Account-Enumeration tritt auf, wenn eine Anwendung die Existenz oder Nichtexistenz eines Benutzerkontos durch Variationen in HTTP-Antworten, Fehlermeldungen oder messbare Zeitunterschiede preisgibt. Angreifer nutzen dieses Verhalten aus, indem sie systematisch Listen von Benutzernamen übermitteln, um valide Konten zu identifizieren, die anschließend Ziel von Credential Stuffing, Brute-Force-Angriffen oder Social Engineering werden. Diese Schwachstelle tritt häufig in Login-Portalen, Funktionen zum Zurücksetzen von Passwörtern, Registrierungsformularen und API-Endpunkten auf, die benutzerspezifische Identifikatoren verarbeiten. Aus Sicht eines Angreifers verringert eine erfolgreiche Enumeration die Angriffsfläche erheblich, indem sie einen blinden Brute-Force-Versuch in eine gezielte Ausnutzung bekannter, valider Identitäten verwandelt.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Sind die Authentifizierungs-Fehlermeldungen in allen Szenarien konsistent?
- [ ] Ja — generische Fehlermeldungen werden sowohl für ungültige Benutzernamen als auch für ungültige Passwörter verwendet  
- [ ] Ja — die Meldungen sind ähnlich, aber geringfügige Abweichungen in der Interpunktion oder Groß-/Kleinschreibung **sind vorhanden**  
- [ ] Nein — Fehlermeldungen geben explizit „Benutzer nicht gefunden“ oder „Falsches Passwort“ an *(Anfällig)*  

### Gibt der Prozess zum Zurücksetzen des Passworts („Passwort vergessen“) die Existenz von Konten preis?
- [ ] Nein — die Anwendung gibt eine generische Meldung zurück, unabhängig davon, ob die E-Mail/der Benutzername existiert  
- [ ] Ja — Kontrollen sind vorhanden, aber ein Bypass **ist möglich** über die Antwortlänge oder Statuscodes  
- [ ] Ja — die Anwendung bestätigt explizit, ob eine E-Mail zum Zurücksetzen gesendet wurde oder ob das Konto **nicht existiert**  

### Ist der Registrierungsprozess gegen User Enumeration geschützt?
- [ ] Nein — die Registrierung gibt keine Informationen preis oder verwendet CAPTCHA/Rate Limiting, um Massenprüfungen zu verhindern  
- [ ] Ja — Kontrollen sind vorhanden, aber ein Bypass **ist möglich** durch Timing-Unterschiede oder Abweichungen in der API-Antwort  
- [ ] Ja — die Anwendung benachrichtigt den Benutzer sofort, wenn der Benutzername oder die E-Mail **bereits vergeben ist**  

### Sind Zeitunterschiede beim Vergleich von validen gegenüber ungültigen Benutzernamen messbar?
- [ ] Nein — die Antwortzeiten sind unabhängig von der Kontogültigkeit konsistent, da Vergleiche in konstanter Zeit (Constant-Time) durchgeführt werden  
- [ ] Ja — Timing-Unterschiede existieren, sind aber zu gering, um zuverlässig über das Netzwerk gemessen zu werden  
- [ ] Ja — messbare Zeitabweichungen **sind vorhanden** (z. B. da das Password-Hashing nur für valide Benutzer ausgeführt wird)  

### Verwendet die Anwendung vorhersagbare oder erratbare Namenskonventionen für Konten?
- [ ] Nein — Konto-Identifikatoren sind zufällig (UUIDs) oder benutzerdefiniert und komplex  
- [ ] Ja — Konto-Identifikatoren folgen einem Muster (z. B. `emp001`, `emp002`) und eine Enumeration **ist möglich**  
- [ ] Ja — Identifikatoren sind sequenzielle Ganzzahlen, was eine vollständige Datenbank-Enumeration **möglich macht**

---

## WSTG-IDNT-05 — Prüfung auf schwache oder nicht erzwungene Benutzernamen-Richtlinien

Die Prüfung auf schwache oder nicht erzwungene Benutzernamen-Richtlinien (Username Policies) konzentriert sich auf die Regeln zur Erstellung von Kontoidentifikatoren und deren Anfälligkeit für Enumeration oder Spoofing. Schwache Richtlinien erlauben oft vorhersehbare Sequenzen, gängige Wörterbuchbegriffe oder Identifikatoren, die interne Mitarbeiter-IDs widerspiegeln, was den Suchraum für Brute Force- und Credential Stuffing-Angriffe erheblich verkleinert. Aus der Sicht eines Angreifers ist die Identifizierung dieser Muster der erste Schritt zur massenhaften Entdeckung von Konten, was oft durch die Analyse von Fehlermeldungen bei der Registrierung, dem Verhalten bei Passwort-Resets oder Metadaten in öffentlich zugänglichen Profilen erreicht wird. Die Gewährleistung einer robusten Richtlinie verhindert, dass Angreifer die Benutzerbasis systematisch erfassen können, und erleichtert die Abwehr gezielter Phishing-Versuche sowie automatisierter Versuche zum Authentication Bypass.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Basieren Benutzernamen auf hochgradig vorhersehbaren oder sequentiellen Mustern?
- [ ] Nein — Benutzernamen sind zufällig, benutzerdefiniert oder Strings mit hoher Entropie  
- [ ] Ja — Benutzernamen folgen einem vorhersehbaren Format (z. B. `user1001`, `user1002`), aber die Authentifizierung ist robust  
- [ ] Ja — Benutzernamen sind **streng sequentiell** oder folgen einem bekannten Unternehmensformat, was eine einfache Enumeration ermöglicht  

### Erzwingt die Anwendung Anforderungen an Mindestlänge und Komplexität für Benutzernamen?
- [ ] Ja — strikte Richtlinien für Länge und Zeichensatz **werden erzwungen** während der Registrierung  
- [ ] Ja — Richtlinien existieren, erlauben jedoch extrem kurze (z. B. 1–2 Zeichen) oder zu einfache Benutzernamen  
- [ ] Nein — **keine Richtlinie** wird bezüglich der Länge des Benutzernamens oder der Zeichentypen erzwungen  

### Können gültige Benutzernamen durch Anwendungsantworten enumeriert werden?
- [ ] Nein — Anwendung gibt generische Fehlermeldungen zurück und weist ein konsistentes Zeitverhalten für alle Versuche auf  
- [ ] Ja — Anwendung gibt unterschiedliche Fehlermeldungen zurück (z. B. „Benutzername bereits vergeben“), aber Rate Limiting **wird angewendet**  
- [ ] Ja — Anwendung **leakt** gültige Benutzernamen durch Registrierungsfehler, Passwort-Resets oder Zeitunterschiede  

### Verhindert die Richtlinie die Registrierung gängiger oder reservierter administrativer Benutzernamen?
- [ ] Ja — Anwendung blockiert gängige Namen (z. B. `admin`, `root`, `support`) und systemreservierte Begriffe  
- [ ] Nein — Benutzer **können** Konten mit sensiblen oder administrativen Namen registrieren  

### Ist die Benutzernamen-Richtlinie über alle Einstiegspunkte (API, Mobile, Web) hinweg konsistent?
- [ ] Ja — Richtlinien **werden konsistent** über alle Schnittstellen und Versionen hinweg angewendet  
- [ ] Nein — Legacy-Endpunkte oder spezifische API-Versionen **erzwingen nicht** dieselben Einschränkungen für Benutzernamen

---

## WSTG-ATHN-01 — Prüfung der Übertragung von Anmeldedaten über verschlüsselte Kanäle

Die Prüfung der über einen verschlüsselten Kanal übertragenen Anmeldedaten stellt sicher, dass sensible Authentifizierungsdaten wie Benutzernamen, Passwörter und Session Tokens vor dem Abfangen während der Übertragung geschützt sind. Angreifer im Netzwerk – beispielsweise durch Man-in-the-Middle (MitM)-Angriffe in öffentlichen WLANs oder kompromittierten internen Netzwerken – können Packet Sniffing-Tools verwenden, um Klartext-Anmeldedaten zu erfassen, wenn TLS/SSL nicht ordnungsgemäß erzwungen wird. Diese Schwachstelle tritt typischerweise bei Login-Endpoints, Passwort-Reset-Formularen und API-Authentifizierungs-Headern auf, bei denen die Anwendung HTTPS nicht vorschreibt oder eine fehlerhafte Weiterleitung von HTTP durchführt. Eine erfolgreiche Ausnutzung gewährt dem Angreifer vollen Zugriff auf Benutzerkonten, was zu einer vollständigen Kompromittierung der Benutzerdaten und potenzieller Lateral Movement innerhalb der Anwendung führt.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Wird HTTPS für den gesamten Authentifizierungsprozess erzwungen?
- [ ] Ja — HSTS ist **aktiviert** und der gesamte Datenverkehr wird strikt über HTTPS erzwungen  
- [ ] Ja — eine Weiterleitung zu HTTPS erfolgt, aber HSTS ist **nicht aktiviert**  
- [ ] Nein — Anmeldedaten **können** über unverschlüsseltes HTTP übermittelt werden  

### Werden Login-Seiten und -Formulare über einen verschlüsselten Kanal bereitgestellt?
- [ ] Ja — die Seite, die das Login-Formular enthält, wird über HTTPS ausgeliefert  
- [ ] Nein — die Login-Seite wird über HTTP bereitgestellt, was Form-Action Hijacking oder das Sniffing von Anmeldedaten ermöglicht  

### Ist das `Secure`-Flag für alle sensiblen Session-Cookies gesetzt?
- [ ] Ja — das `Secure`-Flag ist für alle Authentifizierungs- und Session-Cookies **gesetzt**  
- [ ] Ja — das `Secure`-Flag ist nur für einige Cookies **gesetzt**  
- [ ] Nein — das `Secure`-Flag ist **nicht gesetzt**, was dazu führen kann, dass Cookies über unverschlüsselte Anfragen exfiltriert werden  

### Unterstützt der Server schwache TLS-Versionen oder unsichere Cipher Suites?
- [ ] Nein — nur modernes TLS (1.2+) und starke Ciphers sind **aktiviert**  
- [ ] Ja — veraltete Versionen (TLS 1.0/1.1) oder schwache Ciphers (RC4, DES, CBC) werden **unterstützt**  

### Verhindert die Anwendung die Übertragung von Anmeldedaten an Drittanbieter-Domains über unverschlüsselte Kanäle?
- [ ] Ja — es werden keine sensiblen Daten an externe Domains gesendet oder alle externen Aufrufe werden über HTTPS erzwungen  
- [ ] Nein — Authentifizierungs-Header oder Anmeldedaten werden über HTTP an Drittanbieter-Endpoints (z. B. Analytics oder SSO) gesendet

---

## WSTG-ATHN-02 — Testen auf Standard-Zugangsdaten

Das Testen auf Standard-Zugangsdaten (Default Credentials) umfasst die Identifizierung von Administrations-Schnittstellen, Diensten oder Anwendungskomponenten, die noch werkseitig voreingestellte oder vom Hersteller bereitgestellte Benutzernamen und Passwörter verwenden. Diese Schwachstelle ist ein hochpriorisiertes Ziel für Angreifer, da sie oft einen direkten Pfad zur vollständigen Systemkompromittierung, administrativem Zugriff oder Remote Code Execution (RCE) mit minimalem Aufwand bietet. Dies tritt typischerweise bei Standardsoftware, CMS-Plattformen, Datenbank-Management-Tools und integrierter Hardware wie IoT-Geräten oder Netzwerkgeräten auf. Die Ausnutzung erfolgt in der Regel durch den Abgleich identifizierter Softwareversionen mit öffentlichen Standard-Passwortdatenbanken oder durch den Einsatz automatisierter Credential Stuffing Tools während der ersten Reconnaissance- und Exploit-Phasen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Kritisch / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Sind Administrations- oder Management-Schnittstellen im Internet oder ungesicherten Netzwerksegmenten exponiert?
- [ ] Nein — es sind keine Management-Schnittstellen zugänglich  
- [ ] Ja — Schnittstellen sind vorhanden, aber durch netzwerkseitige Kontrollen eingeschränkt (z. B. VPN, IP Allowlist)  
- [ ] Ja — Schnittstellen **sind** öffentlich zugänglich und erfordern eine Authentifizierung  

### Wurden die vom Hersteller bereitgestellten Standard-Zugangsdaten für alle identifizierten Komponenten geändert?
- [ ] Ja — alle Standard-Zugangsdaten **wurden geändert** über alle Komponenten hinweg *(Am sichersten)*  
- [ ] Ja — die Zugangsdaten **wurden geändert** für die Hauptanwendung, aber sekundäre Komponenten (z. B. CMS, DB) verbleiben im Standardzustand  
- [ ] Nein — Standard-Zugangsdaten **sind aktiv** auf einer oder mehreren identifizierbaren Schnittstellen *(Kritisch)*  

### Erzwingt die Anwendung beim ersten Login für neue oder administrative Konten eine Passwortänderung?
- [ ] Ja — eine Passwortänderung **ist obligatorisch**, bevor administrative Aktionen durchgeführt werden können  
- [ ] Nein — die Anwendung **erlaubt** die dauerhafte Nutzung von Standard-Zugangsdaten  

### Sind Sperrmechanismen oder Rate Limiting Kontrollen aktiv, um automatisierte Tests auf Standard-Zugangsdaten zu verhindern?
- [ ] Ja — aggressives Rate Limiting oder Account Lockout **wird angewendet**  
- [ ] Ja — Kontrollen sind vorhanden, aber ein Bypass **ist möglich** durch Header-Manipulation oder IP-Rotation  
- [ ] Nein — es sind keine Rate Limiting oder Sperrmechanismen **aktiviert**  

### Verwenden Plugins von Drittanbietern, Middleware oder Administrations-Tools (z. B. phpMyAdmin, Tomcat Manager) eindeutige Zugangsdaten?
- [ ] Nein — in der Umgebung existieren keine Drittanbieter-Komponenten  
- [ ] Ja — alle Drittanbieter-Komponenten verwenden eindeutige, nicht-standardmäßige Zugangsdaten  
- [ ] Nein — mindestens eine Drittanbieter-Komponente **ist zugänglich** über bekannte Standard-Zugangsdaten

---

## WSTG-ATHN-03 — Prüfung auf schwache Kontosperrmechanismen (Weak Lock Out Mechanism)

Ein Kontosperrmechanismus (Account Lockout Mechanism) ist eine kritische Defense-in-Depth-Maßnahme, die darauf ausgelegt ist, Brute-Force- und Credential-Stuffing-Angriffe abzumildern, indem ein Konto nach einer festgelegten Anzahl fehlgeschlagener Authentifizierungsversuche deaktiviert wird. Ohne eine robuste Lockout-Richtlinie können Angreifer automatisierte Tools verwenden, um systematisch Passwörter über mehrere Konten hinweg zu erraten (Password Spraying) oder ein einzelnes hochwertiges Konto bis zum Erfolg anzugreifen. Diese Schwachstelle tritt typischerweise bei Login-Portalen, Passwort-Reset-Formularen und API-Endpunkten auf, denen Rate Limiting oder ein Schutz auf Kontoebene fehlt. Aus der Sicht eines Angreifers ermöglicht ein schwacher oder fehlender Sperrmechanismus unbegrenzte Passwortversuche, während ein übermäßig aggressiver Mechanismus ausgenutzt werden kann, um einen verteilten Denial of Service (DoS) gegen legitime Benutzer zu verursachen, indem deren Konten absichtlich gesperrt werden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als Hoch eingestuft, wenn die Anwendung sensible personenbezogene Daten (PII), Finanzdaten verarbeitet oder wenn administrative Konten ohne sekundäre Kontrollen anfällig für Brute-Force-Angriffe sind.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Wird ein Kontosperrmechanismus nach einer festgelegten Anzahl fehlgeschlagener Versuche erzwungen?
- [ ] Ja — das Konto wird nach einem definierten, angemessenen Schwellenwert gesperrt (z. B. 5–10 Versuche)  
- [ ] Ja — das Konto wird gesperrt, jedoch erst nach einer übermäßig hohen Anzahl von Versuchen  
- [ ] Nein — das Konto kann nicht gesperrt werden und ermöglicht zeitlich unbegrenzte Brute-Force-Angriffe  

### Kann der Sperrmechanismus mit gängigen Umgehungstechniken (Evasion Techniques) umgangen werden?
- [ ] Nein — die Sperre wird serverseitig erzwungen und wird nicht durch clientseitige Änderungen beeinflusst  
- [ ] Ja — eine Umgehung der Sperre ist durch die Rotation von IP-Adressen oder die Verwendung eines Proxy-Pools möglich  
- [ ] Ja — eine Umgehung der Sperre ist durch Manipulation von HTTP-Headern wie `X-Forwarded-For` oder `User-Agent` möglich  
- [ ] Ja — eine Umgehung der Sperre ist durch das Löschen von Cookies oder das Starten neuer Sitzungen möglich  

### Bietet der Sperrmechanismus einen Pfad zur Wiederherstellung des Kontos oder zum Ablauf der Sperre?
- [ ] Ja — das Konto wird nach einer festgelegten Dauer automatisch oder per E-Mail-Verifizierung entsperrt  
- [ ] Ja — zum Entsperren des Kontos ist das Eingreifen eines Administrators erforderlich  
- [ ] Nein — die Sperre ist dauerhaft ohne klaren Wiederherstellungspfad, was das DoS-Risiko erhöht  

### Unterscheidet die Anwendung während der Sperrung zwischen gültigen und ungültigen Benutzernamen?
- [ ] Nein — die Antwort der Anwendung ist für existierende und nicht existierende Benutzer identisch  
- [ ] Ja — die Anwendung gibt preis, ob ein Konto gesperrt ist, was eine Benutzernamen-Aufzählung (Username Enumeration) ermöglicht  

### Ist der Sperrmechanismus anfällig für einen Denial of Service (DoS) Angriff?
- [ ] Nein — Kontrollen wie CAPTCHA oder IP-basiertes Rate Limiting verhindern das massenhafte Sperren von Konten  
- [ ] Ja — ein Angreifer kann systematisch alle bekannten Benutzer sperren, indem er falsche Passwörter angibt

---

## WSTG-ATHN-04 — Prüfung auf Umgehung des Authentifizierungsschemas

Die Umgehung des Authentifizierungsschemas beinhaltet das Identifizieren und Ausnutzen von Fehlern in der Applikationslogik, die es einem Angreifer ermöglichen, Zugriff auf geschützte Ressourcen zu erhalten, ohne gültige Zugangsdaten anzugeben. Diese Schwachstelle tritt typischerweise auf, wenn sich die Anwendung auf clientseitige Kontrollen verlässt, es versäumt, eine serverseitige Autorisierung bei jeder Anfrage zu erzwingen, oder administrative "Backdoors" und Debug-Endpunkte in der Produktionsumgebung exponiert lässt. Angreifer nutzen diese Schwächen aus, indem sie Forced Browsing zu sensiblen URLs durchführen, HTTP-Parameter wie `authenticated=true` manipulieren oder Header fälschen (Spoofing), um die Anwendung vorzutäuschen, dass eine Session bereits etabliert ist. Eine erfolgreiche Ausnutzung führt zu einem vollständigen Zusammenbruch des Authentifizierungsperimeters, was potenziell zu unbefugter Datenexfiltration oder einer vollständigen Systemkompromittierung führen kann.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Können sensible interne Seiten direkt ohne eine aktive Session aufgerufen werden?
- [ ] Nein — der direkte Zugriff führt zu einer Weiterleitung zum Login oder einer 403/404-Antwort  
- [ ] Ja — einige sensible Seiten sind zugänglich, liefern jedoch nur begrenzte oder unkritische Daten  
- [ ] Ja — sensible administrative oder benutzerspezifische Seiten **sind** ohne Authentifizierung vollständig zugänglich *(Kritisch)*  

### Verlässt sich die Anwendung auf clientseitige Flags oder Parameter, um den Authentifizierungsstatus zu bestimmen?
- [ ] Nein — der Authentifizierungsstatus wird strikt über den serverseitigen Session-Status verwaltet  
- [ ] Ja — clientseitige Flags existieren, aber eine Modifikation gewährt **keinen** Zugriff auf geschützte Bereiche  
- [ ] Ja — die Modifikation von Parametern (z. B. `is_authenticated=true`, `user_role=admin`) **ermöglicht** die Umgehung der Authentifizierung  

### Kann die Authentifizierung durch Spoofing oder Manipulation spezifischer HTTP-Header umgangen werden?
- [ ] Nein — Header wie `X-Forwarded-For`, `Referer` oder benutzerdefinierte Header haben keinen Einfluss auf die Authentifizierung  
- [ ] Ja — die Manipulation von Headern (z. B. `X-Custom-IP-Authorization`, `X-Remote-User`) **ist möglich** und gewährt unbefugten Zugriff  

### Gibt es alternative Pfade oder Entwicklungsartefakte, die den standardmäßigen Login-Flow umgehen?
- [ ] Nein — alle geschützten Ressourcen sind durch eine zentralisierte, produktionsreife Authentifizierungsprüfung abgesichert  
- [ ] Ja — Legacy-Endpunkte, Debug-Routen (z. B. `/debug/login`) oder vergessene API-Versionen **sind** ohne Zugangsdaten zugänglich  

### Versäumt es die Anwendung, bei privilegierten Aktionen eine erneute Authentifizierung oder Session-Verifizierung durchzuführen?
- [ ] Nein — privilegierte oder sensible Aktionen erfordern ein frisches oder gültiges Session-Token  
- [ ] Ja — sobald eine Umgehung an einem Endpunkt gefunden wurde, **kann** diese genutzt werden, um administrative Aktionen in der gesamten Anwendung durchzuführen

---

## WSTG-ATHN-05 — Prüfung auf Schwachstellen in der "Passwort merken"-Funktion

Die "Remember Me"- oder persistente Login-Funktionalität dient dazu, den authentifizierten Status eines Benutzers über Browser-Neustarts hinweg beizubehalten, indem ein Token oder ein Credential auf der Client-Seite gespeichert wird. Wenn diese Funktion mangelhaft implementiert ist — etwa durch die Speicherung von Klartext-Passwörtern, schwachen Hashes oder Tokens mit geringer Entropie in Cookies — setzt sie den Benutzer dem Risiko einer Kontoübernahme (Account Takeover) durch Cross-Site Scripting (XSS), physischen Zugriff oder Netzwerk-Interception aus. Pentester bewerten die Entropie dieser Persistenz-Tokens, die auf den Speichermechanismus angewendeten Security Flags und den Lebenszyklus der Sitzung, um sicherzustellen, dass die Bequemlichkeit des persistenten Zugriffs keine kritischen Sicherheitskontrollen wie die Multi-Faktor-Authentifizierung (MFA) oder die Sitzungsinvalidierung umgeht. Die Ausnutzung beinhaltet häufig das Harvesting dieser langlebigen Tokens, um unbefugten Zugriff auf Konten zu erlangen, ohne die ursprünglichen Zugangsdaten zu benötigen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn Klartext-Zugangsdaten oder ungesalzene Hashes im persistenten Cookie gespeichert werden oder wenn das Token die Multi-Faktor-Authentifizierung (MFA) umgeht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### Implementiert die Anwendung eine "Remember Me"- oder "Angemeldet bleiben"-Funktion?
- [ ] Nein — Funktion ist in der Anwendung **nicht** vorhanden  
- [ ] Ja — Funktion existiert und verwendet kryptografisch starke, nicht vorhersehbare Tokens  
- [ ] Ja — Funktion existiert, verwendet jedoch vorhersehbare Tokens oder solche mit geringer Entropie *(Mittel)*  
- [ ] Ja — Funktion existiert und speichert Klartext-Zugangsdaten oder Base64-kodierte Passwörter *(Hoch)*  

### Sind die Security Flags korrekt auf das persistente Cookie angewendet?
- [ ] Ja — Die Flags `HttpOnly`, `Secure` und `SameSite` sind **gesetzt**  
- [ ] Ja — Einige Flags sind gesetzt, aber `HttpOnly` **fehlt**  
- [ ] Ja — Einige Flags sind gesetzt, aber `Secure` **fehlt** über HTTPS  
- [ ] Nein — Es sind **keine** Security Flags auf das persistente Cookie angewendet  

### Bleibt das "Remember Me"-Token nach einer manuellen Abmeldung (Logout) gültig?
- [ ] Nein — Das Token wird serverseitig sofort nach dem Logout entwertet  
- [ ] Ja — Das Token bleibt gültig, erfordert jedoch ein Passwort für sensible Aktionen  
- [ ] Ja — Das Token bleibt gültig und ermöglicht den vollen Kontozugriff **ohne** erneute Authentifizierung *(Mittel)*  

### Wird die persistente Sitzung nach einer Passwortänderung beendet?
- [ ] Ja — Alle persistenten Tokens werden widerrufen, wenn der Benutzer sein Passwort ändert  
- [ ] Nein — Das "Remember Me"-Token bleibt nach einem Passwort-Reset oder einer Passwortänderung **gültig** *(Hoch)*  

### Umgeht das persistente Token die Multi-Faktor-Authentifizierung (MFA) bei nachfolgenden Besuchen?
- [ ] Nein — MFA ist erforderlich, auch wenn ein "Remember Me"-Token vorhanden ist  
- [ ] Ja — MFA wird umgangen, aber das Token ist an einen spezifischen Geräte-Fingerabdruck (Device Fingerprint) gebunden  
- [ ] Ja — MFA wird unter Verwendung des persistenten Cookies von jedem beliebigen Gerät aus **vollständig umgangen** *(Hoch)*

---

## WSTG-ATHN-06 — Prüfung auf Schwachstellen im Browser-Cache

Schwachstellen im Browser-Cache treten auf, wenn sensible Informationen im lokalen Browser-Cache gespeichert werden und von einem unbefugten Benutzer mit Zugriff auf denselben physischen Rechner abgerufen werden können. Das Fehlen ordnungsgemäßer Cache-Control-Header ermöglicht es, dass potenziell sensible Daten wie persönliche Informationen, Kontodaten oder Session-Identifier nach dem Abmelden des Benutzers oder dem Schließen der Sitzung bestehen bleiben. Angreifer oder nachfolgende Benutzer an gemeinsam genutzten oder öffentlichen Terminals können dies ausnutzen, indem sie den Browserverlauf durchsuchen, die „Zurück“-Schaltfläche verwenden oder lokale Cache-Dateien untersuchen, um gecachte Antworten zu exfiltrieren. Die Implementierung strenger Direktiven wie `Cache-Control: no-store` und `Pragma: no-cache` ist für jede Anwendung, die authentifizierte Inhalte verarbeitet, von entscheidender Bedeutung, um sicherzustellen, dass sensible Daten niemals auf die Festplatte geschrieben werden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Niedrig / Mittel* |

> *Die Kritikalität wird als „Mittel“ eingestuft, wenn die Anwendung häufig von gemeinsam genutzten/öffentlichen Kiosksystemen aus aufgerufen wird oder hochsensible PII/PHI-Daten enthält.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Werkzeuge:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Sind angemessene Cache-Control-Header auf authentifizierten oder sensiblen Seiten vorhanden?
- [ ] Ja — `Cache-Control: no-store, no-cache` und `Pragma: no-cache` sind **vorhanden** und **korrekt konfiguriert**  
- [ ] Ja — einige Header sind vorhanden, aber `no-store` **fehlt**, was das Caching auf der Festplatte ermöglicht  
- [ ] Nein — es werden keine Cache-bezogenen Header **angewendet**, wodurch das Standardverhalten des Browsers genutzt wird  

### Verwendet die Anwendung die `private`-Direktive für benutzerspezifische Daten?
- [ ] Ja — `Cache-Control: private` ist für alle personalisierten Inhalte **aktiviert**, um das Caching durch Proxies zu verhindern  
- [ ] Nein — Inhalte sind als `public` markiert oder es fehlt die `private`-Direktive, was sie für zwischengeschaltete Proxies **cachebar** macht  

### Sind sensible Informationen nach einem erfolgreichen Logout über die „Zurück“-Schaltfläche des Browsers zugänglich?
- [ ] Nein — der Browser fordert zur erneuten Authentifizierung auf oder die Seite wird **nicht** aus dem lokalen Cache geladen  
- [ ] Ja — sensible Informationen sind nach Beendigung der Sitzung weiterhin über die „Zurück“-Schaltfläche **sichtbar**  

### Sind sensible Nicht-HTML-Dateien (z. B. PDFs, CSV-Exporte, JSON-API-Antworten) vor Caching geschützt?
- [ ] Nein — es werden keine sensiblen Nicht-HTML-Dateien von der Anwendung generiert oder verarbeitet  
- [ ] Ja — strikte Cache-Control-Header werden auf alle sensiblen Dateidownloads und API-Endpunkte **angewendet**  
- [ ] Nein — sensible Downloads oder API-Antworten werden im lokalen Browser-Cache oder in temporären Verzeichnissen **gespeichert**  

### Verwendet die Anwendung `Expires: 0` oder ein Datum in der Vergangenheit, um die Verwendung veralteter Daten zu verhindern?
- [ ] Ja — der `Expires`-Header ist auf `0` oder ein Datum in der Vergangenheit gesetzt, um **sicherzustellen**, dass der Browser den Inhalt sofort als veraltet behandelt  
- [ ] Nein — der `Expires`-Header **fehlt** oder ist für sensible Seiten auf ein Datum in der Zukunft gesetzt

---

## WSTG-ATHN-07 — Prüfung auf schwache Passwortrichtlinien

Die Prüfung auf schwache Passwortrichtlinien umfasst die Bewertung der Anforderungen an Komplexität, Länge und Entropie, die von einer Anwendung bei der Kontoerstellung und der Aktualisierung von Anmeldedaten erzwungen werden. Unzureichende Richtlinien beeinträchtigen direkt die Vertraulichkeit von Benutzerkonten, indem sie diese anfällig für automatisierte Dictionary Attacks, Brute-Force-Versuche und Credential Stuffing machen. Diese Schwachstellen finden sich in der Regel in Registrierungsendpunkten, Workflows zum Zurücksetzen von Passwörtern und administrativen Benutzerverwaltungspanels. Aus der Sicht eines Angreifers reduziert eine freizügige Richtlinie die Rechenkosten und den Zeitaufwand erheblich, die erforderlich sind, um Anmeldedaten mithilfe gängiger Wortlisten oder musterbasierter Cracking-Verfahren erfolgreich zu kompromittieren.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Niedrig* |

> *Der Schweregrad erhöht sich auf „Hoch“, wenn der Anwendung Mechanismen für Account Lockout oder Rate Limiting fehlen, um automatisiertes Erraten zu verhindern.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Tools:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Wird eine Mindestlänge für Passwörter von der Anwendung erzwungen?
- [ ] Ja — Mindestlänge beträgt 12+ Zeichen *(Am sichersten)*  
- [ ] Ja — Mindestlänge liegt zwischen 8 und 11 Zeichen  
- [ ] Ja — Mindestlänge ist **geringer als** 8 Zeichen  
- [ ] Nein — es wird keine Mindestlänge erzwungen  

### Werden Komplexitätsanforderungen (Großbuchstaben, Kleinbuchstaben, Ziffern, Symbole) erzwungen?
- [ ] Ja — mehrere Zeichenklassen sind obligatorisch und ein Bypass ist **nicht möglich**  
- [ ] Ja — Zeichenklassen werden empfohlen, aber **nicht** erzwungen  
- [ ] Nein — es werden keine Komplexitätsanforderungen angewendet  

### Blockiert die Anwendung gängige/schwache Passwörter oder Passwörter, die benutzerspezifische Daten enthalten?
- [ ] Ja — bekannte schwache Passwörter (z. B. „password123“) und auf dem Benutzernamen basierende Passwörter **können nicht** verwendet werden  
- [ ] Ja — gängige Passwörter werden blockiert, aber benutzerspezifische Daten (z. B. Benutzername, E-Mail) **können** verwendet werden  
- [ ] Nein — jede Zeichenfolge, welche die Anforderungen an Länge/Komplexität erfüllt, wird akzeptiert  

### Können Komplexitäts- oder Längenanforderungen durch clientseitige Modifikation umgangen werden?
- [ ] Nein — Richtlinien werden strikt auf der **Serverseite (Server-side)** erzwungen  
- [ ] Ja — Richtlinien werden über JavaScript erzwungen und **können** durch Abfangen der Anfrage umgangen werden  

### Wird die Passwortrichtlinie dem Benutzer klar kommuniziert, ohne Implementierungsdetails preiszugeben?
- [ ] Ja — Richtlinie ist während der Eingabe sichtbar und liefert generische Fehlermeldungen  
- [ ] Ja — Richtlinie ist sichtbar, aber Fehlermeldungen lassen Rückschlüsse auf spezifische Regex oder Logiklücken zu  
- [ ] Nein — Richtlinie ist **nicht** sichtbar, was eine Entdeckung durch Trial-and-Error erzwingt

---

## WSTG-ATHN-08 — Prüfung auf schwache Antworten auf Sicherheitsfragen

Die Prüfung auf schwache Antworten auf Sicherheitsfragen umfasst die Bewertung von Mechanismen der wissensbasierten Authentifizierung (Knowledge-Based Authentication, KBA), die zur Verifizierung der Identität eines Benutzers verwendet werden, üblicherweise während der Passwortwiederherstellung oder in Multi-Faktor-Authentifizierungs-Flows (MFA). Dies ist von Bedeutung, da Sicherheitsfragen oft auf statischen, nicht geheimen Informationen beruhen, die leicht durch Open-Source Intelligence (OSINT) oder Social Engineering ermittelt werden können, was zu einer unbefugten Kontoübernahme (Account Takeover) führen kann. Diese Schwachstellen treten typischerweise in den Modulen „Passwort vergessen“ oder „Konto entsperren“ auf, in denen Benutzer Antworten auf vordefinierte Fragen geben. Aus der Sicht eines Angreifers ist dies ein primäres Ziel für automatisierte Brute-Force-Angriffe oder gezielte Recherche, da viele Benutzer vorhersehbare oder öffentlich überprüfbare Antworten auf gängige Fragen geben, wie z. B. „Wie lautet der Geburtsname Ihrer Mutter?“ oder „Welche Highschool haben Sie besucht?“.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn die Sicherheitsfrage die einzige Anforderung für einen vollständigen Passwort-Reset und eine Kontoübernahme ohne weitere Verifizierung darstellt.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Tools:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### Implementiert die Anwendung Sicherheitsfragen zur Identitätsverifizierung oder Passwortwiederherstellung?
- [ ] Nein — Sicherheitsfragen werden **nicht** zur Authentifizierung oder Wiederherstellung verwendet  
- [ ] Ja — Sicherheitsfragen werden als optionaler sekundärer Faktor **verwendet**  
- [ ] Ja — Sicherheitsfragen werden als primärer/einziger Wiederherstellungsmechanismus **verwendet** *(Hohes Risiko)*  

### Werden Sicherheitsfragen aus einer festen Liste ausgewählt oder sind sie benutzerdefiniert?
- [ ] Nein — die Fragen sind vollständig benutzerdefiniert und erfordern eine hohe Entropie  
- [ ] Ja — die Fragen sind festgelegt, enthalten aber Optionen, die nicht über OSINT ermittelbar sind  
- [ ] Ja — die Fragen stammen aus einer festen Liste gängiger, über OSINT ermittelbarer Themen *(Anfällig)*  

### Wird Rate Limiting oder eine Kontosperrung auf Versuche zur Beantwortung von Sicherheitsfragen angewendet?
- [ ] Ja — striktes Rate Limiting und Kontosperrung (Account Lockout) **werden angewendet**, um Brute-Force-Angriffe zu verhindern  
- [ ] Ja — Rate Limiting **wird angewendet**, aber ein Bypass **ist möglich** über IP-Rotation oder Header-Manipulation  
- [ ] Nein — es wird **kein Rate Limiting** für Antwortversuche erzwungen  

### Erzwingt die Anwendung Komplexität oder Validierung für den Inhalt der Antwort?
- [ ] Ja — die Validierung verhindert gängige Wörter und stellt die Komplexität/Einzigartigkeit der Antwort sicher  
- [ ] Ja — eine grundlegende Validierung ist vorhanden, kann aber mit gängigen Wortlisten oder Dictionary Attacks **umgangen werden**  
- [ ] Nein — es wird **keine Validierung** durchgeführt; einfache oder einstellige Antworten **sind zulässig**  

### Kann die Abfrage der Sicherheitsfrage durch logische Fehler umgangen werden?
- [ ] Nein — der Wiederherstellungs-Flow erfordert zwingend eine gültige Antwort, um fortzufahren  
- [ ] Ja — der Wiederherstellungs-Flow **kann** durch Manipulation von HTTP-Response-Codes oder Session-Parametern umgangen werden  
- [ ] Ja — die Antwort wird im Quellcode oder in versteckten Feldern (Hidden Fields) der Seite reflektiert

---

## WSTG-ATHN-09 — Prüfung auf Schwachstellen in den Funktionen zur Passwortänderung oder zum Zurücksetzen von Passwörtern

Mechanismen zur Passwortänderung und zum Zurücksetzen von Passwörtern sind kritische Komponenten der Authentifizierungsverwaltung. Bei unsachgemäßer Implementierung ermöglichen sie Angreifern die Übernahme von Benutzerkonten. Diese Schwachstellen umfassen in der Regel vorhersehbare Reset-Tokens, fehlendes Rate Limiting oder Kontosperren bei Brute Force Versuchen, eine unsichere Übermittlung von Tokens oder die Möglichkeit, Passwörter zu ändern, ohne das aktuelle Passwort zu verifizieren. Angreifer nutzen diese Mängel aus, indem sie Wiederherstellungs-Links abfangen oder erraten, Host Header Injection durchführen, um Reset-E-Mails umzuleiten, oder Session Fixation anwenden, um den Zugriff nach einer Passwortänderung aufrechtzuerhalten. Die Sicherstellung, dass diese Prozesse eine starke Verifizierung erfordern und kryptografische Zufälligkeit nutzen, ist für die Aufrechterhaltung der Kontointegrität und die Verhinderung unbefugter Zugriffe unerlässlich.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Werkzeuge:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Wird bei einer Standardänderung das aktuelle Passwort benötigt, um ein neues Passwort festzulegen?
- [ ] Ja — das aktuelle Passwort **wird angefordert** und verifiziert  
- [ ] Ja — das aktuelle Passwort **wird angefordert**, kann jedoch durch Parameter Pollution oder Manipulation umgangen werden  
- [ ] Nein — das aktuelle Passwort **wird nicht** angefordert, was eine Kontoübernahme via Session Hijacking ermöglicht *(Hoch)*  

### Sind die Passwort-Reset-Tokens kryptografisch sicher und nicht vorhersehbar?
- [ ] Ja — Tokens weisen eine hohe Entropie auf, sind zufällig und **nicht vorhersehbar**  
- [ ] Ja — Tokens werden generiert, weisen jedoch eine geringe Entropie oder vorhersehbare Muster auf (z. B. zeitstempelbasiert)  
- [ ] Nein — Tokens sind **vorhersehbar** oder basieren auf statischen Benutzerdaten wie Base64-kodierten E-Mails/IDs *(Kritisch)*  

### Kann der Passwort-Reset-Link mittels Host Header Injection umgeleitet werden?
- [ ] Nein — die Anwendung ignoriert den `Host`-Header bei der Link-Generierung oder validiert diesen  
- [ ] Ja — die Anwendung nutzt den `Host`- oder `X-Forwarded-Host`-Header zur Erstellung von Links, jedoch findet eine Validierung **statt**  
- [ ] Ja — Links **können** durch Header-Manipulation auf eine vom Angreifer kontrollierte Domain umgeleitet werden *(Hoch)*  

### Verfügt das Reset-Token über eine begrenzte Lebensdauer und wird es sofort ungültig gemacht?
- [ ] Ja — Tokens laufen schnell ab und werden **unmittelbar** nach der Verwendung oder Passwortänderung invalidiert  
- [ ] Ja — Tokens laufen erst nach langer Zeit ab (z. B. 24+ Stunden) oder ermöglichen eine **Mehrfachverwendung**  
- [ ] Nein — Tokens **laufen niemals ab** oder bleiben gültig, nachdem das Passwort erfolgreich geändert wurde  

### Existieren Rate Limiting oder Kontosperren für die Endpunkte zum Zurücksetzen und Ändern von Passwörtern?
- [ ] Ja — striktes Rate Limiting und CAPTCHAs **werden angewendet**, um Enumeration und Brute Force zu verhindern  
- [ ] Ja — es existieren begrenzte Kontrollmechanismen, aber ein Bypass **ist möglich** via IP-Rotation oder Header-Spoofing  
- [ ] Nein — es wird **kein Rate Limiting** angewendet, was Massen-Enumeration von Konten oder Token-Brute-Forcing ermöglicht

---

## WSTG-ATHN-10 — Prüfung auf schwächere Authentifizierung in alternativen Kanälen

Die Prüfung auf schwächere Authentifizierung in alternativen Kanälen umfasst die Identifizierung und Analyse sekundärer Pfade für den Kontozugriff – wie mobile APIs, Workflows zur Passwortwiederherstellung, Helpdesk-Schnittstellen oder Legacy-Subdomains –, die möglicherweise nicht dieselbe Sicherheitsstrenge wie das primäre Webportal aufweisen. Diesen alternativen Kanälen mangelt es häufig an robuster Multi-Factor Authentication (MFA), striktem Rate Limiting oder komplexen Passwortanforderungen, was ein „schwächstes Glied“ schafft, das das gesamte Authentifizierungssystem untergräbt. Angreifer zielen gezielt auf diese übersehenen Einstiegspunkte ab, um Credential Stuffing durchzuführen oder MFA zu umgehen (Bypass), indem sie Diskrepanzen in der Handhabung der Identitätsprüfung durch verschiedene Schnittstellen ausnutzen. Die Gewährleistung der Parität über alle Authentifizierungsoberflächen hinweg ist entscheidend, um unbefugten Zugriff zu verhindern und die Integrität der Benutzersitzung und der Daten zu wahren.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Sind alternative Authentifizierungskanäle vorhanden und identifiziert?
- [ ] Nein — es existiert nur ein einziger Authentifizierungskanal  
- [ ] Ja — mehrere Kanäle identifiziert (z. B. Mobile-App API, Desktop-Client, Legacy-Portal, SOAP-Dienste)  

### Erzwingen alternative Kanäle die gleiche Sicherheits-Parität wie der primäre Kanal?
- [ ] Ja — alle Kanäle erzwingen **identische** Anforderungen an Passwortkomplexität und MFA  
- [ ] Ja — Kanäle erzwingen Passwortkomplexität, aber MFA ist **nicht erforderlich** oder **kann** umgangen werden  
- [ ] Nein — alternative Kanäle haben **signifikant schwächere** Authentifizierungsanforderungen  

### Sind Rate Limiting und Kontosperren (Account Lockout) über alle Kanäle hinweg konsistent?
- [ ] Ja — Rate Limiting und Sperren werden **konsistent** über alle Endpunkte hinweg erzwungen  
- [ ] Ja — Kontrollen sind vorhanden, aber ein Bypass **ist möglich** auf spezifischen Kanälen (z. B. mobile API)  
- [ ] Nein — Rate Limiting oder Sperren werden **nicht** auf sekundäre Authentifizierungspfade angewendet  

### Kann MFA durch den Wechsel zu einem alternativen Kanal umgangen werden?
- [ ] Nein — MFA ist obligatorisch und **kann nicht** umgangen werden, unabhängig vom Einstiegspunkt  
- [ ] Ja — MFA ist im Webportal erforderlich, wird aber in der mobilen oder Legacy-API **nicht erzwungen**  

### Sind Workflows zur Passwortwiederherstellung oder „Passwort vergessen“-Funktionen schwächer als der Standard-Login?
- [ ] Nein — Wiederherstellungs-Workflows nutzen eine starke Out-of-Band-Verifizierung und lassen keine Informationen abfließen  
- [ ] Ja — Wiederherstellungs-Workflows nutzen schwache Sicherheitsfragen, die leicht erraten oder recherchiert werden **können**  
- [ ] Ja — Wiederherstellungs-Workflows sind anfällig für Enumeration oder **erfordern nicht** das gleiche Maß an Identitätsnachweis wie ein Standard-Login

---

## WSTG-ATHN-11 — Prüfung der Mehrfaktor-Authentisierung (MFA)

Die Prüfung der Mehrfaktor-Authentisierung (MFA) bewertet die Robustheit der sekundären Sicherheitsschicht, die unbefugten Zugriff verhindern soll, selbst wenn die primären Anmeldedaten kompromittiert wurden. Angreifer versuchen häufig, die MFA zu umgehen, indem sie Endpunkte identifizieren, an denen diese nicht konsistent erzwungen wird, wie etwa veraltete API-Versionen, Mobile-Backends oder Workflows zum Zurücksetzen von Passwörtern. Die Ausnutzung umfasst häufig die Manipulation von Serverantworten, Brute-Force-Angriffe auf kurzlebige Codes (OTP), oder das Ausnutzen von Race Conditions und Schwachstellen im Session Management, die es einem Benutzer ermöglichen, in einen authentifizierten Zustand überzugehen, ohne den zweiten Faktor anzugeben.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### Wird MFA konsistent über alle Authentifizierungsportale hinweg erzwungen?
- [ ] Ja — MFA ist für alle web-, mobil- und API-basierten Login-Versuche erforderlich  
- [ ] Ja — aber MFA wird an spezifischen Endpunkten **nicht erzwungen** (z. B. veraltete `/api/v1/login` oder mobilspezifische Portale)  
- [ ] Nein — MFA ist in der Anwendung **nicht implementiert** *(Kritisch)*  

### Kann der MFA-Verifizierungsschritt durch direkte URL-Navigation oder Response-Manipulation umgangen werden?
- [ ] Nein — direkte Navigation oder Parameter-Tampering können die Abfrage **nicht** umgehen  
- [ ] Ja — das direkte Navigieren zu internen Dashboard-URLs **ist möglich**, ohne die MFA-Abfrage abzuschließen  
- [ ] Ja — die Manipulation der Serverantwort (z. B. Ändern eines `401 Unauthorized` in `200 OK`) **ist möglich**, um Zugriff zu erhalten  

### Ist der Prozess der MFA-Codeverifizierung gegen Brute-Force-Angriffe geschützt?
- [ ] Ja — striktes Rate Limiting oder Account-Lockout **wird angewendet**, nachdem mehrere OTP-Versuche fehlgeschlagen sind  
- [ ] Ja — Rate Limiting existiert, aber eine Umgehung **ist möglich** durch IP-Rotation oder Header-Manipulation  
- [ ] Nein — es wird kein Rate Limiting erzwungen und automatisiertes Brute-Forcing von Codes **ist möglich**  

### Behält die Anwendung während des MFA-Übergangs einen sicheren Session-Status bei?
- [ ] Ja — hochprivilegierte Session-Token werden **erst nach** erfolgreichem Abschluss der MFA ausgegeben  
- [ ] Nein — ein voll funktionsfähiges Session-Cookie wird bereits **vor** Abschluss der MFA ausgegeben, was den Zugriff auf einige Funktionen ermöglicht  
- [ ] Nein — die Anwendung verwendet einen statischen Identifikator oder einen vorhersehbaren Session-Übergang, der übernommen (hijacked) werden **kann**  

### Sind alternative MFA-Faktoren (SMS, E-Mail, Backup-Codes) anfällig für eine Ausnutzung?
- [ ] Nein — alle Faktoren verwenden sichere, nicht vorhersehbare und zeitlich begrenzte Werte  
- [ ] Ja — Backup-Codes sind vorhersehbar oder **können** enumeriert werden  
- [ ] Ja — die „Code erneut senden“-Funktionalität kann missbraucht werden, um SMS/E-Mail-Flooding durchzuführen oder Teile der Kontaktdaten offenzulegen

---

## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Schwachstellen wie Directory Traversal und File Inclusion entstehen, wenn eine Anwendung benutzergesteuerte Eingaben zur Konstruktion von Pfaden zu Dateien oder Verzeichnissen verwendet, ohne eine ausreichende Validierung oder Bereinigung (Sanitization) durchzuführen. Angreifer nutzen diese Fehler aus, indem sie Sequenzen wie `../` injizieren, um außerhalb des vorgesehenen Verzeichnisses zu navigieren und potenziell auf sensible Systemdateien, Konfigurationsdaten oder den Quellcode der Anwendung zuzugreifen. In schwerwiegenderen Fällen, die Local File Inclusion (LFI) oder Remote File Inclusion (RFI) betreffen, kann ein Angreifer durch das Einbinden bösartiger Skripte oder die Nutzung von Log Poisoning und PHP-Wrappern eine Remote Code Execution erzielen. Diese Schwachstellen befinden sich typischerweise in Parametern, die für das dynamische Laden von Inhalten, Template-Engines oder Endpunkte zum Abrufen von Bildern verwendet werden, bei denen die serverseitige Logik Dateisystempfade verarbeitet.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Tools:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Akzeptieren Parameter Dateinamen oder Pfade für die serverseitige Verarbeitung?
- [ ] Nein — keine Parameter scheinen mit dem Dateisystem zu interagieren  
- [ ] Ja — Parameter existieren, verwenden jedoch eine strikte Allowlist für Datei-Identifikatoren  
- [ ] Ja — Parameter akzeptieren direkte Dateinamen oder Pfade  

### Werden Eingaben bereinigt, um Directory Traversal-Sequenzen zu verhindern?
- [ ] Ja — Eingaben werden gegen eine strikte Allowlist validiert und Traversal-Sequenzen sind **nicht möglich**  
- [ ] Ja — Eingaben werden durch Entfernen von `../`-Sequenzen bereinigt, aber ein rekursiver Bypass **ist möglich**  
- [ ] Nein — es erfolgt **keine Bereinigung oder Validierung** für pfadbezogene Eingaben  

### Ist es möglich, über Traversal-Sequenzen auf Dateien außerhalb des eingeschränkten Verzeichnisses zuzugreifen?
- [ ] Nein — die Anwendung oder das Betriebssystem verhindert den Zugriff auf Dateien außerhalb des definierten Bereichs  
- [ ] Ja — Zugriff auf Dateien innerhalb des Web-Roots **ist möglich**  
- [ ] Ja — Zugriff auf sensible Systemdateien (z. B. `/etc/passwd`, `C:\Windows\win.ini`) **ist möglich** *(Kritisch)*  

### Erlaubt der Server das Einbinden von Remote-URLs (RFI)?
- [ ] Nein — Remote File Inclusion ist auf Server-/Anwendungsebene **deaktiviert**  
- [ ] Ja — Remote-Dateien **können** eingebunden werden, aber eine Ausführung **ist nicht möglich**  
- [ ] Ja — Remote-Dateien **können** eingebunden und auf dem Server ausgeführt werden *(Kritisch)*  

### Können Filter durch Kodierung oder Sonderzeichen umgangen werden?
- [ ] Nein — Filter verarbeiten verschiedene Kodierungen und Null-Bytes effektiv  
- [ ] Ja — Bypass **ist möglich** unter Verwendung von URL-Encoding, Double URL-Encoding oder 16-Bit Unicode  
- [ ] Ja — Bypass **ist möglich** unter Verwendung von Null Byte Injection (`%00`) oder Filesystem-Wrappern (z. B. `php://filter`)

---

## WSTG-ATHZ-02 — Prüfung auf Umgehung des Autorisierungsschemas

Eine Umgehung des Autorisierungsschemas tritt auf, wenn eine Anwendung die Zugriffskontrollen nicht korrekt erzwingt, wodurch ein Angreifer auf Ressourcen oder Funktionen zugreifen kann, die außerhalb seiner vorgesehenen Berechtigungen liegen. Diese Schwachstelle äußert sich typischerweise als horizontale Privilege Escalation, bei der ein Angreifer auf Daten eines anderen Benutzers mit demselben Rang zugreift, oder als vertikale Privilege Escalation, bei der ein Benutzer mit geringen Privilegien administrative Funktionen erlangt. Angreifer nutzen diese Mängel aus, indem sie Anfrageparameter wie Ressourcen-IDs manipulieren, Session-Token modifizieren oder HTTP-Header wie `X-Original-URL` verwenden, um pfadbasierte Einschränkungen zu umgehen. Die Gewährleistung einer robusten Autorisierung ist entscheidend für die Wahrung der Vertraulichkeit von Daten und die Verhinderung unbefugter administrativer Aktionen, die das gesamte System kompromittieren könnten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Wird horizontale Privilege Escalation zwischen Benutzern derselben Rolle verhindert?
- [ ] Ja — Autorisierungsprüfungen **werden** bei jeder Ressourcenanfrage angewendet und eine Umgehung ist **nicht möglich**  
- [ ] Ja — Autorisierungsprüfungen **werden angewendet**, können aber über IDOR oder Parameter Tampering umgangen werden  
- [ ] Nein — Benutzer **können** auf Daten anderer Benutzer zugreifen, indem sie einfach eine Ressourcen-ID ändern *(Hoch)*  

### Wird vertikale Privilege Escalation für Benutzer mit geringen Privilegien verhindert?
- [ ] Nein — die Anwendung hat nur eine Berechtigungsstufe  
- [ ] Ja — auf administrative Funktionen **kann nicht** von Benutzern mit geringen Privilegien zugegriffen werden  
- [ ] Ja — administrative Funktionen sind in der Benutzeroberfläche ausgeblendet, **können** aber über eine direkte URL aufgerufen werden  
- [ ] Nein — Benutzer mit geringen Privilegien **können** administrative Aktionen durch die Manipulation von Rollen oder Berechtigungen durchführen *(Kritisch)*  

### Kann die Autorisierung mittels HTTP Verb Tampering umgangen werden?
- [ ] Nein — Zugriffskontrollen werden unabhängig von der verwendeten HTTP-Methode erzwungen  
- [ ] Ja — das Ändern der Methode (z. B. von `GET` zu `POST` oder `PUT`) **umgeht** die Autorisierungsprüfungen  

### Sind administrative oder eingeschränkte Endpunkte ohne Authentifizierung zugänglich?
- [ ] Nein — alle sensiblen Endpunkte erfordern eine gültige Sitzung und entsprechende Berechtigungen  
- [ ] Ja — einige administrative Endpunkte sind zugänglich, wenn der Angreifer die direkte URL kennt  
- [ ] Ja — ein nicht authentifizierter Zugriff auf sensible Funktionen **ist möglich** *(Kritisch)*  

### Ermöglichen HTTP-Header die Umgehung einer pfadbasierten Autorisierung?
- [ ] Nein — die Anwendung ist **nicht** anfällig für Header-basierte Bypasses  
- [ ] Ja — Header wie `X-Original-URL`, `X-Rewrite-URL` oder `X-Forwarded-For` **können** verwendet werden, um Zugriffskontrollen zu umgehen

---

## WSTG-ATHZ-03 — Prüfung auf Privilege Escalation

Eine Privilege Escalation tritt auf, wenn ein Angreifer Schwachstellen ausnutzt, um Zugriff auf Ressourcen oder Funktionen zu erhalten, die Benutzern mit höheren Berechtigungsstufen oder anderen Identitäten vorbehalten sind. Bei der vertikalen Eskalation versucht ein Standardbenutzer, auf administrative Funktionen zuzugreifen, während die horizontale Eskalation den Zugriff auf Daten eines anderen Benutzers mit derselben Berechtigungsstufe umfasst. Diese Mängel manifestieren sich typischerweise in schlecht implementierten Access Control Lists (ACLs), Insecure Direct Object References (IDOR) oder Parameter Tampering innerhalb von Session- oder Identity-Tokens. Aus der Perspektive eines Angreifers wird dies häufig durch die Manipulation numerischer IDs, das Ändern rollenbasierter Parameter in Cookies oder JWTs oder das direkte Aufrufen versteckter API-Endpunkte erreicht, bei denen serverseitige Autorisierungsprüfungen fehlen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Implementiert die Anwendung mehrere Berechtigungsstufen oder Multi-Tenancy?
- [ ] Nein — Die Anwendung ist ein Einbenutzer- oder Einzelrollensystem  
- [ ] Ja — Es sind mehrere Rollen oder Mandanten definiert, die Autorisierungstests erfordern  

### Ist eine horizontale Privilege Escalation (Zugriff auf derselben Ebene) möglich?
- [ ] Nein — Autorisierungsprüfungen werden auf alle Ressourcenbezeichner und Session-Besitzer angewendet  
- [ ] Ja — Der Zugriff auf Daten anderer Benutzer ist durch IDOR oder Parameter Tampering möglich  
- [ ] Ja — Ein Datenabfluss ist möglich, aber die Änderung von Daten anderer Benutzer ist nicht möglich  

### Ist eine vertikale Privilege Escalation (niedrig zu hoch) möglich?
- [ ] Nein — Administrative Endpunkte erzwingen strikt rollenbasierte Zugriffskontrollen (RBAC)  
- [ ] Ja — Administrative Funktionen können von Benutzern mit geringen Privilegien über den direkten URL-Zugriff aufgerufen werden  
- [ ] Ja — Administrative Funktionen können durch die Manipulation von rollenbezogenen Headern, Cookies oder JWT-Claims aufgerufen werden  

### Können Benutzerrollen oder Berechtigungen über Mass Assignment oder Parameter Pollution modifiziert werden?
- [ ] Nein — Rollenbasierte Felder sind strikt schreibgeschützt und können nicht von Benutzern geändert werden  
- [ ] Ja — Benutzerberechtigungen können durch das Einfügen versteckter Parameter (z. B. `role=admin`) bei Profilaktualisierungen oder der Registrierung erhöht werden  

### Werden Autorisierungsprüfungen konsistent über alle API-Versionen und HTTP-Methoden hinweg erzwungen?
- [ ] Ja — Kontrollen werden konsistent über alle Versionen und Methoden hinweg angewendet  
- [ ] Nein — Veraltete API-Versionen (z. B. `/v1/`) oder spezifische HTTP-Methoden (z. B. `PUT`, `DELETE`, `PATCH`) umgehen die Autorisierung  
- [ ] Nein — Administrative Funktionen sind in der Benutzeroberfläche ausgeblendet, aber auf API-Ebene für alle Benutzer aktiviert

---

## WSTG-ATHZ-04 — Prüfung auf Insecure Direct Object References

Insecure Direct Object References (IDOR) treten auf, wenn eine Anwendung direkten Zugriff auf Objekte basierend auf benutzereingebauten Eingaben gewährt, ohne eine Autorisierungsprüfung durchzuführen, um die Berechtigungen des Anfragenden zu verifizieren. Diese Schwachstelle beeinträchtigt die Vertraulichkeit und Integrität erheblich, da sie es Angreifern ermöglicht, Daten anderer Benutzer oder des Systems einzusehen, zu ändern oder zu löschen. Sie manifestiert sich typischerweise in URL-Parametern, POST-Body-Daten oder JSON-Keys, die auf interne Datenbankschlüssel, Dateinamen oder Account-Identifikatoren verweisen. Aus der Sicht eines Angreifers beinhaltet die Ausnutzung die Manipulation dieser Identifikatoren – oft durch Inkrementierung von Ganzzahlen oder Fuzzing von UUIDs –, um auf Ressourcen außerhalb des autorisierten Bereichs zuzugreifen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### Verwendet die Anwendung direkte Objekt-Identifikatoren in Anfrageparametern?
- [ ] Nein — die Anwendung verwendet indirekte Referenzen oder verschlüsselte Maps *(Am sichersten)*  
- [ ] Ja — die Anwendung verwendet nicht vorhersehbare Identifikatoren (z. B. lange UUIDs/Hashes)  
- [ ] Ja — die Anwendung verwendet vorhersehbare Identifikatoren (z. B. inkrementelle Ganzzahlen oder Benutzernamen)  

### Wird für jede Anfrage, die einen Objekt-Identifikator enthält, eine serverseitige Autorisierung durchgeführt?
- [ ] Ja — serverseitige Prüfungen **können nicht** für einen Identifikator umgangen werden  
- [ ] Ja — serverseitige Prüfungen sind vorhanden, aber eine Umgehung **ist möglich** über Parameter Pollution oder Header-Manipulation  
- [ ] Nein — es **wird keine** Autorisierungsprüfung angewendet, um den Besitz des angeforderten Objekts zu verifizieren  

### Kann ein Angreifer auf Objekte zugreifen oder diese ändern, die anderen Benutzern gehören (Horizontal IDOR)?
- [ ] Nein — der Zugriff auf Daten anderer Benutzer **ist nicht möglich**  
- [ ] Ja — das Einsehen von Daten anderer Benutzer **ist möglich**, aber eine Änderung **ist nicht möglich**  
- [ ] Ja — sowohl das Einsehen als auch die Änderung von Daten anderer Benutzer **ist möglich** *(Kritisch)*  

### Ist es möglich, auf administrative oder systemnahe Objekte zuzugreifen (Vertical IDOR)?
- [ ] Nein — der Zugriff auf höher privilegierte Objekte **ist nicht möglich**  
- [ ] Ja — der Zugriff auf administrative Konfigurationen oder Systemdateien **ist möglich**  

### Gibt die Anwendung Objekt-Identifikatoren über die Suche, Metadaten oder andere Endpunkte preis?
- [ ] Nein — Identifikatoren werden **nicht unbeabsichtigt** offengelegt  
- [ ] Ja — Identifikatoren für andere Benutzer/Objekte **können** über sekundäre Endpunkte oder öffentliche Profile enumeriert werden

---

## WSTG-ATHZ-05 — Testen auf OAuth-Schwachstellen

OAuth-Schwachstellen umfassen eine Vielzahl von Fehlern in der Implementierung der Protokolle OAuth 2.0 oder OpenID Connect (OIDC), die häufig zu einer vollständigen Kontoübernahme (Account Takeover) oder unbefugtem Datenzugriff führen. Diese Schwachstellen treten typischerweise während des Autorisierungsablaufs (Authorization Flow) auf, insbesondere bei der Validierung des `redirect_uri`, der Entropie und Verifizierung des `state`-Parameters sowie dem sicheren Umgang mit Access- oder ID-Tokens. Angreifer nutzen diese aus, indem sie Autorisierungscodes abfangen, Tokens über Open Redirects auf vom Angreifer kontrollierte Domänen umleiten oder Cross-Site Request Forgery (CSRF) durchführen, um die Sitzung eines Opfers mit dem Konto eines Angreifers zu verknüpfen. Da OAuth oft als primärer Authentifizierungsmechanismus dient, kann eine einzige Fehlkonfiguration die Integrität des gesamten Identitätssystems gefährden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Werkzeuge:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Wird der `state`-Parameter implementiert und validiert, um CSRF zu verhindern?
- [ ] Ja — `state` ist obligatorisch, pro Sitzung eindeutig und wird serverseitig streng validiert  
- [ ] Ja — `state` ist vorhanden, aber statisch oder über verschiedene Sitzungen hinweg vorhersehbar  
- [ ] Ja — `state` ist vorhanden, aber die Anwendung validiert ihn **nicht** beim Callback  
- [ ] Nein — der `state`-Parameter wird in der Autorisierungsanfrage **nicht** verwendet *(Kritisch)*  

### Wird der `redirect_uri` streng gegen eine Whitelist validiert?
- [ ] Ja — nur exakte Übereinstimmungen mit einer vorregistrierten Whitelist werden **akzeptiert**  
- [ ] Ja — die Validierung erfolgt, aber ein Bypass ist über Path Traversal oder Subdomain-Manipulation **möglich**  
- [ ] Ja — die Validierung erfolgt, aber ein Bypass ist über Parameter Pollution oder Fragment Injection **möglich**  
- [ ] Nein — die Anwendung **erlaubt** beliebige URLs im `redirect_uri`-Parameter *(Kritisch)*  

### Kann der `response_type` manipuliert werden, um den Ablauf zu ändern?
- [ ] Nein — die Anwendung akzeptiert nur den vorgesehenen Flow (z. B. `code`) und lehnt andere ab  
- [ ] Ja — ein Wechsel von `code` zu `token` (Implicit Flow) **ist möglich** und führt zum Leak von Tokens in der URL  
- [ ] Ja — Hybrid-Flows oder unbefugte `response_type`-Werte sind **aktiviert** und werden verarbeitet  

### Werden Access-Tokens oder Autorisierungscodes über Referer-Header oder den Browserverlauf geleakt?
- [ ] Nein — Tokens/Codes werden sicher verarbeitet und sensible Seiten verwenden eine angemessene `Referrer-Policy`  
- [ ] Ja — Tokens/Codes **werden** über den `Referer`-Header an Drittanbieter-Domänen geleakt  
- [ ] Ja — Tokens/Codes **sind** aufgrund von unsicherem Caching oder GET-Anfragen im Browserverlauf sichtbar  

### Setzt die Anwendung das "Least Privilege"-Prinzip für OAuth-Scopes durch?
- [ ] Ja — die Anwendung fordert nur die für die Funktionalität minimal erforderlichen Scopes an  
- [ ] Nein — die Anwendung fordert standardmäßig übermäßige oder administrative Scopes an  
- [ ] Nein — der Benutzer **kann** während der Zustimmungsphase (Consent Phase) keine spezifischen Scopes überprüfen oder ablehnen

---

## WSTG-SESS-01 — Prüfung des Session-Management-Schemas

Die Prüfung des Session-Management-Schemas konzentriert sich auf die Analyse, wie die Anwendung den Lebenszyklus und die Struktur von Session-Identifiern handhabt, um sicherzustellen, dass diese robust gegen Vorhersagen und Hijacking sind. Angreifer erfassen mehrere Session-Tokens, um statistische Analysen durchzuführen und nach Mustern, niedriger Entropie oder vorhersagbaren Inkrementen zu suchen, die es ermöglichen, valide Sessions für andere Benutzer zu fälschen. Schwachstellen im Schema äußern sich häufig als Session Fixation-Schwachstellen oder durch die Verwendung unzureichend zufälliger Werte, die typischerweise in HTTP-Cookies, URL-Parametern oder benutzerdefinierten Header-Implementierungen zu finden sind. Die Kompromittierung des Session-Schemas ist ein Angriff mit hoher Auswirkung, der einem Angreifer vollständigen Zugriff auf den authentifizierten Zustand eines Opfers gewährt, ohne dass dessen primäre Anmeldedaten benötigt werden.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Ist der Session-Identifier ausreichend komplex und zufällig?
- [ ] Ja — Token besteht statistische Zufallstests (hohe Entropie) und ein Bypass **ist nicht möglich**  
- [ ] Ja — Token ist lang, enthält aber vorhersagbare Muster oder statische Segmente  
- [ ] Nein — Token ist sequentiell, basiert auf vorhersagbaren Zeitstempeln oder weist eine **kritisch niedrige** Entropie auf *(Kritisch)*  

### Wo wird der Session-Identifier übertragen und gespeichert?
- [ ] Identifier wird in einem `Secure` und `HttpOnly` Cookie gespeichert *(Am sichersten)*  
- [ ] Identifier wird in einem Cookie gespeichert, dem jedoch die Flags `Secure` oder `HttpOnly` fehlen  
- [ ] Identifier wird **nur** über URL-Parameter oder `GET`-Anfragen übertragen *(Hohes Risiko)*  

### Gibt die Anwendung nach erfolgreicher Authentifizierung einen neuen Session-Identifier aus?
- [ ] Ja — ein neuer, eindeutiger Session-ID **wird unmittelbar** nach dem Login generiert  
- [ ] Nein — die Session-ID bleibt vor und nach dem Login identisch *(Session Fixation möglich)*  

### Ist der Session-Identifier an eine spezifische IP-Adresse oder einen User-Agent gebunden, um eine Wiederverwendung zu verhindern?
- [ ] Ja — die Session wird ungültig, wenn sich der Fingerprint des Clients signifikant ändert  
- [ ] Nein — die Session **kann** über verschiedene IP-Adressen oder Geräte hinweg ohne zusätzliche Verifizierung wiederverwendet werden  

### Werden Session-Identifier nach dem Logout oder einem Timeout serverseitig ordnungsgemäß ungültig gemacht?
- [ ] Ja — der serverseitige Session-Status wird beim Logout **vollständig zerstört**  
- [ ] Nein — die Session bleibt auf dem Server gültig, selbst wenn das clientseitige Cookie gelöscht wurde  
- [ ] Nein — die Session **läuft nie ab** oder hat einen übermäßig langen Timeout-Zeitraum

---

## WSTG-SESS-02 — Prüfung der Cookie-Attribute

Das Session-Management stützt sich häufig auf HTTP-Cookies, um den Status aufrechtzuerhalten, was die Sicherheitsattribute dieser Cookies zu einem primären Ziel für Angreifer macht. Durch das Weglassen oder die Fehlkonfiguration von Attributen wie `HttpOnly`, `Secure` und `SameSite` setzen Anwendungen Session-Token dem Diebstahl durch Cross-Site Scripting (XSS), dem Abfangen über unverschlüsselte Kanäle oder Cross-Site Request Forgery (CSRF)-Angriffen aus. Pentester untersuchen diese Flags, um festzustellen, ob ein Angreifer Session-Identifikatoren aus dem Document Object Model (DOM) des Browsers extrahieren oder den Browser dazu zwingen kann, diese in Cross-Origin-Anfragen zu senden. Eine korrekte Attributkonfiguration ist eine grundlegende Defense-in-Depth-Maßnahme, um sicherzustellen, dass sensible Token auf autorisierte, sichere Kontexte beschränkt bleiben.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analyse der Implementierung von Cookie-Flags
| Attribut | Vorhanden | Fehlt |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Wird das `HttpOnly`-Flag auf sensible Session-Cookies angewendet?
- [ ] Ja — auf **alle** sensiblen Session-Cookies angewendet *(Am sichersten)*  
- [ ] Ja — nur auf einige Session-Cookies angewendet  
- [ ] Nein — das Flag wird **nicht angewendet**, was den Zugriff über clientseitige Skripte ermöglicht *(Kritisch)*  

### Wird das `Secure`-Flag für über HTTPS übertragene Cookies erzwungen?
- [ ] Ja — auf **alle** über TLS übertragenen Cookies angewendet  
- [ ] Nein — das Flag wird **nicht angewendet**, wodurch Cookies über unverschlüsseltes HTTP gesendet werden können  

### Wird das `SameSite`-Attribut zur Minderung von CSRF-Risiken verwendet?
- [ ] Ja — für Session-Cookies auf `Strict` oder `Lax` gesetzt  
- [ ] Ja — auf `None` gesetzt, aber mit vorhandenem `Secure`-Flag  
- [ ] Nein — das Attribut **fehlt** oder ist auf `None` **ohne** `Secure` gesetzt  

### Sind die Attribute `Domain` und `Path` auf den minimal erforderlichen Bereich beschränkt?
- [ ] Ja — auf die **spezifische** Subdomain und den Anwendungspfad beschränkt  
- [ ] Nein — der Bereich ist zu **weit gefasst** (z. B. auf eine übergeordnete Domain oder das Root-Verzeichnis `/` gesetzt), was die Angriffsfläche vergrößert  

### Werden sensible Session-Cookies über die Attribute `Expires` oder `Max-Age` ordnungsgemäß für ungültig erklärt?
- [ ] Ja — es sind angemessene Ablaufdaten gesetzt  
- [ ] Nein — Cookies sind persistent mit übermäßig **langen** Lebensdauern  
- [ ] Nein — Cookies sind nur für die Sitzung gültig (Session-only), aber es **fehlt** die serverseitige Erzwingung eines Timeouts

---

## WSTG-SESS-03 — Session Fixation

Session Fixation tritt auf, wenn eine Anwendung die Session-ID nach der erfolgreichen Authentifizierung eines Benutzers nicht entwertet oder rotiert. Dies ermöglicht es einem Angreifer, einem Opfer ein bekanntes Session-Token aufzuzwingen. Wenn die Anwendung die Sitzungs-ID aus der Phase vor der Authentifizierung nach dem Login beibehält, kann ein Angreifer dem Opfer einen speziell manipulierten Link senden, der eine festgelegte Session-ID enthält, und anschließend die authentifizierte Sitzung kapern (hijacken). Diese Schwachstelle manifestiert sich typischerweise in Login-Formularen oder durch Session-IDs, die über URL-Parameter und Cookies akzeptiert werden. Aus der Sicht eines Angreifers beinhaltet die Ausnutzung das „Fixieren“ der Sitzung über einen bösartigen Link oder eine Header-Injection-Schwachstelle und das Warten darauf, dass das Opfer seine Anmeldedaten eingibt, wodurch die Notwendigkeit, ein aktives Token zu stehlen, effektiv umgangen wird.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Ändert sich die Session-ID nach erfolgreicher Authentifizierung?
- [ ] Ja — eine neue Session-ID **wird vergeben** und die alte wird entwertet *(Am sichersten)*  
- [ ] Ja — eine neue Session-ID **wird vergeben**, aber die alte **bleibt gültig**  
- [ ] Nein — die Session-ID **bleibt vor und nach der Authentifizierung identisch** *(Kritisch)*  

### Akzeptiert die Anwendung Session-IDs, die über URL-Parameter übermittelt werden?
- [ ] Nein — Session-IDs werden **ausschließlich** über Cookies verwaltet  
- [ ] Ja — Session-IDs werden in der URL akzeptiert, aber beim Login **ignoriert** oder **rotiert**  
- [ ] Ja — Session-IDs in der URL werden **akzeptiert** und nach der Authentifizierung **beibehalten**  

### Kann eine vom Angreifer definierte Session-ID der Anwendung aufgezwungen werden?
- [ ] Nein — die Anwendung **weist ungültige oder nicht existierende Session-IDs zurück**, die vom Benutzer bereitgestellt werden  
- [ ] Ja — die Anwendung **akzeptiert** und initialisiert eine Sitzung unter Verwendung einer beliebigen ID, die in einem Cookie bereitgestellt wird  
- [ ] Ja — die Anwendung **akzeptiert** und initialisiert eine Sitzung unter Verwendung einer beliebigen ID, die in einem URL-Parameter bereitgestellt wird  

### Werden Sitzungen vor der Authentifizierung korrekt beendet?
- [ ] Ja — die anonyme Sitzung wird beim Login-Ereignis **vollständig zerstört**  
- [ ] Nein — die anonyme Sitzung wird **nicht zerstört**, was potenzielles Session-Harvesting ermöglicht  

### Ist das Session-Cookie gegen clientseitige Injection geschützt?
- [ ] Ja — die Flags `HttpOnly` und `Secure` werden **angewendet**, um eine skriptbasierte Fixierung zu verhindern  
- [ ] Nein — die Flags werden **nicht angewendet**, sodass Session-IDs über Cross-Site Scripting (XSS) gesetzt werden können

---

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

## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) ist eine Schwachstelle, bei der ein Angreifer den Browser eines Opfers dazu verleitet, eine unerwünschte Aktion auf einer anderen Website auszuführen, auf der das Opfer aktuell authentifiziert ist. Dieser Exploit nutzt das Verhalten des Browsers aus, automatisch „Ambient Credentials“ wie Session-Cookies oder Authorization-Header an ausgehende Anfragen anzuhängen. Angreifer zielen in der Regel auf zustandsändernde (state-changing) Operationen ab, wie das Ändern von Passwörtern, das Aktualisieren von E-Mail-Adressen oder die Durchführung von Finanztransaktionen, indem sie bösartige Skripte oder versteckte Formulare auf einer Drittanbieter-Seite hosten. Eine erfolgreiche Ausnutzung kann zu einer vollständigen Kontoübernahme (Account Takeover) oder unbefugten Datenänderung führen, ohne dass der Benutzer davon Kenntnis hat oder zustimmt.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel / Hoch* |

> *Die Kritikalität wird als Hoch eingestuft, wenn die anfällige Aktion eine Kontoübernahme (Account Takeover), Privilegieneskalation oder unbefugte Finanztransaktionen ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Werkzeuge:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Wurden Anti-CSRF-Token für zustandsändernde Anfragen implementiert?
- [ ] Ja — eindeutige, kryptografisch starke Token sind für alle zustandsändernden Aktionen erforderlich  
- [ ] Ja — Token sind vorhanden, aber **nicht** pro Session eindeutig oder sie sind vorhersehbar  
- [ ] Nein — Anti-CSRF-Token sind **nicht** implementiert  

### Ist die serverseitige Validierung des Anti-CSRF-Tokens robust?
- [ ] Ja — der Server validiert das Token strikt und ein Bypass ist **nicht möglich**  
- [ ] Ja — die Validierung wird durchgeführt, kann aber durch Entfernen des Token-Parameters **umgangen** werden  
- [ ] Ja — die Validierung wird durchgeführt, kann aber durch die Angabe eines leeren oder Dummy-Tokens **umgangen** werden  
- [ ] Ja — die Validierung wird durchgeführt, aber das Token ist **nicht** an die Session des Benutzers gebunden  

### Verlässt sich die Anwendung auf leicht umgehbare Methoden zum CSRF-Schutz?
- [ ] Nein — die Anwendung verlässt sich nicht allein auf schwache Header oder Origin-Prüfungen  
- [ ] Ja — der Schutz beruht ausschließlich auf dem `Referer`- oder `Origin`-Header, welche **gefälscht** (spoofed) oder entfernt werden können  
- [ ] Ja — der Schutz beruht auf der Prüfung des `X-Requested-With`-Headers, was über CORS-Fehlkonfigurationen **umgangen** werden kann  

### Sind Session-Cookies mit dem `SameSite`-Attribut konfiguriert?
- [ ] Ja — `SameSite` ist bei allen session-bezogenen Cookies auf `Strict` oder `Lax` gesetzt  
- [ ] Nein — das `SameSite`-Attribut **fehlt**, was zum browserspezifischen Standardverhalten führt  
- [ ] Nein — `SameSite` ist explizit auf `None` gesetzt, ohne das `Secure`-Flag oder zusätzliche Schutzmaßnahmen  

### Kann der CSRF-Schutz durch Ändern der HTTP-Anfragemethode umgangen werden?
- [ ] Nein — der Schutz wird unabhängig von der verwendeten HTTP-Methode erzwungen  
- [ ] Ja — Token werden nur bei `POST`-Anfragen validiert, aber die Aktion **kann** über `GET` durchgeführt werden  
- [ ] Ja — das Ändern der Methode (z. B. auf `PUT` oder `DELETE`) umgeht die Token-Prüfung

---

## WSTG-SESS-06 — Prüfung der Logout-Funktionalität

Die Logout-Funktionalität ist eine kritische Sicherheitskontrolle, die darauf ausgelegt ist, die Sitzung eines Benutzers zu beenden und die zugehörigen Sitzungskennungen (Session Identifier) sowohl clientseitig als auch serverseitig zu entwerten. Eine fehlerhafte Implementierung des Logouts ermöglicht Session Fixation oder Hijacking-Angriffe, da ein Angreifer, der nach dem "Abmelden" eines Benutzers Zugriff auf dessen Rechner erhält, den immer noch aktiven Session Token potenziell wiederverwenden könnte. Pentester bewerten dies, indem sie prüfen, ob das Session Cookie aus dem Browser gelöscht wird, ob der serverseitige Sitzungsstatus explizit zerstört wird und ob die Navigation über die Zurück-Schaltfläche des Browsers den Zugriff auf gecachte sensible Daten ermöglicht. Eine sichere Logout-Implementierung stellt sicher, dass nach Auslösung der Aktion keine nachfolgenden Anfragen mit dem alten Session Token von der Anwendung autorisiert werden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Ist ein funktionaler Logout-Mechanismus vorhanden und zugänglich?
- [ ] Ja — Logout-Schaltfläche existiert und löst eine Terminanfrage (Termination Request) aus  
- [ ] Nein — Logout-Schaltfläche **fehlt** oder löst **kein** Terminierungsereignis aus  

### Wird die Sitzungskennung (Session Identifier) serverseitig entwertet?
- [ ] Ja — Der Server lehnt alle nachfolgenden Anfragen ab, die den alten Session Token verwenden  
- [ ] Nein — Der Server **akzeptiert weiterhin** den alten Session Token nach dem Logout  

### Löscht die Anwendung die Session Cookies im Browser beim Logout?
- [ ] Ja — Cookies werden mit einem abgelaufenen Datum oder einem leeren Wert überschrieben  
- [ ] Ja — Cookies bleiben bestehen, sind aber auf dem Server **nicht mehr gültig**  
- [ ] Nein — Cookies verbleiben im Browser und **behalten** ihre ursprünglichen Werte bei  

### Kann auf sensible authentifizierte Inhalte über die 'Zurück'-Schaltfläche des Browsers nach dem Logout zugegriffen werden?
- [ ] Nein — `Cache-Control: no-store` oder ähnliche Header verhindern das Anzeigen sensibler Seiten nach dem Logout  
- [ ] Ja — Sensible Seiten sind gecacht und **können** durch Navigation nach Beendigung der Sitzung eingesehen werden

---

## WSTG-SESS-07 — Testen von Session-Timeouts

Die Prüfung des Session-Timeouts bewertet, ob eine Anwendung eine Benutzersitzung nach einem vordefinierten Zeitraum der Inaktivität oder einer festgelegten Gesamtdauer effektiv beendet. Diese Kontrolle ist entscheidend zur Minderung des Risikos von Session Hijacking, insbesondere an gemeinsam genutzten Workstations oder in Szenarien, in denen ein Angreifer einen Session-Identifier durch Netzwerk-Interception oder Cross-Site Scripting (XSS) erlangt. Pentesters analysieren sowohl Idle-Timeouts (Inaktivitäts-Timeouts), die nach einem Zeitraum ohne Benutzerinteraktion ausgelöst werden, als auch absolute Timeouts, welche die Gesamtlebensdauer einer Sitzung unabhängig von der Aktivität begrenzen. Aus der Perspektive eines Angreifers bieten unbefristete oder übermäßig lange Sitzungen ein erweitertes Zeitfenster, um unbefugten Zugriff aufrechtzuerhalten und die Notwendigkeit einer erneuten Authentifizierung zu umgehen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Implementiert die Anwendung einen Idle-Session-Timeout?
- [ ] Ja — die Sitzung läuft nach einem kurzen Zeitraum (z. B. 15–30 Minuten) der Inaktivität ab  
- [ ] Ja — die Sitzung läuft ab, aber die Timeout-Dauer ist **übermäßig lang** (z. B. > 24 Stunden)  
- [ ] Nein — die Sitzung **bleibt während der Inaktivität unbegrenzt aktiv**  

### Wird der Session-Timeout serverseitig erzwungen?
- [ ] Ja — der Server lehnt Anfragen mit abgelaufenen Token unabhängig vom Client-seitigen Status ab  
- [ ] Nein — der Timeout wird **nur** über clientseitiges JavaScript oder Meta-Refresh erzwungen  

### Implementiert die Anwendung einen absoluten Session-Timeout?
- [ ] Ja — die Sitzung wird nach einer festen maximalen Dauer unabhängig von der Aktivität beendet  
- [ ] Nein — die Sitzung **kann** unbegrenzt verlängert werden, solange eine kontinuierliche Aktivität besteht  

### Wird der Session-Identifier bei Timeout auf dem Server ungültig gemacht?
- [ ] Ja — der Session-Token **kann nicht** wiederverwendet werden, sobald die Timeout-Schwelle erreicht ist  
- [ ] Nein — der Session-Token **ist auf dem Server weiterhin gültig**, selbst nachdem der Client zur Login-Seite weitergeleitet wurde  

### Kann die Sitzung durch automatisierte Heartbeat-Anfragen unbegrenzt aufrechterhalten werden?
- [ ] Nein — ein absoluter Timeout oder sekundäre Kontrollen **verhindern** eine unendliche Sitzungsverlängerung  
- [ ] Ja — periodische Hintergrundanfragen (z. B. AJAX) **können** die Sitzung unbegrenzt aufrechterhalten

---

## WSTG-SESS-08 — Session Puzzling

Session Puzzling, auch bekannt als Session Variable Overloading, tritt auf, wenn eine Anwendung dieselbe Session-Variable für mehrere Zwecke in verschiedenen Modulen verwendet oder es versäumt, Session-Zustände während Workflow-Übergängen ordnungsgemäß zu bereinigen. Angreifer nutzen dieses Verhalten aus, indem sie Session-Variablen in einem Kontext füllen – etwa in einer unauthentifizierten Profilvorschau oder einer mehrstufigen Registrierung – und anschließend zu einem geschützten Bereich navigieren, in dem die Anwendung fälschlicherweise diesen bereits existierenden Variablen vertraut. Dies kann zu kritischen Auswirkungen führen, einschließlich Authentication Bypass, Privilege Escalation oder unbefugtem Zugriff auf sensible Daten, indem die Anwendung getäuscht wird zu glauben, dass sich eine Session in einem höher privilegierten Zustand befindet, als es tatsächlich der Fall ist. Die Schwachstelle tritt am häufigsten in komplexen, zustandsabhängigen Anwendungen auf, in denen die Session-Management-Logik inkonsistent über verschiedene funktionale Komponenten hinweg angewendet wird.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### Verwendet die Anwendung dieselben Session-Variablennamen für unterschiedliche Zwecke in verschiedenen Modulen?
- [ ] Nein — Variablennamen sind eindeutig oder Scopes sind isoliert  
- [ ] Ja — Gemeinsame Variablennamen existieren, aber der Zustand wird zwischen Modulen bereinigt  
- [ ] Ja — Gemeinsame Variablennamen existieren und der Zustand **wird nicht** bereinigt  

### Können unauthentifizierte Aktionen Session-Variablen beeinflussen, die in authentifizierten Kontexten verwendet werden?
- [ ] Nein — Session-Variablen werden erst **nach** erfolgreicher Authentifizierung initialisiert  
- [ ] Ja — Session-Variablen können vor dem Login gesetzt werden, beeinflussen aber **nicht** den Zustand nach dem Login  
- [ ] Ja — Session-Variablen, die während der Pre-Authentication gesetzt wurden, **wird** nach der Authentifizierung vertraut *(Kritisch)*  

### Initialisiert die Anwendung den Session-Identifier und die zugehörigen Variablen bei einer Änderung der Berechtigungsstufe neu?
- [ ] Ja — Die Session wird vollständig zurückgesetzt und eine neue ID **wird ausgegeben**  
- [ ] Teilweise — Eine neue ID wird ausgegeben, aber einige Session-Variablen **bleiben bestehen**  
- [ ] Nein — Die Session-ID und alle Variablen **bleiben** unverändert  

### Können Administratorrechte durch Manipulation von Session-Variablen über ein nicht privilegiertes Konto erlangt werden?
- [ ] Nein — Berechtigungsvariablen **können nicht** durch Benutzer modifiziert werden  
- [ ] Ja — Variablen können modifiziert werden, aber die Anwendung **validiert** diese gegen die Backend-Datenbank  
- [ ] Ja — Variablen können modifiziert werden und die Anwendung **vertraut** dem Session-Zustand implizit *(Kritisch)*  

### Wird der Session-Zustand sofort nach Abschluss eines bestimmten Workflows (z. B. Passwort-Reset, Checkout) bereinigt?
- [ ] Ja — Session-Variablen für den Workflow werden nach Abschluss zerstört  
- [ ] Nein — Workflow-Variablen **bleiben bestehen** und können in anderen Anwendungsbereichen wiederverwendet werden

---

## WSTG-SESS-09 — Prüfung auf Session Hijacking

Session Hijacking tritt auf, wenn ein Angreifer eine gültige Sitzungskennung (Session Identifier, SID) abfängt, vorhersagt oder fixiert, um unbefugten Zugriff auf die aktive Sitzung eines Benutzers zu erhalten. Diese Schwachstelle resultiert typischerweise aus unzureichender Transportverschlüsselung, fehlenden Cookie-Security-Flags oder vorhersagbaren Algorithmen zur SID-Generierung, die es einem Angreifer ermöglichen, die Authentifizierung vollständig zu umgehen. Aus der Sicht eines Angreifers ermöglicht eine erfolgreiche Ausnutzung die vollständige Identitätsübernahme des Opfers, ohne dessen Zugangsdaten zu benötigen. Dies wird häufig durch Network Sniffing, Cross-Site Scripting (XSS) oder Session Fixation Angriffe erreicht.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Werden Sitzungskennungen während der Übertragung geschützt?
- [ ] Ja — Das `Secure`-Flag ist **aktiviert** und HSTS wird strikt erzwungen  
- [ ] Ja — Das `Secure`-Flag ist **aktiviert**, aber HSTS ist **deaktiviert**  
- [ ] Nein — Das `Secure`-Flag wird **nicht angewendet**, was das Abfangen der SID über unverschlüsselte Kanäle ermöglicht *(Hoch)*  

### Ist die Sitzungskennung vor dem Zugriff durch clientseitige Skripte geschützt?
- [ ] Ja — Das `HttpOnly`-Flag wird auf alle sitzungsbezogenen Cookies **angewendet**  
- [ ] Nein — Das `HttpOnly`-Flag wird **nicht angewendet**, was die Exfiltration der SID via XSS ermöglicht *(Kritisch)*  

### Implementiert die Anwendung eine Sitzungsbindung an Client-Attribute?
- [ ] Ja — Die Sitzung ist an Client-Attribute (IP/User-Agent) gebunden und eine Wiederverwendung ist **nicht möglich**  
- [ ] Ja — Eine Sitzungsbindung ist vorhanden, aber ein Bypass ist durch Header-Spoofing **möglich**  
- [ ] Nein — Es existiert keine Sitzungsbindung; die SID **ist gültig**, wenn sie von einer beliebigen Quelle aus verwendet wird  

### Ist die Sitzungskennung ausreichend zufällig, um Vorhersagbarkeit zu verhindern?
- [ ] Ja — Die SID verwendet einen kryptographisch sicheren Pseudozufallszahlengenerator (CSPRNG)  
- [ ] Nein — Die SID-Länge ist unzureichend oder folgt einer **vorhersagbaren** Sequenz bzw. einem Muster  

### Werden gleichzeitige Sitzungen verwaltet, um das Angriffsfenster zu begrenzen?
- [ ] Nein — Die Anwendung erzwingt strikt eine einzelne aktive Sitzung pro Benutzer  
- [ ] Ja — Mehrere gleichzeitige Sitzungen sind **aktiviert**, werden aber überwacht  
- [ ] Ja — Unbegrenzte gleichzeitige Sitzungen **sind** über verschiedene Geräte/Standorte hinweg **möglich**

---

## WSTG-SESS-10 — Testen von JSON Web Tokens

JSON Web Tokens (JWTs) werden häufig für das stateless Session-Management und die Identitätspropagation verwendet, aber ihre Sicherheit beruht vollständig auf der korrekten Implementierung von Signaturalgorithmen und einer strengen Claim-Verifizierung. Angreifer versuchen häufig, die Authentifizierung zu umgehen, indem sie Schwachstellen im „none“-Algorithmus ausnutzen, schwache HS256-Secret-Keys mittels Brute Force angreifen oder Algorithm-Switching-Angriffe durchführen, bei denen ein asymmetrischer öffentlicher Schlüssel als symmetrisches HMAC-Secret verwendet wird. Diese Schwachstellen treten typischerweise in Session-Cookies oder Authorization-Headern auf und ermöglichen es einem Angreifer potenziell, Berechtigungen zu erweitern (Privilege Escalation) oder beliebige Benutzer zu imitieren (Impersonation), indem Claims wie `role` oder `user_id` modifiziert werden, ohne die kryptografische Integrität des Tokens zu verletzen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Tools:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### Wird der „none“-Algorithmus vom Server akzeptiert?
- [ ] Nein — der Server **weist** Tokens mit `alg: none` im Header **ab**  
- [ ] Ja — der Server **akzeptiert** Tokens mit `alg: none` und einer leeren Signatur *(Kritisch)*  

### Wie wird die JWT-Signatur verifiziert und gegen Brute Force geschützt?
- [ ] Ja — es werden starke asymmetrische Schlüssel (RS256/ES256) oder HMAC-Secrets mit hoher Entropie verwendet  
- [ ] Ja — HS256 wird verwendet, aber das Secret **kann** aufgrund geringer Entropie oder Standardwerten mittels Brute Force geknackt werden  
- [ ] Nein — die Signatur-Verifizierung **wird nicht angewendet** oder **kann** durch Algorithm-Switching (RS256 zu HS256) umgangen werden  

### Werden Standard-Claims (exp, nbf, iat) vom Backend strikt erzwungen?
- [ ] Ja — `exp` (Expiration) ist vorhanden und **kann nicht** umgangen werden  
- [ ] Ja — `exp` ist vorhanden, aber der Server **erzwingt** die Prüfung während der Validierung **nicht**  
- [ ] Nein — Expiration-Claims **fehlen** oder werden ignoriert, was ein zeitlich unbegrenztes Session Replay ermöglicht  

### Verhindert die Implementierung Header-basierte Key-Injection (kid, jku, x5u)?
- [ ] Ja — Header werden bereinigt (Sanitization) und nur vertrauenswürdige interne Schlüssel/Quellen sind **erlaubt**  
- [ ] Nein — der `kid` (Key ID) Header ist anfällig für Directory Traversal oder SQL Injection, um auf eine bekannte Datei zu verweisen  
- [ ] Nein — die `jku` oder `x5u` Header **können** auf vom Angreifer kontrollierte URLs verwiesen werden, um bösartige Schlüssel zu laden

---

## WSTG-SESS-11 — Prüfung auf gleichzeitige Sitzungen (Testing for Concurrent Sessions)

Die Prüfung auf gleichzeitige Sitzungen (Concurrent Sessions) bewertet, ob eine Anwendung es einem einzelnen Benutzerkonto ermöglicht, mehrere simultane aktive Sitzungen über verschiedene Browser, Geräte oder IP-Adressen hinweg aufrechtzuerhalten. Das Versäumnis, gleichzeitige Sitzungen zu beschränken, vergrößert das Zeitfenster für Angreifer, um gekaperte Session Tokens oder kompromittierte Anmeldedaten zu nutzen, ohne den rechtmäßigen Benutzer zu alarmieren oder bestehende Sitzungen zu beenden. Dieses Verhalten wird in der Regel durch Authentifizierung von mehreren Quellen und die Überwachung der Sitzungsstabilität bewertet, was häufig Schwachstellen in der Sitzungsmanagement-Logik oder fehlende sicherheitsorientierte Geschäftsregeln aufdeckt. Aus der Sicht eines Angreifers ermöglicht das Fehlen einer Concurrency Control eine unauffällige Persistenz nach einer ersten Kompromittierung und erleichtert parallele automatisierte Angriffe.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Werden gleichzeitige Sitzungen durch die Anwendungsrichtlinie begrenzt?
- [ ] Ja — nur eine Sitzung **ist zulässig** pro Benutzer; neue Logins machen alte ungültig *(Am sichersten)*  
- [ ] Ja — eine feste Anzahl gleichzeitiger Sitzungen **wird erzwungen** und kann nicht überschritten werden  
- [ ] Nein — eine unbegrenzte Anzahl gleichzeitiger Sitzungen **ist möglich**  

### Wie reagiert die Anwendung, wenn das Sitzungslimit erreicht ist?
- [ ] Die älteste aktive Sitzung **wird automatisch beendet** (Mitigation von Session Fixation/Hijacking)  
- [ ] Der Benutzer **wird aufgefordert**, bestehende Sitzungen zu beenden, bevor die neue Sitzung autorisiert wird  
- [ ] Der neue Login-Versuch **wird blockiert**, bis eine manuelle Abmeldung von einem anderen Gerät erfolgt  
- [ ] Keine Maßnahmen — mehrere Sitzungen bleiben gleichzeitig **aktiviert**  

### Bietet die Anwendung eine Schnittstelle zur Sitzungsverwaltung für den Benutzer an?
- [ ] Ja — Benutzer **können** aktive Sitzungen einsehen (IP, Gerät, Zeit) und diese remote beenden  
- [ ] Ja — Benutzer **können** aktive Sitzungen einsehen, diese aber **nicht** remote beenden  
- [ ] Nein — Benutzer **können** keine anderen aktiven Sitzungen ihres Kontos sehen oder verwalten  

### Wird der Benutzer benachrichtigt, wenn gleichzeitige Login-Aktivitäten erkannt werden?
- [ ] Ja — ein Alarm (E-Mail, SMS oder In-App) **wird ausgelöst** bei Logins von neuen Geräten oder Standorten  
- [ ] Nein — es **erfolgt keine Benachrichtigung**, wenn gleichzeitige Sitzungen aufgebaut werden

---

## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Reflected Cross Site Scripting (XSS) tritt auf, wenn eine Anwendung nicht vertrauenswürdige Daten ohne ausreichende Validierung oder Kodierung in eine HTTP-Antwort einfügt, wodurch der Payload im Browser-Kontext des Opfers ausgeführt wird. Angreifer übermitteln bösartige Payloads mittels Social Engineering, in der Regel durch manipulierte URLs oder Formulare, um Benutzersitzungen zu kapern, sensible Cookies zu exfiltrieren oder unbefugte Aktionen im Namen des Benutzers durchzuführen. Diese Schwachstelle tritt häufig in Suchparametern, Fehlermeldungen und allen Endpunkten auf, die Eingaben direkt an die Benutzeroberfläche zurückgeben. Die Ausnutzung setzt voraus, dass das Opfer mit einem schädlichen Link interagiert, was es zu einem nicht-persistenten, aber hochwirksamen Angriffsvektor gegen spezifische administrative oder authentifizierte Benutzer macht.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Werden vom Benutzer bereitgestellte Eingaben im Response Body reflektiert?
- [ ] Nein — Eingaben werden **nie** an den Benutzer zurückgegeben  
- [ ] Ja — Eingaben werden reflektiert, sind jedoch ordnungsgemäß kodiert und ein Bypass ist **nicht möglich**  
- [ ] Ja — Eingaben werden **ohne** Kodierung oder Bereinigung reflektiert  

### Ist eine kontextsensitive Ausgabekodierung implementiert?
- [ ] Ja — eine korrekte Kodierung (HTML, Attribute, JavaScript) wird basierend auf dem spezifischen Reflexionskontext **angewendet**  
- [ ] Ja — eine Kodierung wird **angewendet**, ist jedoch für den spezifischen Kontext unzureichend (z. B. HTML-Kodierung innerhalb eines `<script>`-Tags)  
- [ ] Nein — es wird **keine** Kodierung angewendet  

### Können Eingabevalidierungen oder Filter der Web Application Firewall (WAF) umgangen werden?
- [ ] Nein — Filter blockieren effektiv alle gängigen und obfuskerten XSS-Payloads  
- [ ] Ja — Filter sind vorhanden, aber ein Bypass **ist möglich** durch Zeichenkodierung, nicht-standardkonforme Tags oder Polyglots  
- [ ] Nein — es sind keine Filter oder Validierungsmechanismen vorhanden  

### Was ist der Ausführungskontext der reflektierten Eingabe?
- [ ] Sicher — die Eingabe wird an einer nicht ausführbaren Stelle reflektiert (z. B. innerhalb von standardmäßigen `<div>`- oder `<span>`-Tags)  
- [ ] Riskant — die Eingabe wird innerhalb von HTML-Attributen oder in den `src`/`href`-Attributen von Tags reflektiert  
- [ ] Kritisch — die Eingabe wird direkt in `<script>`-Blöcken, Event-Handlern oder Template-Literals reflektiert  

### Sind moderne Browser-Sicherheits-Header vorhanden, um die Auswirkungen von XSS zu minimieren?
- [ ] Ja — `Content-Security-Policy` (CSP) ist mit einer restriktiven script-src **aktiviert**  
- [ ] Ja — `Content-Security-Policy` (CSP) ist **aktiviert**, enthält jedoch `unsafe-inline` oder schwache Whitelists  
- [ ] Nein — es sind weder CSP- noch veraltete `X-XSS-Protection`-Header vorhanden

---

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

## WSTG-INPV-04 — Prüfung auf HTTP Parameter Pollution

HTTP Parameter Pollution (HPP) tritt auf, wenn eine Anwendung mehrere HTTP-Parameter mit demselben Namen erhält und diese auf inkonsistente oder unsichere Weise verarbeitet. Durch die Bereitstellung doppelter Parameter kann ein Angreifer die interne Logik der Anwendung manipulieren und potenziell Web Application Firewalls (WAF), Eingabevalidierungsfilter oder Zugriffskontrollmechanismen umgehen. Diese Schwachstelle ist stark vom zugrunde liegenden Webserver oder Anwendungs-Framework abhängig, welches entweder das erste, das letzte oder eine verkettete Version der wiederholten Parameter wählen kann. Die Ausnutzung zielt in der Regel auf Endpunkte ab, die Parameter an Backend-APIs, Datenbankabfragen oder URL-Weiterleitungsmechanismen übergeben, was zu Auswirkungen von Cross-Site Scripting (XSS) bis hin zu unbefugter Datenexfiltration führen kann.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Wie verarbeitet die Anwendung mehrere HTTP-Parameter mit demselben Namen?
- [ ] Nein — doppelte Parameter werden vom Server **abgelehnt** oder ignoriert  
- [ ] Ja — der Server wählt konsistent nur das **erste** oder **letzte** Vorkommen aus  
- [ ] Ja — der Server **verkettet** Werte, was potenziell Injektionen ermöglicht  

### Kann HPP genutzt werden, um Sicherheitskontrollen wie eine WAF oder Eingabefilter zu umgehen?
- [ ] Nein — Sicherheitskontrollen prüfen korrekt **alle** Vorkommen eines Parameters  
- [ ] Ja — eine WAF oder ein Filter prüft nur das **erste** Vorkommen, wodurch ein bösartiges **zweites** Vorkommen passieren kann  
- [ ] Ja — die Verkettung ermöglicht es einem Angreifer, ein Payload über mehrere Parameter zu **verteilen**, um die Erkennung zu umgehen  

### Gibt die Anwendung manipulierte Parameter in der Antwort ohne ordnungsgemäße Bereinigung zurück?
- [ ] Nein — Parameter werden bereinigt oder kodiert, bevor sie in der UI ausgegeben werden  
- [ ] Ja — Pollution führt zu reflektierten Inhalten, aber eine Bereinigung **wird angewendet**  
- [ ] Ja — Pollution führt zu reflektierten Inhalten und eine Bereinigung **wird nicht angewendet**, was zu XSS oder Logikfehlern führt  

### Sind Parameter, die an interne API-Aufrufe oder Backend-Systeme übergeben werden, anfällig für Pollution?
- [ ] Nein — Backend-Anfragen verwenden sichere, nicht-dynamische Konstruktionsmethoden  
- [ ] Ja — HPP ermöglicht es einem Angreifer, Parameter in der Backend-Anfragestruktur zu **injizieren** oder zu **überschreiben**  

### Zeigt die Anwendung ein unterschiedliches Verhalten, wenn Parameter in GET- vs. POST-Anfragen manipuliert werden?
- [ ] Nein — das Verhalten ist über alle HTTP-Methoden hinweg konsistent  
- [ ] Ja — nur GET-Parameter sind anfällig für Pollution  
- [ ] Ja — nur POST-Parameter (Body) sind anfällig für Pollution

---

## WSTG-INPV-05 — SQL Injection

SQL Injection tritt auf, wenn vom Benutzer bereitgestellte Eingaben ohne ordnungsgemäße Parametrisierung oder Bereinigung in SQL-Abfragen einbezogen werden, was es Angreifern ermöglicht, die Abfragelogik zu manipulieren. Eine erfolgreiche Ausnutzung kann zu unbefugter Datenexfiltration, Authentifizierungsumgehung, Datenmanipulation und in einigen Fällen zu Remote Code Execution über Datenbankfunktionen wie `xp_cmdshell` oder `LOAD_FILE()` führen. Diese Schwachstelle betrifft häufig Login-Formulare, Suchfunktionen, REST API-Parameter, HTTP-Header und alle Endpunkte, die dynamische SQL-Abfragen aus benutzergesteuerten Eingaben erstellen. Aus der Sicht eines Angreifers ist die Identifizierung eines Injection-Punkts das primäre Tor zur vollständigen Kompromittierung der Datenbank und zu potenziellem Lateral Movement innerhalb der Infrastruktur.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Test-Status** | Nicht durchgeführt |
| **Schweregrad** | Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Tools:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Werden benutzergesteuerte Parameter mit sicheren Datenbankzugriffsmethoden verarbeitet?
- [ ] Ja — Die Anwendung verwendet ausschließlich ORM oder parametrisierte Abfragen, und ein Bypass ist **nicht möglich**  
- [ ] Ja — Parametrisierte Abfragen werden verwendet, aber ein Bypass **ist möglich** über Randfälle (z. B. `ORDER BY`-Klauseln)  
- [ ] Nein — Zur Erstellung von SQL-Abfragen wird eine einfache String-Verkettung **ohne** Parametrisierung verwendet  

### Kann der Authentifizierungsmechanismus über SQL Injection umgangen werden?
- [ ] Nein — Login-Parameter sind **nicht** anfällig für Injection  
- [ ] Ja — Eine Authentifizierungsumgehung **ist möglich** unter Verwendung von Tautologie-basierten Payloads (z. B. `' OR 1=1 --`)  

### Ist eine Datenexfiltration über In-Band- oder Out-of-Band-Techniken möglich?
- [ ] Nein — Es wurde keine Datenexfiltration identifiziert  
- [ ] Ja — UNION-basierte oder Error-basierte Exfiltration **ist möglich**  
- [ ] Ja — Blinde (Boolean- oder zeitbasierte) Exfiltration **ist möglich**  
- [ ] Ja — Out-of-Band (DNS/HTTP) Exfiltration **ist möglich**  

### Weisen Anwendungsfunktionen Second-Order SQL Injection Schwachstellen auf?
- [ ] Nein — Gespeicherte Daten werden sicher verarbeitet, wenn sie für spätere Abfragen abgerufen werden  
- [ ] Ja — Benutzereingaben werden gespeichert und später in einer unsicheren SQL-Abfrage **ohne** Validierung verwendet  

### Sind die Berechtigungen des Datenbank-Dienstkontos auf das erforderliche Minimum beschränkt?
- [ ] Ja — Das Dienstkonto verfügt über **Least Privilege** (z. B. beschränkt auf bestimmte Tabellen/Aktionen)  
- [ ] Nein — Das Dienstkonto verfügt über erweiterte Berechtigungen (z. B. `DROP TABLE`, `FILE`-Berechtigungen oder `xp_cmdshell` ist **aktiviert**)

---

## WSTG-INPV-06 — Testing for LDAP Injection

Eine LDAP-Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Daten in LDAP-Filter (Lightweight Directory Access Protocol) einbindet, ohne diese ordnungsgemäß zu bereinigen oder zu maskieren (Escaping), wodurch ein Angreifer die Abfragelogik manipulieren kann. Durch das Injizieren von Sonderzeichen wie Sternchen, Klammern und logischen Operatoren können Angreifer den Suchfilter modifizieren, um Authentifizierungsmechanismen zu umgehen oder sensible Verzeichnisinformationen wie Benutzernamen, Gruppenmitgliedschaften und Organisationsattribute zu exfiltrieren. Diese Schwachstelle tritt typischerweise in Funktionen wie Unternehmensverzeichnissuchen, Mitarbeiterportalen oder Single Sign-On (SSO) Systemen auf, die mit Active Directory oder OpenLDAP interagieren. Aus der Sicht eines Angreifers beinhaltet eine erfolgreiche Ausnutzung oft Blind-Techniken, um die Verzeichnisstruktur oder Attributwerte Bit für Bit zu enumerieren, wenn die direkte Abfrageausgabe von der Anwendung unterdrückt wird.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Test Status** | Not Performed |
| **Schweregrad** | High |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### Nutzt die Anwendung LDAP für die Authentifizierung oder Verzeichnissuchen?
- [ ] Nein — LDAP wird in der Anwendungsarchitektur **nicht verwendet**  
- [ ] Ja — LDAP wird verwendet und alle Eingaben werden streng bereinigt oder parametrisiert  
- [ ] Ja — LDAP wird verwendet und Benutzereingaben werden **ohne** ordnungsgemäßes Escaping in Filter verkettet  

### Werden LDAP-Metazeichen in Benutzereingaben ordnungsgemäß maskiert oder gefiltert?
- [ ] Ja — Zeichen wie `(`, `)`, `&`, `|`, `*` und `\` sind **nicht erlaubt** oder werden korrekt maskiert  
- [ ] Nein — Metazeichen **können** injiziert werden, um die Struktur des LDAP-Filters zu verändern  

### Kann der Authentifizierungsmechanismus durch Injection umgangen werden?
- [ ] Nein — Die Login-Logik ist **nicht anfällig** für Abfagemanipulationen  
- [ ] Ja — Die Authentifizierung **kann** durch die Verwendung von logischem ODER (`|`) oder Wildcard-Injection (`*`) in den Benutzernamen- oder Passwortfeldern umgangen werden  

### Ist eine Blind-Datenexfiltration aus dem Verzeichnisdienst möglich?
- [ ] Nein — Die Suchergebnisse sind begrenzt und es werden keine Zeitunterschiede oder booleschen Differenzen beobachtet  
- [ ] Ja — Verzeichnisattribute **können** Bit für Bit über boolesche Blind-Injection-Techniken enumeriert werden  
- [ ] Ja — Vollständige Verzeichnisdatensätze **werden** aufgrund von Abfragemanipulation in der Antwort der Anwendung widergespiegelt  

### Sind sekundäre Anwendungskomponenten (z. B. Mailer, SSO) anfällig für LDAP-basierte Attribut-Injection?
- [ ] Nein — LDAP-Attribute werden in allen Komponenten sicher verarbeitet  
- [ ] Ja — Ein Angreifer **kann** seine eigenen Attribute (z. B. E-Mail, Gruppenmitgliedschaft) mittels Injection ändern, um Berechtigungen zu eskalieren oder die Kommunikation umzuleiten

---

## WSTG-INPV-07 — XML Injection

XML Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Daten in ein XML-Dokument oder einen Stream einfügt, ohne eine ordnungsgemäße Validierung, Bereinigung (Sanitization) oder Maskierung (Escaping) von XML-Metazeichen durchzuführen. Durch das Injizieren spezifischer XML-Tags oder Strukturelemente kann ein Angreifer die Logik des Dokuments manipulieren, Authentifizierungsmechanismen umgehen oder die Integrität der vom Backend verarbeiteten Daten beeinträchtigen. Diese Schwachstelle tritt häufig in SOAP-basierten Webdiensten, REST APIs, die XML akzeptieren, SAML-Assertions sowie in Anwendungen auf, die XML-Konfigurationsdateien oder Berichte dynamisch generieren. Aus der Sicht eines Angreifers besteht das Ziel darin, aus dem vorgesehenen Datenkontext auszubrechen, um die Dokumentstruktur zu verändern, was potenziell zu einer unbefugten Ausweitung von Berechtigungen (Privilege Escalation) oder der Ausführung von unbeabsichtigter Geschäftslogik führen kann.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Tools:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### Akzeptiert und verarbeitet die Anwendung vom Benutzer bereitgestellte Eingaben im XML-Format?
- [ ] Nein — die Anwendung verarbeitet keine XML-Eingaben  
- [ ] Ja — XML-Eingaben werden über APIs (SOAP/REST) verarbeitet  
- [ ] Ja — XML wird über Datei-Uploads (SVG, DOCX usw.) verarbeitet  

### Werden XML-Metazeichen (z. B. `<`, `>`, `&`, `'`, `"`) ordnungsgemäß maskiert oder bereinigt?
- [ ] Ja — alle Metazeichen werden **ordnungsgemäß** maskiert oder bereinigt *(Am sichersten)*  
- [ ] Ja — es wird eine gewisse Filterung angewendet, aber ein Bypass ist mittels Kodierung **möglich**  
- [ ] Nein — Roheingaben werden **ohne** Bereinigung direkt in XML-Strukturen eingebettet  

### Wird eine serverseitige Schema-Validierung (XSD/DTD) für alle XML-Eingaben erzwungen?
- [ ] Ja — eine strikte Schema-Validierung ist **aktiviert** und verhindert strukturelle Änderungen  
- [ ] Ja — die Validierung ist **aktiviert**, verhindert jedoch **keine** Tag-Injection innerhalb von Datenfeldern  
- [ ] Nein — die Schema-Validierung ist **deaktiviert** oder nicht implementiert  

### Kann ein Angreifer neue XML-Tags injizieren, um die Struktur oder Logik des Dokuments zu verändern?
- [ ] Nein — die Struktur bleibt unabhängig von der Eingabe intakt  
- [ ] Ja — Tags können injiziert werden, können aber die Geschäftslogik **nicht** verändern  
- [ ] Ja — eine Tag-Injection **ist möglich** und erlaubt die Umgehung der Logik oder Datenmodifikation *(Kritisch)*  

### Kann der XML-Parser mithilfe von CDATA-Abschnitten oder XML-Kommentaren manipuliert werden?
- [ ] Nein — der Parser verarbeitet oder lehnt CDATA-/Kommentar-Markierungen korrekt ab  
- [ ] Ja — die Injection von `<![CDATA[...]]>` oder `<!-- ... -->` **ist möglich**, um Filter zu verbergen oder zu umgehen

---

## WSTG-INPV-08 — Prüfung auf SSI Injection

Server-Side Includes (SSI) Injection tritt auf, wenn eine Anwendung Benutzereingaben nicht ausreichend bereinigt, bevor sie diese in HTML-Dateien einfügt, die von der SSI-Engine des Servers verarbeitet werden. Angreifer nutzen dies aus, indem sie SSI-Direktiven wie `<!--#exec cmd="id" -->` injizieren, um beliebige Shell-Befehle auszuführen, lokale Dateien zu lesen oder Umgebungsvariablen zu exfiltrieren. Diese Schwachstelle findet sich typischerweise in Anwendungen, die veraltete Dateierweiterungen wie `.shtml`, `.shtm` oder `.stm` verwenden, oder wenn der Webserver explizit so konfiguriert ist, dass er SSI innerhalb von Standard-HTML-Dateien parst. Eine erfolgreiche Ausnutzung gewährt dem Angreifer dieselben Berechtigungen wie dem Webserver-Prozess, was oft zu einer vollständigen Systemkompromittierung oder der Offenlegung sensibler Daten führt.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Unterstützt und verarbeitet der Webserver SSI-Direktiven?
- [ ] Nein — der Server unterstützt kein SSI oder Dateierweiterungen wie `.shtml` sind deaktiviert  
- [ ] Ja — SSI ist aktiviert, aber Benutzereingaben werden nicht in den verarbeiteten Seiten reflektiert  
- [ ] Ja — SSI ist aktiviert und Benutzereingaben werden in den verarbeiteten Seiten reflektiert  

### Können beliebige Systembefehle über die `#exec`-Direktive ausgeführt werden?
- [ ] Nein — die `#exec`-Direktive ist deaktiviert oder durch die Serverkonfiguration eingeschränkt  
- [ ] Ja — die Befehlsausführung ist über `#exec cmd` oder `#exec cgi` Payloads möglich  

### Ist die Exfiltration von sensiblen Dateien oder Umgebungsvariablen möglich?
- [ ] Nein — Direktiven wie `#include` oder `#config` sind deaktiviert  
- [ ] Ja — das Lesen lokaler Dateien (z. B. `/etc/passwd`) ist über `#include virtual` möglich  
- [ ] Ja — Server-Umgebungsvariablen können über `#printenv` oder `#echo` exfiltriert werden  

### Sind Eingabevalidierung oder WAF-Kontrollen gegen SSI-Payloads wirksam?
- [ ] Ja — eine strikte Eingabevalidierung wird angewendet und ein Bypass ist nicht möglich  
- [ ] Ja — Validierung/WAF wird angewendet, aber ein Bypass ist unter Verwendung von Zeichenkodierung oder unterschiedlicher SSI-Syntax möglich  
- [ ] Nein — es ist keine Validierung oder kein WAF-Schutz für SSI-Zeichen (`<`, `!`, `#`, `-`, `"`) vorhanden

---

## WSTG-INPV-09 — Testing for XPath Injection

Eine XPath Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Informationen verwendet, um eine XPath-Abfrage für XML-Daten zu konstruieren, was es einem Angreifer ermöglicht, die Logik der Abfrage zu manipulieren. Durch das Einschleusen spezifischer Syntax wie einfache Anführungszeichen, Klammern und logische Operatoren kann ein Angreifer in der XML-Dokumentstruktur navigieren, die Authentifizierung umgehen oder sensible Datenknoten exfiltrieren. Diese Schwachstelle tritt häufig in Web-Services, Konfigurationsmanagement-Schnittstellen und Altsystemen (Legacy Systems) auf, die auf XML-basierten Datenspeichern anstelle von traditionellen SQL-Datenbanken basieren. Aus der Sicht eines Angreifers wird dies oft unter Verwendung von Boolean-basierten oder Error-basierten Techniken ausgenutzt, um den XML-Baum systematisch abzubilden und verborgene Werte aus dem Backend abzurufen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Tools:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Werden Benutzereingaben, die zur Konstruktion von XPath-Abfragen verwendet werden, ordnungsgemäß bereinigt oder parametrisiert?
- [ ] Ja — Eingaben werden **nicht** in XPath-Abfragen verwendet oder sind strikt parametrisiert  
- [ ] Ja — eine robuste Eingabevalidierung/-kodierung **wird angewendet** und ein Bypass ist **nicht möglich**  
- [ ] Ja — eine gewisse Validierung **wird angewendet**, aber ein Bypass ist mittels kodierter Zeichen **möglich**  
- [ ] Nein — Benutzereingaben werden direkt in XPath-Abfragen verkettet *(Kritisch)*  

### Offenbart die Anwendung strukturelle XML/XPath-Details über Fehlermeldungen?
- [ ] Nein — es werden generische Fehlerseiten angezeigt und **keine** XML-Details preisgegeben  
- [ ] Ja — Fehlermeldungen offenbaren das Vorhandensein von XML-Verarbeitung, aber **keine** Pfaddetails  
- [ ] Ja — ausführliche Fehlermeldungen offenbaren XPath-Syntax, Knotennamen oder Dateipfade  

### Ist es möglich, die Authentifizierungs- oder Autorisierungslogik mittels XPath-Logik zu umgehen?
- [ ] Nein — die Authentifizierungslogik basiert **nicht** auf XML/XPath oder ist sicher implementiert  
- [ ] Ja — Login- oder Berechtigungsprüfungen können mittels Logik-basierten Payloads wie `' or 1=1 or '` umgangen werden  

### Kann die XML-Dokumentstruktur oder die darin enthaltenen Daten über Blind Injection enumeriert werden?
- [ ] Nein — die Anwendung ist **nicht** anfällig für Boolean-basierte oder Zeit-basierte Rückschlüsse (Inference)  
- [ ] Ja — Knotennamen und Daten **können** mittels Boolean-basierter Rückschlüsse exfiltriert werden  
- [ ] Ja — eine vollständige XML-Tree-Traversal und Datenextraktion **ist möglich**

---

## WSTG-INPV-10 — IMAP SMTP Injection

IMAP- und SMTP-Injection-Schwachstellen entstehen, wenn eine Webanwendung benutzergelieferte Daten unzureichend filtert, bevor sie in Befehle integriert werden, die an einen Mailserver gesendet werden. Durch das Injizieren von Carriage-Return- und Line-Feed-Zeichen (CRLF) kann ein Angreifer den beabsichtigten Befehl beenden und beliebige Mail-Anweisungen anhängen, wie z. B. das Hinzufügen weiterer Empfänger (CC/BCC) oder das Modifizieren des Nachrichtentextes. Diese Schwachstellen treten am häufigsten in Web-to-Mail-Gateways, Kontaktformularen und E-Mail-Clients auf, die direkt über Protokolle wie IMAP4 oder SMTP mit Mailservern kommunizieren. Eine erfolgreiche Ausnutzung ermöglicht das unbefugte Weiterleiten (Relaying) von Spam, die Exfiltration sensibler E-Mail-Inhalte und in einigen Fällen die vollständige Kompromittierung der Integrität des Mailservers.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Verarbeitet die Anwendung vom Benutzer bereitgestellte Eingaben, um IMAP- oder SMTP-Befehle zu konstruieren?
- [ ] Nein — Die Anwendung interagiert **nicht** über Benutzereingaben mit Mailservern  
- [ ] Ja — Die Anwendung interagiert mit Mailservern, verwendet jedoch eine sichere API oder Bibliothek  
- [ ] Ja — Die Anwendung konstruiert Raw-Mail-Befehle unter Verwendung benutzergesteuerter Parameter  

### Werden Eingaben validiert, um die Injection von Carriage-Return- (`\r`) und Line-Feed-Sequenzen (`\n`) zu verhindern?
- [ ] Ja — CRLF-Zeichen werden **strikt** gefiltert, kodiert oder abgelehnt  
- [ ] Ja — Eingaben werden gegen eine strikte Whitelist (Allowlist) von Zeichen validiert  
- [ ] Nein — CRLF-Zeichen werden **nicht** gefiltert, was den Abbruch von Befehlen und Injection ermöglicht  

### Kann ein Angreifer zusätzliche Mail-Header wie `Bcc:` oder `Cc:` injizieren?
- [ ] Nein — Eine Header-Modifikation ist aufgrund strikter Eingabebeschränkungen **nicht möglich**  
- [ ] Ja — Die Injection zusätzlicher Header **ist** über CRLF-Sequenzen **möglich**  
- [ ] Ja — Mail-Relaying an externe Domains **ist aktiviert** und ausnutzbar *(Kritisch)*  

### Ermöglicht die Anwendung die Manipulation von IMAP-Befehlen, um auf nicht autorisierte Postfächer zuzugreifen?
- [ ] Nein — Die Anwendung verwendet **kein** IMAP oder beschränkt den Postfachzugriff strikt auf den authentifizierten Benutzer  
- [ ] Ja — IMAP-Command-Injection **ist möglich**, jedoch auf die Enumeration von Metadaten beschränkt  
- [ ] Ja — IMAP-Command-Injection ermöglicht das Lesen oder Löschen von E-Mails anderer Benutzer  

### Ist der Backend-Mailserver anfällig für Command-Pipelining?
- [ ] Nein — Der Mailserver lehnt gebündelte Befehle (Batched Commands) oder mehrere Befehle in einer einzigen Sitzung ab  
- [ ] Ja — Der Mailserver verarbeitet mehrere Befehle, die über ein einzelnes Eingabefeld injiziert wurden

---

## WSTG-INPV-11 — Code Injection

Code Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Code-Snippets innerhalb ihrer Laufzeitumgebung auswertet, typischerweise durch unsichere sprachspezifische Funktionen wie `eval()`, `exec()` oder `system()`. Diese Schwachstelle unterscheidet sich von Command Injection, da sie auf den Ausführungskontext der Programmiersprache (z. B. PHP, Python, Node.js) abzielt und nicht auf die zugrunde liegende Betriebssystem-Shell. Angreifer nutzen diese Stellen aus, um beliebige Logik auszuführen, Umgebungsvariablen zu exfiltrieren oder eine vollständige Remote Code Execution (RCE) auf dem Server zu erreichen. Pentesters sollten Funktionen untersuchen, die mathematische Ausdrücke, serverseitige Templates oder dynamische Konfigurationsparameter verarbeiten, bei denen Eingaben als ausführbarer Code interpretiert werden könnten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Werkzeuge:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Evaluieren Anwendungsfunktionen dynamische Eingaben über sprachspezifische Evaluationsfunktionen?
- [ ] Nein — dynamische Evaluationsfunktionen sind in der Codebasis nicht vorhanden  
- [ ] Ja — Funktionen wie `eval()`, `exec()` oder `include()` werden verwendet, akzeptieren jedoch keine Benutzereingaben  
- [ ] Ja — Funktionen werden verwendet und verarbeiten vom Benutzer bereitgestellte Eingaben  

### Werden Mechanismen zur Eingabevalidierung oder Bereinigung (Sanitization) vor der Evaluation auf Eingaben angewendet?
- [ ] Ja — die Eingabe wird strikt gegen eine hartkodierte Whitelist validiert  
- [ ] Ja — die Eingabe wird über Blacklisting bereinigt, jedoch ist ein Bypass möglich  
- [ ] Nein — die Eingabe wird direkt an die Evaluationsfunktion übergeben und es werden keine Kontrollen angewendet  

### Ist die Ausführungsumgebung in einer Sandbox isoliert oder eingeschränkt, um Zugriff auf Systemebene zu verhindern?
- [ ] Ja — die Ausführung erfolgt in einer stark eingeschränkten Sandbox, in der keine Systemaufrufe durchgeführt werden können  
- [ ] Ja — es existieren einige Einschränkungen, aber ein Sandbox Escape ist möglich  
- [ ] Nein — die Umgebung erlaubt vollen Zugriff auf die zugrunde liegenden Sprach-APIs und Betriebssystemfunktionen *(Kritisch)*  

### Kann die Code Injection zur Exfiltration sensibler Informationen genutzt werden?
- [ ] Nein — die Ausführung erfolgt blind und es ist keine Out-of-Band-Kommunikation möglich  
- [ ] Ja — Umgebungsvariablen oder lokale Dateien können über HTTP/DNS-Anfragen exfiltriert werden  
- [ ] Ja — Ergebnisse des ausgeführten Codes werden direkt in der Antwort der Anwendung reflektiert  

### Ermöglicht die Anwendung Remote File Inclusion oder das dynamische Laden von Bibliotheken?
- [ ] Nein — es können nur lokale, vordefinierte Codepfade ausgeführt werden  
- [ ] Ja — die Anwendung kann dazu gezwungen werden, Code von einer entfernten oder vom Angreifer kontrollierten Quelle zu laden und auszuführen

---

## WSTG-INPV-12 — Command Injection

Command Injection tritt auf, wenn eine Anwendung unsichere, vom Benutzer bereitgestellte Daten, wie Formulareingaben, HTTP-Header oder Cookies, an eine System-Shell übergibt. Diese Schwachstelle ermöglicht es einem Angreifer, beliebige Betriebssystembefehle auf dem Server auszuführen, in der Regel mit den Berechtigungen des Webanwendungsprozesses. Aus der Sicht eines Angreifers wird dies durch die Verwendung von Shell-Metacharacters wie Semikolons, Pipes oder Backticks ausgenutzt, um bösartige Befehle an den beabsichtigten Ausführungsfluss zu koppeln. Die Auswirkungen sind häufig katastrophal und führen zu einer vollständigen Systemkompromittierung, unbefugter Datenexfiltration oder Lateral Movement innerhalb des internen Netzwerks.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Tools:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### Ruft die Anwendung Befehle auf Systemebene oder Shell-Executables auf?
- [ ] Nein — die Anwendung interagiert **nicht** mit der OS-Shell  
- [ ] Ja — die Anwendung verwendet interne APIs oder High-Level-Bibliotheken für Systemaufgaben  
- [ ] Ja — die Anwendung ruft Shell-Befehle direkt über Systemaufrufe wie `system()`, `exec()` oder `popen()` auf  

### Werden Benutzereingaben validiert oder bereinigt, bevor sie an Systemaufrufe übergeben werden?
- [ ] Ja — striktes Allow-Listing und Eingabevalidierung **werden angewendet**  
- [ ] Ja — Bereinigung (Sanitization) oder Escaping wird verwendet, aber ein Bypass **ist möglich**  
- [ ] Nein — Benutzereingaben werden **ohne** Validierung direkt an die Shell übergeben  

### Können Shell-Metacharacters verwendet werden, um den Ablauf der Befehlsausführung zu manipulieren?
- [ ] Nein — Metacharacters werden korrekt gefiltert oder escaped  
- [ ] Ja — In-Band-Ausführung, bei der die Befehlsausgabe in der Antwort reflektiert wird, **ist möglich**  
- [ ] Ja — Blind-Ausführung, bei der keine Ausgabe reflektiert wird, **ist möglich**  

### Ist eine Out-of-Band (OOB) Exfiltration vom Host aus möglich?
- [ ] Nein — der Server hat keinen Egress oder OOB-Signale (DNS/HTTP) schlagen fehl  
- [ ] Ja — Datenexfiltration über DNS- oder HTTP-basierte OOB-Signale **ist möglich**  

### Wird der Anwendungsprozess mit erhöhten Systemberechtigungen ausgeführt?
- [ ] Nein — die Anwendung läuft unter einem dedizierten Dienstkonto mit geringen Berechtigungen  
- [ ] Ja — die Anwendung läuft als `root`, `Administrator` oder als Benutzer mit hohen Berechtigungen *(Kritisch)*

---

## WSTG-INPV-13 — Format String Injection

Format-String-Injection tritt auf, wenn eine Webanwendung benutzergesteuerte Eingaben direkt in das Format-String-Argument einer variadischen Funktion übergibt, wie z. B. die `printf`-Familie in C oder Low-Level-Logging-Wrapper. Obwohl diese Sicherheitslücke in modernen Managed-Code-Web-Frameworks seltener vorkommt, bleibt sie in Legacy-CGI-Anwendungen, nativen Erweiterungen oder Edge-Case-Logging-Systemen, die mit systemnahen Bibliotheken interagieren, kritisch. Angreifer nutzen dies aus, indem sie Formatspezifikatoren wie `%x` oder `%p` liefern, um sensible Speicheradressen und Daten aus dem Stack auszulesen (Leak), oder den `%n`-Spezifikator verwenden, um beliebige Schreibzugriffe auf den Speicher (Arbitrary Memory Writes) durchzuführen. Eine erfolgreiche Ausnutzung führt in der Regel zur vollständigen Systemübernahme durch Remote Code Execution (RCE) oder zumindest zu einem schwerwiegenden Denial-of-Service (DoS) durch Prozessabstürze.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Verarbeitet die Anwendung Benutzereingaben über nativen Code oder Legacy-CGI-Schnittstellen?
- [ ] Nein — die Anwendung basiert vollständig auf Managed-Code-Frameworks (z. B. JVM, CLR)  
- [ ] Ja — die Anwendung verwendet JNI, native C/C++-Erweiterungen oder binäre Legacy-CGIs, bei denen Format-String-Schwachstellen existieren **können**  

### Werden vom Benutzer bereitgestellte Eingaben als primäres Argument an format-sensitive Funktionen übergeben?
- [ ] Nein — Eingaben werden als Datenargumente übergeben, und Format-Strings sind **statisch** und **hartcodiert**  
- [ ] Ja — die Benutzereingabe wird direkt in das Format-String-Argument verkettet, aber eine Bereinigung (Sanitization) **wird angewendet**  
- [ ] Ja — die Benutzereingabe wird direkt als Format-String verwendet und es **wird keine** Bereinigung angewendet  

### Ist es möglich, Speicherinhalte mithilfe von Formatspezifikatoren auszulesen?
- [ ] Nein — die Eingabe wird validiert und Spezifikatoren wie `%p`, `%x` oder `%s` werden **nicht verarbeitet**  
- [ ] Ja — die Angabe von `%p`- oder `%x`-Spezifikatoren führt zur Reflektion von Stack- oder Heap-Speicheradressen  
- [ ] Ja — sensible Daten (z. B. Pointer, Stack Cookies) **können** über Formatspezifikatoren exfiltriert werden  

### Kann die Anwendung mithilfe des `%n`-Spezifikators zum Absturz gebracht oder manipuliert werden?
- [ ] Nein — Spezifikatoren, die Schreibzugriffe auf den Speicher erlauben, sind **deaktiviert** oder werden vom Compiler/der Umgebung blockiert  
- [ ] Ja — die Anwendung stürzt ab (DoS), wenn `%s%s%s%s` oder ähnliche Zeichenfolgen bereitgestellt werden  
- [ ] Ja — beliebige Speicherzugriffe über `%n` sind **möglich**, was potenziell zu Remote Code Execution (RCE) führt  

### Sind moderne Binärschutzmechanismen (ASLR, DEP, Stack Canaries) durch diese Injection umgehbar?
- [ ] Nein — die Umgebung ist gehärtet und Speicherleaks **können nicht** zum Umgehen von Schutzmechanismen verwendet werden  
- [ ] Ja — Format-String-Injection **kann** verwendet werden, um Basisadressen auszulesen, wodurch ein ASLR/DEP-Bypass **möglich** wird

---

## WSTG-INPV-14 — Prüfung auf inkubierte Schwachstellen

Inkubierte Schwachstellen (Incubated Vulnerabilities), auch als persistente oder Second-Order-Schwachstellen bezeichnet, treten auf, wenn ein bösartiger Payload von der Anwendung in einem „ruhenden“ Zustand gespeichert und später in einem anderen Kontext ausgeführt oder verarbeitet wird. Dieses Angriffsmuster zielt typischerweise auf Back-End-Systeme, Administrationskonsolen oder automatisierte Reporting-Tools ab, die Daten aus einer gemeinsamen Datenbank oder einem Dateisystem abrufen, ohne eine angemessene Re-Validierung oder ein Output-Encoding durchzuführen. Pentester müssen den Lebenszyklus von Benutzereingaben vom Eintrittspunkt (dem „Inkubator“) bis zu jedem Ort verfolgen, an dem diese Daten schließlich gerendert oder von anderen Komponenten oder sekundären Anwendungen genutzt werden. Eine erfolgreiche Ausnutzung führt häufig zu weitreichenden Folgen wie dem Hijacking von Administrationssitzungen über Stored XSS oder Datenexfiltration durch Second-Order SQL Injection in internen Reporting-Modulen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Tools:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Gibt es Endpunkte, an denen benutzergesteuerte Eingaben für den späteren Abruf durch andere Benutzer oder Prozesse gespeichert werden?
- [ ] Nein — Die Anwendung speichert **keine** Benutzereingaben für die spätere Verwendung  
- [ ] Ja — Eingaben **werden** in Datenbankfeldern, Logdateien oder Konfigurationseinstellungen gespeichert  

### Wird eine Eingabevalidierung oder Bereinigung (Sanitization) angewendet, bevor Daten in die persistente Speicherschicht geschrieben werden?
- [ ] Ja — Eine strikte Allow-List-Validierung **wird** am Eintrittspunkt angewendet  
- [ ] Ja — Eine generische Bereinigung **wird** angewendet, aber ein Bypass **ist möglich** durch Codierung oder Nicht-Standard-Zeichen  
- [ ] Nein — Eingaben werden in ihrer rohen, unvalidierten Form gespeichert  

### Werden gespeicherte Daten ordnungsgemäß codiert oder bereinigt, wenn sie in administrativen oder sekundären Schnittstellen gerendert werden?
- [ ] Ja — Kontextsensitives Output-Encoding **wird** überall dort angewendet, wo die Daten gerendert werden  
- [ ] Ja — Das Encoding **wird** an einigen Stellen angewendet, **fehlt** jedoch an anderen (z. B. im internen Admin-Dashboard)  
- [ ] Nein — Daten werden ohne jegliches Encoding oder Sanitization gerendert, was die Ausführung von Payloads ermöglicht  

### Können in der Datenbank gespeicherte Payloads Backend-Batch-Prozesse oder Out-of-Band-Monitoringsysteme beeinträchtigen?
- [ ] Nein — Backend-Prozesse verwenden sichere Parsing-Methoden oder Parameterized Queries  
- [ ] Ja — Gespeicherte Payloads **können** Interaktionen in der Backend-Logik auslösen (z. B. CSV Injection, Command Injection in Logs)  
- [ ] Ja — Gespeicherte Payloads **können** Out-of-Band (OOB) Anfragen an vom Angreifer kontrollierte Server auslösen

---

## WSTG-INPV-15 — HTTP Splitting Smuggling

HTTP Splitting und Smuggling Schwachstellen entstehen durch Diskrepanzen in der Art und Weise, wie Frontend-Proxies und Backend-Server HTTP-Anfragegrenzen interpretieren und verarbeiten, insbesondere in Bezug auf `Content-Length` und `Transfer-Encoding` Header. Durch das Erstellen mehrdeutiger Anfragen kann ein Angreifer eine versteckte Anfrage an das Backend „schmuggeln“ oder CRLF-Sequenzen injizieren, um eine Antwort aufzuteilen, was zu Cache Poisoning, Request Hijacking oder dem Umgehen von Sicherheitskontrollen führt. Diese Fehler treten typischerweise in komplexen Umgebungen auf, die Reverse Proxies, Load Balancer oder CDNs verwenden, welche eine inkonsistente Parsing-Logik aufweisen. Aus der Sicht eines Angreifers ermöglicht dies die Umleitung des Benutzerverkehrs, den Diebstahl sensibler Session-Token und die Ausführung unbefugter Aktionen im Kontext der Sitzungen anderer Benutzer.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Werkzeuge:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Ist die Umgebung anfällig für Request Smuggling über CL.TE- oder TE.CL-Diskrepanzen?
- [ ] Nein — Frontend- und Backend-Server verarbeiten Anfragegrenzen konsistent  
- [ ] Ja — Diskrepanzen existieren, aber eine Ausnutzung ist aufgrund von Infrastruktur-Mitigationen **nicht möglich**  
- [ ] Ja — CL.TE oder TE.CL Smuggling **ist möglich**, wodurch versteckte Anfragen das Backend erreichen können *(Hoch)*  
- [ ] Ja — TE.TE (Double Encoding/Obfuscation) **ist möglich**, um Frontend-Filter zu umgehen *(Kritisch)*  

### Kann HTTP Response Splitting durch CRLF-Injection in Headern erreicht werden?
- [ ] Nein — die Anwendung bereinigt CRLF-Sequenzen in allen Header-Eingaben ordnungsgemäß  
- [ ] Ja — CRLF-Sequenzen werden in Headern reflektiert, aber Response Splitting ist **nicht möglich**  
- [ ] Ja — CRLF-Injection **ist möglich**, was Header-Injection oder Cache Poisoning erlaubt  

### Werden Sicherheitskontrollen (WAF/ACLs) mithilfe von geschmuggelten Anfragen umgangen?
- [ ] Nein — Sicherheitskontrollen gelten sowohl für äußere als auch für geschmuggelte Anfragen  
- [ ] Ja — geschmuggelte Anfragen **können** Frontend-WAF-Regeln oder IP-basierte ACLs umgehen  

### Ist es möglich, Sitzungen anderer Benutzer zu übernehmen (Hijacking) oder deren Datenverkehr umzuleiten?
- [ ] Nein — Request/Response-Streams sind isoliert und können nicht gekreuzt werden  
- [ ] Ja — Request Smuggling **ist möglich** und erlaubt das Erfassen von Anfragen anderer Benutzer *(Kritisch)*  
- [ ] Ja — Response Splitting **ist möglich** und erlaubt Browser-seitiges Cache Poisoning oder XSS

---

## WSTG-INPV-16 — HTTP Request Smuggling

HTTP Request Smuggling (HRS) tritt auf, wenn eine Kette von HTTP-Servern (wie z. B. ein Loadbalancer und ein Backend-Webserver) die Grenzen einer Anfrage unterschiedlich interpretiert, was in der Regel auf widersprüchliche `Content-Length`- und `Transfer-Encoding`-Header zurückzuführen ist. Ein Angreifer nutzt diese Desynchronisation aus, um einen Eintrag in den Anfrage-Puffer des Backend-Servers einzuschleusen („smuggling“), wodurch der nächsten legitimen Benutzeranfrage effektiv ein vom Angreifer kontrolliertes Segment vorangestellt wird. Diese Technik ermöglicht schwerwiegende Angriffe, einschließlich Session Hijacking, die Umgehung von Sicherheitskontrollen und Cache Poisoning. Die Ausnutzung erfolgt üblicherweise durch zeitbasierte Analysen (timing-based analysis) oder durch die Beobachtung unerwarteter Antworten vom Backend, wenn nachfolgende Anfragen an den Server gesendet werden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Kritisch / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Werkzeuge:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Verarbeiten die Frontend- und Backend-Server den `Transfer-Encoding: chunked`-Header konsistent?
- [ ] Ja — beide Server verarbeiten Chunked Encoding identisch und eine Desynchronisation ist **nicht möglich**  
- [ ] Nein — Server interpretieren Header unterschiedlich, aber eine Bereinigung/Normalisierung **wird angewendet**  
- [ ] Nein — CL.TE (Content-Length/Transfer-Encoding) Desynchronisation **ist möglich** *(Kritisch)*  
- [ ] Nein — TE.CL (Transfer-Encoding/Content-Length) Desynchronisation **ist möglich** *(Kritisch)*  

### Kann ein Angreifer den serverseitigen oder clientseitigen Cache mittels einer eingeschleusten Anfrage vergiften (Cache Poisoning)?
- [ ] Nein — Caching ist **deaktiviert** oder nicht anfällig für Poisoning  
- [ ] Ja — eingeschleuste Anfragen **können** Benutzer auf bösartige Domains umleiten oder Skripte via Cache Poisoning injizieren  

### Ist es möglich, Anfragen anderer Benutzer oder Session-Token durch Request Concatenation zu erfassen?
- [ ] Nein — Smuggling ermöglicht **keine** benutzerübergreifende Datenexposition  
- [ ] Ja — Smuggling ermöglicht es, die Anfrage des nächsten Benutzers an einen vom Angreifer kontrollierten `POST`-Body anzuhängen, wodurch eine Exfiltration **möglich ist**  

### Nutzt die Infrastruktur HTTP/2 und ist sie anfällig für H2.CL- oder H2.TE-Desynchronisation?
- [ ] Nein — die Anwendung nutzt nur HTTP/1.1 oder HTTP/2 ist sicher implementiert, ohne dass ein Downgrade stattfindet  
- [ ] Ja — HTTP/2-zu-HTTP/1.1-Cleartext-Downgrades treten auf und eine Desynchronisation **ist möglich**  

### Werden fehlerhafte Header (z. B. `Transfer-Encoding:  chunked` mit einem Leerzeichen) sicher gehandhabt?
- [ ] Ja — fehlerhafte Header werden über alle Ebenen hinweg abgelehnt oder normalisiert  
- [ ] Nein — TE.TE-Desynchronisation **ist möglich** aufgrund inkonsistenter Handhabung von fehlerhaften oder verschleierten Headern

---

## WSTG-INPV-17 — Prüfung auf Host Header Injection

Host Header Injection tritt auf, wenn eine Anwendung dem vom Benutzer bereitgestellten `Host`-Header vertraut und diesen ohne ausreichende Validierung in die serverseitige Logik einbezieht, wie z. B. bei der Link-Generierung oder bei Weiterleitungen. Angreifer manipulieren diesen Header, um Web Cache Poisoning zu begünstigen, Password Reset Poisoning durch Umleitung sensibler Token an eine externe Domain zu ermöglichen oder Sicherheitskontrollen zu umgehen, die auf Header-basiertem Routing beruhen. Eine erfolgreiche Ausnutzung ermöglicht es einem Angreifer, Sitzungsdaten zu exfiltrieren, ein Account Takeover durchzuführen oder ahnungslose Benutzer auf bösartige Infrastrukturen umzuleiten. Diese Schwachstelle tritt am häufigsten in Umgebungen mit Load-Balancern oder in Anwendungen auf, die absolute URLs dynamisch basierend auf dem Kontext der eingehenden Anfrage generieren.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel / Hoch* |

> *Die Kritikalität wird als Hoch eingestuft, wenn der Host-Header zur Generierung sensibler Links in E-Mails verwendet wird, wie z. B. beim Zurücksetzen von Passwörtern.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Tools:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### Spiegelt die Anwendung den Wert des `Host`-Headers in Links, Weiterleitungen oder Skripten wider?
- [ ] Nein — Die Anwendung verwendet fest kodierte Basis-URLs oder eine streng validierte Konfiguration  
- [ ] Ja — Der `Host`-Header wird gespiegelt, aber streng gegen eine Whitelist validiert  
- [ ] Ja — Der `Host`-Header wird gespiegelt und ein Bypass **ist möglich** durch fehlerhafte Werte (z. B. `expected.com:attacker.com`)  
- [ ] Ja — Der `Host`-Header wird **ohne** Validierung gespiegelt  

### Werden alternative host-bezogene Header von der Anwendung oder Infrastruktur verarbeitet?
- [ ] Nein — Header wie `X-Forwarded-Host`, `X-Host` oder `Forwarded` werden ignoriert  
- [ ] Ja — Alternative Header werden verarbeitet, überschreiben jedoch **nicht** die interne Logik  
- [ ] Ja — Alternative Header **können** verwendet werden, um den primären `Host`-Header-Wert zu überschreiben  

### Kann der `Host`-Header manipuliert werden, um systemgenerierte E-Mails (z. B. Passwort-Resets) zu manipulieren?
- [ ] Nein — E-Mail-Links verwenden eine statische, vertrauenswürdige Domain aus der Serverkonfiguration  
- [ ] Ja — Der `Host`-Header beeinflusst E-Mail-Links, aber die Validierung **kann nicht** umgangen werden  
- [ ] Ja — Der `Host`-Header **kann** manipuliert werden, um Passwort-Reset-Token an eine vom Angreifer kontrollierte Domain umzuleiten *(Kritisch)*  

### Ist die Anwendung anfällig für Web Cache Poisoning über den `Host`-Header?
- [ ] Nein — Es ist kein Caching-Mechanismus vorhanden oder der `Host`-Header ist Teil des Cache Keys  
- [ ] Ja — Ein Cache ist vorhanden, aber er validiert den `Host`-Header streng oder ignoriert ihn bei Unkeyed Inputs  
- [ ] Ja — Ein Cache existiert und **kann** durch einen injizierten `Host`-Header manipuliert werden, um bösartige Inhalte an andere Benutzer auszuliefern  

### Akzeptiert der Server beliebige `Host`-Header für das Virtual Host Routing?
- [ ] Nein — Der Server gibt bei nicht erkannten `Host`-Headern einen Fehler oder eine Standardseite zurück  
- [ ] Ja — Der Server akzeptiert jeden `Host`-Header, was für Reconnaissance oder die Enumeration interner Dienste nützlich **ist**

---

## WSTG-INPV-18 — Testing for Server-Side Template Injection

Server-Side Template Injection (SSTI) tritt auf, wenn eine Anwendung benutzerkontrollierte Eingaben in eine serverseitige Template-Engine einbettet, ohne eine ausreichende Bereinigung (Sanitization) oder Maskierung (Escaping) durchzuführen. Dies ermöglicht es einem Angreifer, bösartige Template-Direktiven zu injizieren, die dann auf dem Server ausgeführt werden, was potenziell zu einer vollständigen Systemkompromittierung durch Remote Code Execution (RCE) führen kann. SSTI manifestiert sich typischerweise in Funktionen wie der dynamischen E-Mail-Generierung, Profilseiten und Benachrichtigungssystemen, die Engines wie Jinja2, Twig oder FreeMarker verwenden. Aus der Sicht eines Angreifers wird diese Schwachstelle häufig ausgenutzt, um sensible Umgebungsvariablen auszuspähen, interne Dateien zu lesen oder Systembefehle unter Ausnutzung der nativen Objekte und Methoden der Template-Engine auszuführen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Tools:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### Verwendet die Anwendung eine serverseitige Template-Engine zur Verarbeitung von Benutzereingaben?
- [ ] Nein — Die Anwendung verwendet **keine** serverseitigen Templates für dynamische Inhalte  
- [ ] Ja — Templates werden verwendet, aber Benutzereingaben werden **nicht** in Template-Direktiven eingebettet  
- [ ] Ja — Benutzereingaben werden **ohne** ordnungsgemäße Bereinigung in Templates eingebettet  

### Werden grundlegende arithmetische Ausdrücke ausgewertet, wenn sie in Eingabefelder injiziert werden?
- [ ] Nein — Payloads wie `{{7*7}}` oder `${7*7}` werden als literale Zeichenfolgen reflektiert  
- [ ] Ja — Ausdrücke werden ausgewertet (z. B. Rückgabe von `49`), was bestätigt, dass die Template-Engine **anfällig ist**  

### Kann auf die integrierten Objekte oder Methoden der Template-Engine zugegriffen werden?
- [ ] Nein — Der Zugriff auf gefährliche Objekte, Methoden oder Konfigurationen ist **eingeschränkt** oder erfolgt in einer Sandbox  
- [ ] Ja — Auf integrierte Objekte (z. B. `self`, `config`, `__class__`) **kann** zugegriffen werden, um sensible Daten abfließen zu lassen  
- [ ] Ja — Remote Code Execution (RCE) über Systemaufrufe oder Betriebssystembibliotheken **ist möglich**  

### Gibt es eine Sandbox oder einen Allowlist-Mechanismus, der die Ausführung gefährlicher Direktiven verhindert?
- [ ] Ja — Eine strikte Allowlist oder eine sichere Sandbox ist vorhanden und ein Bypass **ist nicht möglich**  
- [ ] Ja — Eine Sandbox ist vorhanden, aber ein Bypass **ist möglich** über spezifische, engine-abhängige Payloads  
- [ ] Nein — Es wird **keine** Sandbox oder Allowlist auf die Template-Engine angewendet

---

## WSTG-INPV-19 — Prüfung auf Server-Side Request Forgery (SSRF)

Server-Side Request Forgery tritt auf, wenn ein Angreifer eine Webanwendung dazu veranlassen kann, nicht autorisierte Anfragen vom Server an interne oder externe Ressourcen zu stellen. Durch die Manipulation von Parametern, die URLs, Hostnamen oder IP-Adressen erwarten, kann ein Angreifer Kontrollen auf Netzwerkebene wie Firewalls und ACLs umgehen, um interne Netzwerksegmente zu sondieren, auf Cloud-Metadatendienste (IMDS) zuzugreifen oder mit verwundbaren internen APIs und Datenbanken zu interagieren. Diese Schwachstelle tritt typischerweise in Funktionen auf, die das Abrufen von Bildern, die PDF-Generierung, Webhooks oder Datei-Uploads via URL beinhalten. Aus der Sicht eines Angreifers ist SSRF ein mächtiges Primitiv, das verwendet wird, um in isolierte Umgebungen vorzudringen, sensible Konfigurationsdaten zu exfiltrieren oder potenziell eine Remote Code Execution durch Interaktion mit internen Diensten wie Redis oder Memcached zu erreichen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Teststatus** | Not Performed |
| **Schweregrad** | High / Critical |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Tools:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### Verarbeitet die Anwendung vom Benutzer bereitgestellte URLs oder Hostnamen?
- [ ] Nein — keine Funktionen akzeptieren externe URLs oder Hostnamen  
- [ ] Ja — Funktion existiert, aber die Eingabe **wird streng gegen eine Allow-list validiert**  
- [ ] Ja — Funktion existiert und akzeptiert vom Benutzer bereitgestellte URLs  

### Werden Validierungskontrollen oder Blacklists auf das angeforderte Ziel angewendet?
- [ ] Ja — eine strenge **Allow-list** vertrauenswürdiger Domänen/IPs wird erzwungen  
- [ ] Ja — eine **Deny-list** wird verwendet (z. B. Blockierung von `127.0.0.1`, `169.254.169.254`)  
- [ ] Nein — **keine Validierung** oder Filterung wird auf die Eingabe angewendet  

### Ist es möglich, Filter durch Kodierung, Redirects oder DNS Rebinding zu umgehen?
- [ ] Nein — Filter sind robust und verarbeiten Redirects/DNS-Auflösung sicher  
- [ ] Ja — Bypass **ist möglich** durch URL-Kodierung oder alternative IP-Formate (Hexadezimal, Oktal)  
- [ ] Ja — Bypass **ist möglich** über 3xx HTTP Redirects zu internen Zielen  
- [ ] Ja — Bypass **ist möglich** über DNS Rebinding Angriffe, um auf interne IPs aufzulösen  

### Kann die Anwendung interne Metadatendienste oder lokale Schnittstellen erreichen?
- [ ] Nein — das lokale Netzwerk und Cloud-Metadaten-Endpunkte sind **nicht erreichbar**  
- [ ] Ja — Zugriff auf Localhost (`127.0.0.1`) oder Loopback-Dienste **ist möglich**  
- [ ] Ja — Zugriff auf Cloud-Metadatendienste (z. B. AWS/GCP/Azure IMDS) **ist möglich** *(Kritisch)*  

### Was ist die Art der SSRF-Antwort?
- [ ] Blind — keine Antwort oder Metadaten werden an den Benutzer zurückgegeben  
- [ ] Partial — Antwort-Metadaten (Header, Größe, Timing) werden an den Benutzer zurückgegeben  
- [ ] Full — der vollständige Antwort-Body der internen Ressource **wird an den Benutzer reflektiert**

---

## WSTG-INPV-20 — Mass Assignment

Mass Assignment, auch bekannt als Overposting oder unsichere Deserialisierung von Parametern, tritt auf, wenn eine Webanwendung eingehende HTTP-Request-Parameter automatisch an interne Objekteigenschaften bindet, ohne eine ordnungsgemäße Filterung der Attribute vorzunehmen. Diese Sicherheitslücke ist in modernen MVC-Frameworks und RESTful APIs verbreitet, bei denen JSON-, XML- oder form-encoded Daten direkt in anwendungsseitige Datenmodelle deserialisiert werden. Angreifer nutzen dieses Verhalten aus, indem sie zusätzliche, nicht aufgeführte Parameter in eine Anfrage einschleusen – wie z. B. `isAdmin`, `role` oder `account_balance` –, um sensible Felder zu modifizieren, die von den Entwicklern nicht für die Benutzerkontrolle vorgesehen waren. Eine erfolgreiche Ausnutzung führt in der Regel zu Privilege Escalation, unbefugter Datenänderung oder der Umgehung kritischer Business-Logic-Workflows.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Mittel* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn administrative Attribute, Berechtigungsstufen oder Abrechnungsfelder geändert werden können.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### Nutzt die Anwendung automatisches Binding für eingehende Request-Parameter?
- [ ] Nein — Parameter werden manuell auf spezifische Variablen oder sichere Objekte gemappt  
- [ ] Ja — automatisches Binding wird verwendet, aber strikte **Whitelists** oder DTOs (Data Transfer Objects) werden erzwungen  
- [ ] Ja — automatisches Binding wird verwendet und sensible interne Attribute **können** modifiziert werden  

### Können administrative oder berechtigungsrelevante Attribute erfolgreich injiziert werden?
- [ ] Nein — injizierte Parameter werden ignoriert, entfernt oder führen zu einem Validierungsfehler  
- [ ] Ja — zusätzliche Parameter werden akzeptiert, beeinflussen aber **nicht** den Zustand des Backend-Objekts  
- [ ] Ja — das Injizieren von Parametern wie `is_admin`, `role` oder `permissions` führt **erfolgreich** zu einer Privilege Escalation *(Kritisch)*  

### Ist es möglich, sensible Business-Logik oder Finanzfelder via Overposting zu modifizieren?
- [ ] Nein — Felder wie `price`, `balance` oder `status` sind vor Mass Assignment geschützt  
- [ ] Ja — sensible Geschäftsattribute **können** modifiziert werden, indem sie dem JSON- oder Form-Request-Body hinzugefügt werden  

### Offenbart die Anwendung interne Objektstrukturen, die das Erraten von Parametern erleichtern?
- [ ] Nein — Antworten enthalten nur vorgesehene öffentliche Felder und die Dokumentation ist eingeschränkt  
- [ ] Ja — interne Objektfelder sind in GET-Antworten sichtbar, was das Discovery von Parametern **möglich** macht  
- [ ] Ja — die API-Dokumentation (Swagger/OpenAPI) listet explizit interne Eigenschaften auf, die zugewiesen (**assigned**) werden können

---

## WSTG-ERRH-01 — Testing for Improper Error Handling

Eine unsachgemäße Fehlerbehandlung (Improper Error Handling) tritt auf, wenn eine Webanwendung sensible technische Details — wie Stack Traces, Datenbank-Schema-Informationen oder interne Dateipfade — über ihre Fehlermeldungen preisgibt. Angreifer lösen diese Fehler aus, indem sie fehlerhafte Eingaben (Malformed Input) übermitteln, auf nicht existierende Ressourcen zugreifen oder serverseitige Ausnahmen (Exceptions) provozieren, um die zugrunde liegende Architektur der Anwendung zu erfassen und potenzielle Vektoren für weitere Angriffe (Exploitation) zu identifizieren. Dieser Informationsabfluss (Information Leakage) dient oft als Vorstufe für schwerwiegendere Angriffe, einschließlich SQL Injection oder Path Traversal, indem dem Angreifer präzise Umgebungsspezifikationen geliefert werden. Eine sichere Implementierung muss generische, benutzerfreundliche Meldungen bereitstellen, während detaillierte Diagnosedaten nur in sicheren, serverseitigen Protokollen (Logs) erfasst werden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### Nutzt die Anwendung einen generischen, globalen Error Handler für nicht abgefangene Ausnahmen?
- [ ] Ja — generische Fehlerseiten sind **aktiviert** und geben **keine** technischen Details preis  
- [ ] Ja — benutzerdefinierte Fehlerseiten werden verwendet, aber ein Informationsabfluss **ist möglich** über spezifische Header  
- [ ] Nein — Standard-Fehlerseiten des Servers (z. B. Tomcat, IIS, Nginx) sind **sichtbar**  

### Können technische Informationen durch fehlerhafte Eingaben oder Grenzfälle enumeriert werden?
- [ ] Nein — Fehler werden angemessen behandelt, mit eindeutigen Referenz-IDs für die Protokollierung  
- [ ] Ja — Stack Traces oder Datenbankabfragen **werden in den Antworten offengelegt**  
- [ ] Ja — interne Dateipfade, Umgebungsvariablen oder Server-Versionsstrings **werden offengelegt**  

### Geben API-Antworten ausführliche Fehlerobjekte oder Debug-Informationen preis?
- [ ] Nein — APIs geben standardisierte Fehlercodes und bereinigte JSON-Meldungen zurück  
- [ ] Ja — APIs geben ausführliche Debug-Objekte oder vollständige Details zu Ausnahmen im Response-Body zurück  
- [ ] Ja — Stack Traces sind in den Feldern `details` oder `exception` der JSON-Antwort enthalten  

### Verhält sich die Anwendung unterschiedlich (zeitbasiert oder inhaltsbasiert), wenn Fehler auftreten?
- [ ] Nein — Antwort-Signaturen und Zeitverhalten sind konsistent, unabhängig vom Fehlertyp  
- [ ] Ja — differenzielle Antworten **können** genutzt werden, um valide vs. invalide Zustände zu enumerieren (z. B. User Enumeration)

---

## WSTG-ERRH-02 — Prüfung auf Stack Traces

Stack Traces werden generiert, wenn eine Anwendung eine Exception nicht kontrolliert verarbeitet, was zur Offenlegung des internen Ausführungsstatus und der Aufrufhierarchie gegenüber dem Endbenutzer führt. Für einen Penetrationstester sind diese Traces von unschätzbarem Wert, da sie das Anwendungsframework, Bibliotheksversionen, interne Dateisystempfade und den Logikfluss preisgeben, was den Aufwand für die Reconnaissance erheblich reduziert. Die Ausnutzung beinhaltet das absichtliche Auslösen von Fehlern durch manipulierte Eingaben, unerwartete Datentypen oder Verletzungen von Grenzwerten, um die Anwendung in einen instabilen Zustand zu zwingen und Informationen über den zugrunde liegenden Technologiestack zu exfiltrieren. Dieser Informationsabfluss bietet oft den notwendigen Kontext, um verwundbare Drittanbieter-Komponenten zu identifizieren oder spezifische Exploits für Logikfehler zu entwickeln, die innerhalb der offengelegten Pfade im Code entdeckt wurden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel* |

> *Der Schweregrad erhöht sich auf Mittel, wenn der Stack Trace sensible architektonische Details, Datenbankabfragen oder interne Konfigurationsparameter preisgibt.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Werden bei Anwendungsfehlern vollständige Stack Traces an den Client zurückgegeben?
- [ ] Nein — Anwendung gibt generische Fehlerseiten oder benutzerdefinierte Fehlermeldungen zurück  
- [ ] Ja — Stack Traces sind **aktiviert**, aber nur für spezifische, nicht-sensible Module  
- [ ] Ja — vollständige Stack Traces sind **aktiviert** und im HTTP-Response-Body sichtbar  

### Offenbaren Fehlermeldungen sensible Umgebungsinformationen?
- [ ] Nein — Fehlermeldungen enthalten keine technischen oder umgebungsbezogenen Details  
- [ ] Ja — Meldungen offenbaren **interne Dateipfade** oder serverseitige **Verzeichnisstrukturen**  
- [ ] Ja — Meldungen offenbaren **Datenbankschemata**, **SQL-Abfragen** oder **Versionen von Drittanbieter-Bibliotheken**  

### Können Stack Traces durch Manipulation von Eingabeparametern ausgelöst werden?
- [ ] Nein — manipulierte Eingaben werden kontrolliert verarbeitet oder durch die Validierung abgelehnt  
- [ ] Ja — Traces können durch **unerwartete Datentypen** ausgelöst werden (z. B. Übergabe eines Arrays, wo ein String erwartet wird)  
- [ ] Ja — Traces werden durch **Null-Bytes**, **Sonderzeichen** oder **Grenzwerte** ausgelöst  

### Wird ein globaler Error Handler konsistent in der gesamten Anwendung angewendet?
- [ ] Ja — ein globaler Exception Handler wird **konsistent** auf alle getesteten Endpunkte angewendet  
- [ ] Ja — ein Handler ist vorhanden, wird jedoch **nicht** auf Legacy-Systeme, APIs oder neu hinzugefügte Endpunkte angewendet  
- [ ] Nein — es wurde kein globaler Error-Handling-Mechanismus **festgestellt**, was zu Standard-Serverfehlern führt

---

## WSTG-CRYP-01 — Prüfung auf schwache Transport Layer Security

Die Prüfung auf schwache Transport Layer Security umfasst die Analyse der Implementierung und Konfiguration von SSL/TLS, um die Vertraulichkeit und Integrität der Daten während der Übertragung zu gewährleisten. Angreifer zielen auf Fehlkonfigurationen wie veraltete Protokolle (SSLv2, TLS 1.0), schwache Cipher Suites (NULL, RC4 oder anonym) sowie ungültige oder schwach signierte Zertifikate ab, um Man-in-the-Middle (MitM)-Angriffe durchzuführen. Dieser Test richtet sich in der Regel gegen die Webserver- oder Load Balancer-Endpunkte der Anwendung, an denen die verschlüsselte Kommunikation aufgebaut wird. Eine erfolgreiche Ausnutzung ermöglicht es einem Angreifer, sensible Daten abzufangen, Benutzersitzungen zu übernehmen (Session Hijacking) oder Verbindungen durch Protokoll-Downgrades oder Entschlüsselung des Datenverkehrs in unsichere Zustände zu versetzen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Mittel* |

> *Der Schweregrad ist Hoch, wenn sensible Daten (PII, Anmeldedaten, Session-Tokens) übertragen werden; Mittel für allgemeinen Informationsverkehr.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Werden veraltete oder unsichere TLS/SSL-Protokolle unterstützt?
- [ ] Nein — nur TLS 1.2 und TLS 1.3 sind **aktiviert**  
- [ ] Ja — TLS 1.0 oder TLS 1.1 sind **aktiviert**  
- [ ] Ja — SSLv2 oder SSLv3 sind **aktiviert** *(Kritisch)*  

### Werden schwache oder unsichere Cipher Suites vom Server akzeptiert?
- [ ] Nein — es werden nur starke, moderne Ciphers (z. B. AES-GCM, ChaCha20) **unterstützt**  
- [ ] Ja — Ciphers mit schwacher Verschlüsselung (z. B. 3DES, RC4) oder geringer Bitlänge werden **unterstützt**  
- [ ] Ja — NULL, anonyme (ADH) oder Export-Ciphers werden **unterstützt** *(Kritisch)*  

### Ist das Server-Zertifikat gültig und vertrauenswürdig?
- [ ] Ja — das Zertifikat ist gültig, stimmt mit der Domain überein und ist von einer **vertrauenswürdigen CA** signiert  
- [ ] Nein — das Zertifikat ist **abgelaufen** oder verwendet einen **schwachen Signaturalgorithmus** (z. B. SHA-1)  
- [ ] Nein — das Zertifikat ist **selbstsigniert** oder es liegt ein Domain-Name-**Mismatch** vor  

### Unterstützt der Server Secure Renegotiation und verhindert er Downgrade-Angriffe?
- [ ] Ja — Secure Renegotiation wird **unterstützt** und TLS Fallback SCSV ist **aktiviert**  
- [ ] Nein — Secure Renegotiation wird **nicht unterstützt**, was potenziell MitM-Injektionen ermöglicht  
- [ ] Nein — Der Server ist anfällig für **POODLE**- oder **ROBOT**-Angriffe  

### Ist HTTP Strict Transport Security (HSTS) ordnungsgemäß implementiert?

| Header | Vorhanden | Fehlt |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Ja — der HSTS-Header ist vorhanden, weist eine lange `max-age` auf und `includeSubDomains` ist **aktiviert**  
- [ ] Ja — der HSTS-Header ist vorhanden, aber `includeSubDomains` fehlt oder die `max-age` ist **kurz**  
- [ ] Nein — der HSTS-Header ist **nicht vorhanden**

---

## WSTG-CRYP-02 — Prüfung auf Padding Oracle

Padding-Oracle-Schwachstellen treten auf, wenn der Entschlüsselungsprozess einer Anwendung Informationen darüber preisgibt, ob das Padding eines entschlüsselten Chiffretexts (Ciphertext) gültig oder ungültig ist. Durch die Beobachtung dieser Seitenkanal-Antworten – wie unterschiedliche HTTP-Statuscodes, spezifische Fehlermeldungen oder Variationen in der Antwortzeit – kann ein Angreifer Daten entschlüsseln, ohne den Verschlüsselungsschlüssel (Encryption Key) zu kennen, und potenziell beliebigen Klartext (Plaintext) neu verschlüsseln. Diese Schwachstelle tritt typischerweise in verschlüsselten Session-Cookies, Authentifizierungs-Token oder URL-Parametern auf, die Blockchiffren im Cipher Block Chaining (CBC) Modus mit PKCS#7-Padding verwenden. Eine Ausnutzung (Exploitation) ermöglicht den vollständigen Verlust der Vertraulichkeit und kann durch die Konstruktion gefälschter verschlüsselter Blöcke zu einer Beeinträchtigung der Integrität führen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch* |

> *Der Schweregrad ist kritisch, wenn sich die Schwachstelle in der Sitzungsverwaltung, in Identitäts-Token oder in der Verschlüsselung von PII (personenbezogenen Daten) befindet.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Wird eine Blockchiffre im CBC-Modus für sensible Daten verwendet?
- [ ] Nein — Die Anwendung verwendet authentifizierte Verschlüsselung (AES-GCM/ChaCha20-Poly1305)  
- [ ] Ja — Die Anwendung verwendet Blockchiffren, aber ein Padding ist **nicht** erforderlich (z. B. Stromchiffren oder CTR-Modus)  
- [ ] Ja — Die Anwendung verwendet den CBC-Modus und ein Padding **wird angewendet**  

### Gibt der Server die Gültigkeit des Paddings über Seitenkanäle preis?
- [ ] Nein — Der Server liefert einheitliche Fehlermeldungen und konsistente Antwortzeiten  
- [ ] Ja — Der Server gibt unterschiedliche HTTP-Statuscodes (z. B. 200 vs. 500) bei Padding-Fehlern zurück  
- [ ] Ja — Der Server gibt spezifische Fehlermeldungen zurück (z. B. "Invalid Padding", "Decryption failed")  
- [ ] Ja — Der Server weist messbare Zeitunterschiede bei Entschlüsselungsfehlern auf  

### Ist eine automatisierte Entschlüsselung des Chiffretexts möglich?
- [ ] Nein — Die Seitenkanäle sind **nicht** stabil genug für eine Ausnutzung  
- [ ] Ja — Der Chiffretext **kann** teilweise entschlüsselt werden (die letzten Blöcke)  
- [ ] Ja — Eine vollständige Klartext-Wiederherstellung (Plaintext Recovery) **ist möglich** unter Verwendung von Tools wie `PadBuster`  

### Kann der Angreifer Bit-Flipping durchführen oder gültige Chiffretexte fälschen?
- [ ] Nein — Integritätsprüfungen (HMAC) werden **vor** der Entschlüsselung verifiziert  
- [ ] Ja — Es ist keine Integritätsprüfung vorhanden, aber die Konstruktion des Payloads **ist nicht** praktikabel  
- [ ] Ja — Die Konstruktion beliebiger Chiffretexte **ist möglich**, was eine Rechteausweitung (Privilege Escalation) oder Dateninjektion erlaubt

---

## WSTG-CRYP-03 — Prüfung auf Übertragung sensibler Informationen über unverschlüsselte Kanäle

Die Übertragung sensibler Daten über unverschlüsselte Kanäle, wie z. B. reines HTTP, setzt Informationen dem Abfangen und Modifizieren durch Man-in-the-Middle (MITM)-Angriffe aus. Diese Schwachstelle tritt typischerweise auf, wenn Webanwendungen es versäumen, Transport Layer Security (TLS) an allen Endpunkten zu erzwingen, insbesondere bei Altsystemen (Legacy-Komponenten), Login-Formularen oder während des initialen Übergangs von HTTP zu HTTPS. Aus der Sicht eines Angreifers ermöglicht dies das passive Sniffing von Authentifizierungsdaten (Credentials), Session-Token und personenbezogenen Daten (PII) in kompromittierten oder nicht vertrauenswürdigen Netzwerksegmenten wie öffentlichem WLAN. Die Sicherstellung, dass alle sensiblen Daten innerhalb eines robusten TLS-Tunnels gekapselt sind, ist grundlegend für die Wahrung der Vertraulichkeit und Integrität der Benutzerinteraktionen und zur Verhinderung von Session-Hijacking.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Teststatus** | Nicht durchgeführt |
| **Risikostufe** | Hoch / Mittel |

> *Die Risikostufe wird als "Hoch" eingestuft, wenn Authentifizierungsdaten, Session-Identifikatoren oder PII im Klartext übertragen werden.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### Erlaubt die Anwendung die Kommunikation über unverschlüsseltes HTTP für sensible Endpunkte?
- [ ] Nein — der gesamte Anwendungsdatenverkehr wird strikt über HTTPS erzwungen *(Am sichersten)*  
- [ ] Ja — nicht-sensible Seiten sind über HTTP erreichbar, aber sensible Endpunkte sind es **nicht**  
- [ ] Ja — sensible Endpunkte (Login, Checkout, Profil) **sind** über unverschlüsseltes HTTP erreichbar *(Hohes Risiko)*  

### Gibt es eine globale Weiterleitung von HTTP zu HTTPS?
- [ ] Ja — die Anwendung führt für alle HTTP-Anfragen einen 301/302-Redirect auf HTTPS durch  
- [ ] Ja — die Weiterleitung wird nur für spezifische sensible Endpunkte durchgeführt  
- [ ] Nein — es wird **keine** Weiterleitung von HTTP zu HTTPS angewendet  

### Ist HTTP Strict Transport Security (HSTS) implementiert?
- [ ] Ja — HSTS ist **aktiviert** mit einem langen `max-age` und enthält die Direktiven `includeSubDomains` und `preload`  
- [ ] Ja — HSTS ist **aktiviert**, aber es fehlen die Direktiven `includeSubDomains` oder `preload`  
- [ ] Nein — HSTS ist auf dem Anwendungsserver **nicht aktiviert**  

### Sind sensible Cookies vor einer Übertragung im Klartext geschützt?

| Cookie-Name | Secure-Flag vorhanden | Secure-Flag fehlt |
|-------------|:-------------------:|:------------------:|
| `SessionID` |                     |                    |
| `AuthToken` |                     |                    |
| `CSRF-Token`|                     |                    |

### Überträgt die Anwendung sensible Daten über unverschlüsselte Kanäle an APIs von Drittanbietern?
- [ ] Nein — alle externen API-Aufrufe erfolgen über verschlüsselte (HTTPS) Tunnel  
- [ ] Ja — einige nicht-sensible Telemetriedaten werden über HTTP gesendet  
- [ ] Ja — API-Keys, PII oder Credentials **werden** über unverschlüsseltes HTTP an Drittanbieter gesendet

---

## WSTG-CRYP-04 — Testing for Weak Cryptographic Primitives

Schwache kryptografische Primitive umfassen die Verwendung veralteter oder unsicherer Algorithmen wie MD5, SHA-1, DES oder RC4, die anfällig für Kollisionsangriffe (Collision Attacks), Frequenzanalysen oder Brute Force Entschlüsselung sind. Diese Schwachstellen treten typischerweise beim Passwort-Hashing, der Verschlüsselung von ruhenden Daten (Data at Rest), der Generierung von Session-Token oder der Verifizierung digitaler Signaturen innerhalb des Anwendungs-Backends auf. Aus der Sicht eines Angreifers ermöglicht die Identifizierung dieser Primitiven die Kompromittierung der Datenintegrität und Vertraulichkeit, wie etwa das Cracken abgefangener Hashes zur Wiederherstellung von Klartext-Passwörtern oder das Fälschen von Authentifizierungs-Token. Moderne Anwendungen sollten stattdessen industriestandardisierte, kollisionsresistente und hoch-entropische Primitive wie Argon2, Bcrypt oder AES-GCM einsetzen, um einen robusten Schutz gegen Kryptanalyse zu gewährleisten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Test Status** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Werden veraltete Hashing-Algorithmen wie MD5 oder SHA-1 für die Speicherung sensibler Daten verwendet?
- [ ] Nein — Die Anwendung verwendet moderne kollisionsresistente Algorithmen wie Argon2 oder Bcrypt  
- [ ] Ja — Veraltete Algorithmen werden verwendet, aber **nur** für nicht sicherheitsrelevante Integritätsprüfungen  
- [ ] Ja — Veraltete Algorithmen **werden** für Passwörter oder sensible Token verwendet *(Hoch)*  

### Kommen derzeit schwache symmetrische Verschlüsselungsverfahren wie DES, 3DES oder RC4 zum Einsatz?
- [ ] Nein — AES (128-Bit oder höher) in einem sicheren Modus wie GCM oder CBC wird verwendet  
- [ ] Ja — Legacy-Verschlüsselungsverfahren sind für Abwärtskompatibilität **aktiviert**, aber **nicht** der Standard  
- [ ] Ja — Legacy-Verschlüsselungsverfahren sind die primäre Methode zum Schutz von Daten im Ruhezustand oder bei der Übertragung  

### Implementiert die Anwendung eigenentwickelte ("roll-your-own") oder nicht standardisierte kryptografische Logik?
- [ ] Nein — Die Anwendung hält sich strikt an standardisierte, von Experten geprüfte kryptografische Bibliotheken  
- [ ] Ja — Eigene Wrapper existieren, aber die zugrunde liegenden Primitiven **sind** Industriestandard  
- [ ] Ja — Proprietäre kryptografische Algorithmen oder benutzerdefinierte Logik **wird** auf sensible Daten angewendet *(Kritisch)*  

### Werden kryptografische Salts und Initialisierungsvektoren (IVs) gemäß Best Practices verwaltet?
- [ ] Ja — Salts sind pro Benutzer eindeutig und IVs sind für jede Operation unvorhersehbar  
- [ ] Ja — Salts werden verwendet, sind aber **nicht** über alle Datensätze hinweg eindeutig  
- [ ] Nein — Salts sind statisch, hartkodiert oder fehlen ganz, und IVs **werden** über mehrere Sitzungen hinweg wiederverwendet  

### Ist die Bit-Länge asymmetrischer Schlüssel (RSA, Diffie-Hellman) für moderne Standards ausreichend?
- [ ] Ja — RSA/DH-Schlüssel haben 2048 Bit oder mehr, oder es wird Elliptic Curve (ECC) verwendet  
- [ ] Nein — Schlüssel haben weniger als 2048 Bit, aber ein Bypass ist **nicht** trivial  
- [ ] Nein — Schlüssel haben 1024 Bit oder weniger, was sie **anfällig** für Faktorisierung oder Rechenangriffe macht

---

## WSTG-BUSL-01 — Test der Validierung von Geschäftslogik-Daten

Die Prüfung der Validierung von Geschäftslogik-Daten konzentriert sich auf die Fähigkeit der Anwendung, Daten zu identifizieren und abzulehnen, die zwar syntaktisch korrekt, aber im Kontext der Geschäftsdomäne logisch ungültig sind. Angreifer nutzen diese Schwachstellen aus, indem sie unerwartete Werte übermitteln – wie negative Mengen in einem Warenkorb, Daten in der Vergangenheit für zukünftig datierte Ereignisse oder extrem große Ganzzahlen (Integers) –, um Einschränkungen zu umgehen und den Status der Anwendung zu manipulieren. Diese Schwachstellen befinden sich häufig in mehrstufigen Workflows, Checkout-Prozessen und Kontoverwaltungsmodulen, bei denen die serverseitige Logik die kontextuelle Integrität der vom Benutzer bereitgestellten Daten nicht überprüft. Eine erfolgreiche Ausnutzung kann zu finanziellen Verlusten, Beeinträchtigungen der Integrität und unbefugtem Zugriff auf eingeschränkte Funktionen oder Daten führen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Werkzeuge:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Erzwingt die Anwendung logische Bereichsgrenzen für numerische Eingaben?
- [ ] Ja — Die Anwendung weist negative Werte, Null oder übermäßig große Werte dort korrekt ab, wo sie unangebracht sind *(Am sichersten)*  
- [ ] Ja — Die Anwendung weist einige ungültige Bereiche ab, ist aber anfällig für Integer Overflow oder das Umgehen von Grenzwerten (Boundary Bypass)  
- [ ] Nein — Die Anwendung akzeptiert jeden numerischen Wert ungeachtet des Geschäftskontexts *(Kritisch)*  

### Sind serverseitige Validierungsprüfungen über alle Schichten der Anwendung hinweg konsistent?
- [ ] Ja — Die Validierung wird konsistent sowohl auf der API-/Service- als auch auf der Datenbankebene angewendet  
- [ ] Ja — Die Validierung wird im UI/Frontend angewendet, aber eine Umgehung durch direkte API-Anfragen ist möglich  
- [ ] Nein — Die Validierung wird nicht angewendet oder ist inkonsistent, was eine logische Datenkorruption ermöglicht  

### Kann die Geschäftslogik durch Manipulation von Daten in mehrstufigen Workflows untergraben werden?
- [ ] Nein — Der Status wird sicher auf dem Server verwaltet, und Daten aus vorherigen Schritten können nicht manipuliert werden  
- [ ] Ja — Die Validierung erfolgt in frühen Schritten, aber die endgültige Übermittlung prüft die Integrität der Daten nicht erneut  
- [ ] Ja — Ein Workflow-Bypass ist möglich, indem Schritte übersprungen oder Daten in der falschen Reihenfolge übermittelt werden  

### Verarbeitet die Anwendung „Edge-Case“-Daten ordnungsgemäß, die Geschäftseinschränkungen umgehen?
- [ ] Ja — Die Einschränkungen sind robust und verarbeiten ungewöhnliche, aber syntaktisch gültige Eingaben (z. B. Schaltjahre, Zeitzonenverschiebungen)  
- [ ] Ja — Einige Sonderfälle werden behandelt, aber spezifische Kombinationen können zu unerwartetem Verhalten führen  
- [ ] Nein — Edge-Cases führen direkt zu Logikfehlern, wie z. B. kostenlosen Käufen oder unbefugten Statusänderungen

---

## WSTG-BUSL-02 — Prüfung der Fähigkeit zur Fälschung von Anfragen

Das Fälschen von Anfragen (Forging Requests) im Kontext der Business Logic beinhaltet die Manipulation von anwendungsdefinierten Parametern, um Aktionen außerhalb des beabsichtigten Workflows oder Berechtigungsniveaus durchzuführen. Angreifer zielen auf mehrstufige Prozesse ab, wie z. B. Checkout-Systeme oder die Registrierung von Konten, bei denen der Status (State) über clientseitige Parameter verwaltet wird, die der Server nicht erneut gegen eine vertrauenswürdige Backend-Quelle validiert. Durch das Ändern versteckter Felder, JSON-Werte oder REST-Parameter wie Preise, Mengen oder interne Status-Flags kann ein Angreifer Zahlungsanforderungen umgehen, Berechtigungen ausweiten (Privilege Escalation) oder unbeabsichtigte Statusänderungen auslösen. Diese Schwachstelle tritt typischerweise in zustandsbehafteten Webformularen oder APIs auf, bei denen der Server der clientseitigen Darstellung des Business-Status während einer Transaktion implizit vertraut.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch* |

> *Der Schweregrad wird als Kritisch eingestuft, wenn die gefälschte Anfrage finanzielle Verluste, unbefugte administrative Aktionen oder eine vollständige Kontoübernahme (Account Takeover) ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Validiert die Anwendung Transaktionsparameter gegen eine serverseitige "Source of Truth"?
- [ ] Ja — alle sensiblen Parameter (Preis, Rolle, ID) werden gegen die Datenbank validiert und ein Bypass ist **nicht möglich**  
- [ ] Ja — eine Validierung erfolgt, kann aber durch Parameter Pollution oder alternative Kodierung umgangen werden  
- [ ] Nein — die Anwendung vertraut clientseitigen Werten, um die Kosten oder das Ergebnis einer Transaktion zu bestimmen *(Kritisch)*  

### Können geschäftskritische Werte (z. B. Mengen, Beträge) zu unbefugten Werten modifiziert werden?
- [ ] Nein — negative Werte, Nullwerte oder übermäßig hohe Werte werden vom Server abgelehnt  
- [ ] Ja — der Server akzeptiert modifizierte Werte, jedoch nur innerhalb eines begrenzten Bereichs  
- [ ] Ja — negative Werte oder Nullwerte **können** übermittelt werden, was zu finanziellen Verlusten oder Logik-Bypasses führt  

### Werden versteckte Felder oder API-Parameter verwendet, um den Status über mehrstufige Prozesse hinweg aufrechtzuerhalten?
- [ ] Nein — der Status wird sicher über serverseitige Sessions oder signierte Token verwaltet  
- [ ] Ja — der Status wird über den Client übermittelt, aber Integritätsprüfungen (HMAC/Signaturen) sind **aktiviert** und robust  
- [ ] Ja — der Status wird über den Client übermittelt und Integritätsprüfungen sind **deaktiviert** oder können entfernt werden  

### Ist es möglich, Anfragen zu fälschen, die obligatorische Zwischenschritte in einem Workflow überspringen?
- [ ] Nein — der Server erzwingt eine strikte Abfolge von Operationen für jede Transaktion  
- [ ] Ja — Schritte **können** übersprungen werden, aber die finale Aktion erfordert weiterhin valide Daten aus vorherigen Schritten  
- [ ] Ja — die finale "Submit"- oder "Complete"-Anfrage **kann** direkt gefälscht werden, um Autorisierungs- oder Zahlungsschritte zu umgehen

---

## WSTG-BUSL-03 — Integritätsprüfungen testen

Die Prüfung der Integritätskontrollen konzentriert sich auf die Verifizierung, dass die Anwendung sicherstellt, dass Daten unverändert und vertrauenswürdig bleiben, während sie verschiedene Geschäftsprozesse durchlaufen oder zwischen dem Client und dem Server übertragen werden. Angreifer zielen auf diese Logik ab, indem sie kritische Parameter – wie Produktpreise, Mengen, Benutzerberechtigungen oder Transaktionsstatus – innerhalb von HTTP-Requests, versteckten Feldern oder Cookies manipulieren. Falls der Anwendung eine serverseitige Verifizierung oder robuste Integritätskontrollen wie Hashing oder HMACs fehlen, kann ein Angreifer diese Werte manipulieren, um Betrug zu begehen, die eigenen Berechtigungen zu eskalieren oder beabsichtigte Geschäftsbeschränkungen zu umgehen. Dieser Test ist entscheidend für jede Anwendung, die Finanztransaktionen, sensible Statusübergänge oder mehrstufige Workflows verarbeitet, bei denen der Output einer Phase als vertrauenswürdiger Input für die nächste dient.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn der Integritätsfehler Finanzdiebstahl, unbefugte Privilegieneskalation oder die Modifikation dauerhafter Datensätze ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Sind sensible Geschäftsparameter clientseitigen Manipulationen ausgesetzt?
- [ ] Nein — sensible Werte werden ausschließlich serverseitig verwaltet  
- [ ] Ja — Werte wie Preis, Menge oder Status-Flags **sind** exponiert, aber verschlüsselt  
- [ ] Ja — Werte **sind** im Klartext in versteckten Feldern, URL-Parametern oder Cookies exponiert  

### Wird ein Integritätsmechanismus (z. B. HMAC, Checksum) auf clientgesteuerte Parameter angewendet?
- [ ] Ja — eine robuste Integritätsprüfung **wird angewendet** und bei jedem Request verifiziert  
- [ ] Ja — eine Integritätsprüfung **wird angewendet**, verwendet jedoch einen schwachen/vorhersehbaren Algorithmus oder ein hartcodiertes Secret  
- [ ] Nein — es **werden keine** Integritätsprüfungen auf sensible Parameter angewendet  

### Kann die Integritätsprüfung von einem Angreifer umgangen oder neu berechnet werden?
- [ ] Nein — die Signaturprüfung wird strikt erzwungen und ein Bypass ist **nicht möglich**  
- [ ] Ja — die Signaturprüfung kann durch Weglassen des Signaturparameters umgangen werden  
- [ ] Ja — die Signatur **kann** neu berechnet werden, da der Secret Key oder die Logik exponiert/schwach ist  

### Führt die Anwendung eine Backend-Revalidierung der Daten gegen eine vertrauenswürdige Quelle durch?
- [ ] Ja — der Server revalidiert alle Werte gegen die Datenbank und ein Bypass ist **nicht möglich**  
- [ ] Ja — der Server führt eine teilweise Validierung durch, aber ein Bypass **ist möglich** durch spezifische Edge Cases  
- [ ] Nein — der Server vertraut clientseitig bereitgestellten Daten ohne Backend-Verifizierung *(Kritisch)*  

### Ist es möglich, den Status eines mehrstufigen Prozesses zu manipulieren?
- [ ] Nein — der Session-Status wird serverseitig verwaltet und kann nicht manipuliert werden  
- [ ] Ja — Statusinformationen werden über den Client übergeben und Manipulationen **sind möglich**, um Schritte zu überspringen oder Aktionen zu wiederholen

---

## WSTG-BUSL-04 — Test auf Process Timing

Process Timing-Angriffe beinhalten das Messen der Zeit, die eine Anwendung zur Verarbeitung spezifischer Anfragen benötigt, um Rückschlüsse auf sensible Informationen über interne Zustände oder die Existenz von Daten zu ziehen. Durch die Analyse von Abweichungen im Millisekundenbereich bei den Antwortzeiten — oft verursacht durch Early-Exit-Logik, Datenbankabfragen oder kryptographische Vergleiche mit nicht-konstanter Laufzeit — können Angreifer eine Seitenkanal-Analyse (Side-Channel Analysis) durchführen, um valide Benutzernamen aufzuzählen (Enumerate), Passwortfragmente zu verifizieren oder die Existenz spezifischer Datensätze zu bestätigen. Diese Schwachstellen treten typischerweise an Authentifizierungs-Endpunkten, Passwort-Reset-Formularen und ressourcenintensiven API-Filtern auf, bei denen die Backend-Logik basierend auf der Gültigkeit der Eingabe verzweigt. Aus der Sicht eines Angreifers dienen diese Zeitunterschiede als „Boolean Oracle“, das Wahrheiten über die Backend-Daten offenbart, selbst wenn die Anwendung identische, generische Fehlermeldungen an den Benutzer zurückgibt.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Test Status** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Tools:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### Weist die Anwendung unterschiedliche Antwortzeiten für valide gegenüber invaliden Ressourcenanfragen auf?
- [ ] Nein — Antwortzeiten sind unabhängig von der Gültigkeit der Eingabe identisch  
- [ ] Ja — es existieren vernachlässigbare Zeitunterschiede, die jedoch **nicht** statistisch signifikant sind  
- [ ] Ja — konsistente Zeitunterschiede sind **beobachtbar** und bieten ein zuverlässiges Oracle  

### Sind Vergleichsfunktionen mit konstanter Laufzeit (Constant-Time Comparison) oder künstliche Verzögerungen implementiert, um die Antwortzeiten zu normalisieren?
- [ ] Ja — Constant-Time-Algorithmen werden auf alle sensiblen Vergleichsoperationen **angewendet** *(Am sichersten)*  
- [ ] Ja — künstliche Verzögerungen oder Jitter sind **aktiviert**, aber die zugrunde liegende Logik bleibt ohne konstante Laufzeit  
- [ ] Nein — es werden keine Maßnahmen zur zeitlichen Absicherung (Timing Mitigations) **angewendet**, was die direkte Messung von Logikverzweigungen ermöglicht  

### Kann ein Angreifer statistische Analysen nutzen, um das Timing-Signal vom Netzwerkrauschen zu isolieren?
- [ ] Nein — Netzwerk-Jitter und Anwendungsrauschen machen eine Timing-Analyse **unmöglich**  
- [ ] Ja — mit einer ausreichenden Stichprobengröße **kann** das Timing-Signal vom Rauschen isoliert werden  
- [ ] Ja — das Timing-Delta ist groß genug, sodass eine statistische Normalisierung **nicht** erforderlich ist  

### Ist eine Account Enumeration über die Login- oder Passwort-Reset-Funktionalität möglich?
- [ ] Nein — die Existenz eines Accounts kann nicht durch Timing bestimmt werden  
- [ ] Ja — Zeitdiskrepanzen ermöglichen es einem Remote-Angreifer, valide Benutzernamen oder E-Mails **aufzuzählen (Enumerate)**  

### Lassen ressourcenintensive Operationen (z. B. Dateiverarbeitung, komplexe Suchen) die Existenz von Daten über das Timing erkennen?
- [ ] Nein — der Ressourcenverbrauch ist einheitlich, unabhängig davon, ob ein Datensatz gefunden wird  
- [ ] Ja — die Existenz sensibler Datensätze **kann** durch Messung der Dauer der Backend-Verarbeitung abgeleitet werden

---

## WSTG-BUSL-05 — Prüfung der Nutzungslimits von Funktionen

Dieser Test bewertet, ob eine Anwendung Geschäftslogik-Einschränkungen (Business Logic Constraints) für die Häufigkeit und Gesamtzahl der Ausführungen einer bestimmten Aktion oder Funktion durch einen einzelnen Benutzer, eine Session oder eine IP-Adresse erzwingt. Das Fehlen dieser Limits ermöglicht es Angreifern, hochwertige Funktionen zu missbrauchen – wie den Versand von SMS OTPs, das Auslösen von Passwort-Reset-E-Mails, das Anwenden von Rabattcodes oder die Durchführung umfangreicher Datenexporte –, was zu finanziellen Verlusten, Ressourcenerschöpfung oder Reputationsschäden führen kann. Diese Schwachstellen finden sich typischerweise in transaktionalen Endpunkten oder Kommunikationsfunktionen und werden durch automatisierte Skripte ausgenutzt, die Aktionen weit über die beabsichtigten Geschäftsschwellenwerte hinaus wiederholen. Aus der Sicht eines Angreifers ist dies ein primäres Ziel für SMS Pumping, Mail Bombing oder das Brute-Forcing von Workflows, denen explizites Rate Limiting oder zählerbasierte Drosselungen fehlen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn die Funktion Finanztransaktionen oder SMS-Kosten betrifft oder die systemweite Verfügbarkeit beeinträchtigt.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### Definiert und erzwingt die Anwendung Limits für sensible Geschäftsfunktionen?
- [ ] Ja — Limits werden strikt erzwungen und ein Bypass ist **nicht möglich**  
- [ ] Ja — Limits werden erzwungen, sind aber zu hoch, um Missbrauch zu verhindern  
- [ ] Nein — es werden keine Limits auf die Funktionsausführung angewendet  

### Werden Rate Limits und Nutzungszähler serverseitig erzwungen?
- [ ] Ja — die Durchsetzung erfolgt strikt serverseitig und **kann nicht** manipuliert werden  
- [ ] Nein — die Durchsetzung stützt sich auf clientseitige Logik (z. B. JavaScript oder versteckte Felder)  
- [ ] Nein — es findet auf keiner Ebene eine Durchsetzung statt  

### Können die Nutzungslimits durch Header- oder Session-Manipulation umgangen werden?
- [ ] Nein — ein Bypass via `X-Forwarded-For`, `Origin` oder Session-Rotation ist **nicht möglich**  
- [ ] Ja — Limits **können** durch Spoofing von IP-Headern oder das Löschen von Cookies umgangen werden  
- [ ] Ja — Limits **können** durch das Erstellen mehrerer Accounts oder Sessions umgangen werden  

### Löst das Überschreiten des Limits eine angemessene Abwehrreaktion aus?
- [ ] Ja — die Anwendung gibt `429 Too Many Requests` zurück und protokolliert das Ereignis *(Am sichersten)*  
- [ ] Ja — die Anwendung gibt einen Fehler zurück, protokolliert aber den potenziellen Missbrauch **nicht**  
- [ ] Nein — die Anwendung verarbeitet Anfragen weiter, ignoriert aber die Ausgabe  
- [ ] Nein — die Anwendung liefert keine Antwort oder stürzt unter Last ab  

### Ist die Auswirkung des Funktionsmissbrauchs für das Unternehmen erheblich?
- [ ] Nein — der Missbrauch der Funktion hat vernachlässigbare Kosten- oder Ressourceneffekte  
- [ ] Ja — Missbrauch **kann** zu geringfügigem Ressourcenverbrauch oder Belästigung der Benutzer führen  
- [ ] Ja — Missbrauch **kann** zu erheblichen finanziellen Kosten (z. B. SMS-Gebühren) oder Dienstverweigerung (Service Denial) führen *(Kritisch)*

---

## WSTG-BUSL-06 — Testing for the Circumvention of Work Flows

Die Umgehung von Workflows (Workflow Circumvention) beinhaltet die Manipulation der Logik einer Anwendung, um beabsichtigte Sequenzen oder Voraussetzungen innerhalb mehrstufiger Prozesse zu umgehen. Angreifer identifizieren sensible Endpunkte – wie Zahlungsbestätigungen, administrative Genehmigungen oder MFA-Abschlussseiten – und versuchen, direkt auf diese zuzugreifen, wobei obligatorische Zwischenschritte übersprungen werden. Diese Schwachstelle tritt auf, wenn die serverseitige Logik keine robuste State Machine implementiert, sondern stattdessen darauf vertraut, dass ein Benutzer dem UI-gesteuerten Pfad folgt. Eine erfolgreiche Ausnutzung kann zu kritischen Fehlern in der Business Logic führen, einschließlich nicht autorisierter Transaktionen, Privilege Escalation und der Umgehung von Identitätsverifizierungsmechanismen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn die Umgehung nicht autorisierte Finanztransaktionen, Privilege Escalation oder administrativen Zugriff ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Werkzeuge:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Enthält die Anwendung mehrstufige Geschäftsprozesse?
- [ ] Nein — die Anwendung besteht nur aus Aktionen mit Einzelanfragen  
- [ ] Ja — mehrstufige Prozesse (z. B. Warenkörbe, Wizard-Formulare, Registrierung) **sind** vorhanden  

### Wird die Abfolge der Schritte vom Server strikt erzwungen?
- [ ] Ja — serverseitiges State Tracking stellt sicher, dass Schritte **nicht** übersprungen werden können  
- [ ] Ja — clientseitige Kontrollen geben eine Sequenz vor, aber der Server validiert den Fortschritt **nicht**  
- [ ] Nein — die Anwendung erzwingt **keine** spezifische Reihenfolge der Operationen  

### Kann ein Angreifer die letzte Phase eines Prozesses erreichen, indem er die finale URL oder den Endpunkt direkt aufruft?
- [ ] Nein — der direkte Zugriff auf finale Schritte ist ohne vorherigen Abschluss **nicht möglich**  
- [ ] Ja — finale Schritte (z. B. `/api/v1/checkout/complete`) **können** durch direkte Anfrage erreicht werden  

### Überprüft die Anwendung, ob die erforderlichen Daten aus vorherigen Schritten vorhanden und valide sind?
- [ ] Ja — der Server validiert alle Daten der vorherigen Schritte, bevor er fortfährt  
- [ ] Nein — der Server **ist** anfällig für das „Überspringen“ von Schritten, bei denen Daten erwartet, aber nicht verifiziert werden  

### Können Sicherheitskontrollen wie MFA oder Identitätsverifizierung durch Manipulation des Workflows umgangen werden?
- [ ] Nein — kritische Kontrollen **können nicht** durch Workflow-Manipulation umgangen werden  
- [ ] Ja — eine Umgehung von MFA oder Identitätsprüfungen **ist möglich**, indem zum Schritt nach der Verifizierung gesprungen wird

---

## WSTG-BUSL-07 — Abwehrmechanismen gegen Anwendungsmissbrauch testen

Abwehrmechanismen gegen Anwendungsmissbrauch bewerten die Fähigkeit der Anwendung, anomales Benutzerverhalten zu identifizieren, zu protokollieren und darauf zu reagieren, das von der beabsichtigten Business Logic abweicht. Im Gegensatz zur traditionellen signaturbasierten Sicherheit konzentrieren sich diese Abwehrmechanismen auf die Erkennung von Mustern wie schnell aufeinanderfolgenden Anfragen, Credential Stuffing oder sequenzieller Ressourcen-Enumeration, die auf automatisierten Missbrauch hindeuten. Aus der Sicht eines Angreifers bestimmt dieser Test die Widerstandsfähigkeit der Anwendung gegen Automatisierung und „Low and Slow“-Angriffe, die darauf abzielen, Daten zu exfiltrieren oder Ressourcen zu erschöpfen, ohne Standard-Sicherheitswarnungen auszulösen. Das Versäumnis, diese Abwehrmechanismen zu implementieren, führt häufig zu massivem Data Scraping, Account Takeovers oder Denial of Service (DoS) durch legitime, aber ressourcenintensive Anwendungsfunktionen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Sind Mechanismen für Rate Limiting oder Throttling an sensiblen Endpunkten implementiert?
- [ ] Ja — striktes Rate Limiting **wird angewendet** und an allen sensiblen Endpunkten durchgesetzt *(Am sichersten)*  
- [ ] Ja — Rate Limiting **wird angewendet**, kann aber durch IP-Rotation oder Header-Manipulation umgangen werden  
- [ ] Ja — Rate Limiting **wird nur** auf Authentifizierungs-Endpunkte angewendet, wodurch andere exponiert bleiben  
- [ ] Nein — an keinerlei Anwendungsfunktionen **wird** Rate Limiting oder Throttling **angewendet**  

### Erkennt und blockiert die Anwendung automatisierte Interaktionsmuster?
- [ ] Ja — Verhaltensanalyse oder CAPTCHAs **sind aktiviert** und blockieren Bots effektiv  
- [ ] Ja — Anti-Automation **ist aktiviert**, aber eine Umgehung **ist möglich** durch Headless Browser oder Session Cycling  
- [ ] Nein — die Anwendung **kann nicht** zwischen einem menschlichen Benutzer und einem automatisierten Skript unterscheiden  

### Wird anomales Verhalten protokolliert und folgt darauf eine defensive Reaktion?
- [ ] Ja — Missbrauch löst eine automatisierte Blockierung aus und benachrichtigt das Sicherheitsteam  
- [ ] Ja — Missbrauch wird für Audits protokolliert, aber es **wird keine** automatisierte defensive Maßnahme ergriffen  
- [ ] Nein — die Anwendung protokolliert ungewöhnliche Aktivitätsmuster **nicht** und reagiert auch nicht darauf  

### Können Abwehrmechanismen durch Manipulation von clientseitigen Identifikatoren oder Headern umgangen werden?
- [ ] Nein — Abwehrmechanismen verlassen sich auf den serverseitigen Status und verifizierte Telemetrie  
- [ ] Ja — Kontrollmechanismen **können** durch das Löschen von Cookies oder das Rotieren von `User-Agent`-Strings umgangen werden  
- [ ] Ja — Kontrollmechanismen **können** durch das Injizieren von Headern wie `X-Forwarded-For` oder `X-Real-IP` umgangen werden

---

## WSTG-BUSL-08 — Test auf das Hochladen unerwarteter Dateitypen

Das Hochladen unerwarteter Dateitypen ermöglicht es Angreifern, Geschäftslogik-Einschränkungen zu umgehen, indem sie Dateien einreichen, die von der Anwendung nicht verarbeitet werden sollen, was potenziell zu Remote Code Execution, Cross-Site Scripting oder Denial of Service führen kann. Angreifer untersuchen diese Schwachstellen, indem sie Dateierweiterungen, MIME-Typen und Magic Bytes modifizieren, um die serverseitige Validierungslogik zur Annahme bösartiger Inhalte zu verleiten. Dies geschieht typischerweise bei Profilbild-Uploads, Dokumentenmanagementsystemen oder Anhängen für Support-Tickets, wenn im Back-End eine unzureichende Validierung vorliegt. Eine erfolgreiche Ausnutzung kann den Host-Server gefährden, wenn die hochgeladene Datei ausgeführt wird, oder zu clientseitigen Angriffen führen, wenn die Datei anderen Benutzern mit einem falschen Content-Type ausgeliefert wird.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Beschränkt die Anwendung Datei-Uploads auf eine bestimmte Auswahl an Erweiterungen?
- [ ] Ja — eine strikte Whitelist von Erweiterungen wird erzwungen und ein Bypass ist **nicht möglich**  
- [ ] Ja — eine Whitelist für Erweiterungen ist vorhanden, aber ein Bypass ist über doppelte Dateierweiterungen oder Null Bytes **möglich**  
- [ ] Nein — jede Dateierweiterung **kann** hochgeladen werden  

### Wird der Dateiinhalt über die Dateierweiterung hinaus validiert?
- [ ] Ja — die serverseitige Logik verifiziert Magic Bytes und führt eine tiefgehende Inhaltsprüfung durch  
- [ ] Ja — die serverseitige Logik verifiziert den MIME-Typ ausschließlich über den `Content-Type`-Header  
- [ ] Nein — nach der Prüfung der Erweiterung **wird keine** Inhaltsvalidierung angewendet  

### Kann ein Angreifer durch das Hochladen einer Web Shell oder eines Skripts Code ausführen?
- [ ] Nein — hochgeladene Dateien werden in einem nicht ausführbaren Verzeichnis oder einem Storage Bucket (S3/Azure Blob) gespeichert  
- [ ] Ja — Dateien werden in einem öffentlich zugänglichen Verzeichnis gespeichert, aber die Ausführung **ist** über die Serverkonfiguration **deaktiviert**  
- [ ] Ja — hochgeladene bösartige Skripte **können** direkt über einen vorhersehbaren URL-Pfad ausgeführt werden *(Kritisch)*  

### Sind clientseitige Angriffe wie XSS über hochgeladene Dateitypen möglich?
- [ ] Nein — Dateien werden mit `Content-Disposition: attachment` und `X-Content-Type-Options: nosniff` ausgeliefert  
- [ ] Ja — SVG- oder HTML-Dateien **können** hochgeladen und im Browser gerendert werden, was zu XSS führt  
- [ ] Ja — Bildmetadaten (EXIF) **werden verarbeitet** und ohne Sanitization in der Benutzeroberfläche angezeigt

---

## WSTG-BUSL-09 — Test des Uploads bösartiger Dateien

Die Datei-Upload-Funktionalität ermöglicht es Benutzern, Daten an den Server zu übermitteln, was einen direkten Vektor für das Einschleusen bösartiger Inhalte in die Umgebung der Anwendung darstellt. Angreifer nutzen schwache Validierungslogik aus, um Web Shells, Malware oder speziell präparierte Dateien – wie SVG oder HTML – hochzuladen, um Remote Code Execution (RCE) oder Cross-Site Scripting (XSS) zu erreichen. Diese Schwachstelle findet sich typischerweise in Funktionen wie Profileinstellungen, Dokumentenmanagementsystemen und Attachment-Handlern, bei denen der Server Dateitypen, Inhalte und Ausführungsberechtigungen nicht ordnungsgemäß überprüft. Eine erfolgreiche Ausnutzung führt häufig zur vollständigen Kompromittierung des Systems, zum Datenabfluss (Data Exfiltration) oder zur Nutzung des Servers als Pivot-Punkt für Angriffe auf das interne Netzwerk.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### Bietet die Anwendung Funktionen zum Hochladen von Dateien?
- [ ] Nein — es existiert keine Datei-Upload-Funktionalität  
- [ ] Ja — Funktionalität existiert für authentifizierte Benutzer  
- [ ] Ja — Funktionalität ist für **nicht authentifizierte** Benutzer verfügbar *(Kritisch)*  

### Wird die Validierung der Dateiendung serverseitig erzwungen?
- [ ] Ja — eine strikte Allowlist von Endungen wird **angewendet** und ein Bypass ist **nicht möglich**  
- [ ] Ja — eine Allowlist wird verwendet, aber ein Bypass **ist möglich** über Groß-/Kleinschreibung oder doppelte Endungen  
- [ ] Ja — eine Blocklist wird verwendet, welche mit alternativen Endungen (z. B. `.phtml`, `.asa`) umgangen werden **kann**  
- [ ] Nein — es wird keine Validierung der Dateiendung auf der Serverseite **angewendet**  

### Wird der Dateiinhalt über die Dateiendung oder den MIME-Typ hinaus validiert?
- [ ] Ja — die Anwendung überprüft Magic Bytes und führt eine Tiefenprüfung des Inhalts (Deep Content Inspection) durch  
- [ ] Ja — die Anwendung prüft nur den `Content-Type`-Header, welcher leicht manipuliert (**spoofed**) werden kann  
- [ ] Nein — die Anwendung verlässt sich bei der Validierung vollständig auf die Dateiendung  

### Werden hochgeladene Dateien in einem Verzeichnis mit Ausführungsberechtigungen gespeichert?
- [ ] Nein — Dateien werden auf einem dedizierten Speicherdienst (z. B. S3) oder in einem nicht ausführbaren Verzeichnis gespeichert  
- [ ] Ja — Dateien werden auf dem Webserver gespeichert, aber die Ausführung ist über die Konfiguration **deaktiviert**  
- [ ] Ja — Dateien werden in einem web-zugänglichen Verzeichnis gespeichert und die Ausführung **ist aktiviert** *(Kritisch)*  

### Können Sicherheitsfilter durch Manipulation des Dateinamens umgangen werden?
- [ ] Nein — Dateinamen werden vom Server bereinigt (sanitized) oder zufällig neu generiert  
- [ ] Ja — Filter **können** unter Verwendung von Null-Bytes (z. B. `shell.php%00.jpg`) umgangen werden  
- [ ] Ja — Path Traversal-Zeichen (z. B. `../../`) **können** verwendet werden, um Dateien in beliebige Verzeichnisse hochzuladen

---

## WSTG-BUSL-10 — Testen der Zahlungsfunktionalität

Das Testen der Zahlungsfunktionalität umfasst die Identifizierung von Business Logic Flaws, die es einem Angreifer ermöglichen, Payment Gateways zu umgehen, Bestellsummen zu manipulieren oder erfolgreiche Transaktionssignale vorzutäuschen (Spoofing). Diese Schwachstellen befinden sich typischerweise in der Kommunikation zwischen der Anwendung, dem Client-Browser und Drittanbietern von Zahlungsdiensten wie Stripe, PayPal oder internen Ledger-Systemen. Ein Angreifer könnte versuchen, Preisparameter während der Übertragung zu modifizieren, negative Mengen zu übermitteln, um die Gesamtkosten zu senken, oder erfolgreiche Transaktions-Callbacks per Replay-Angriff erneut zu senden, um Bestellungen ohne tatsächliche Zahlung auszuführen. Eine erfolgreiche Ausnutzung führt zu direktem finanziellen Verlust, Inventardifferenzen und einer potenziellen Beeinträchtigung der E-Commerce-Integrität der Anwendung.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Wird der Transaktionsbetrag vor der Verarbeitung serverseitig validiert?
- [ ] Ja — der Betrag wird strikt gegen die serverseitige Produktdatenbank validiert *(Am sichersten)*  
- [ ] Ja — der Betrag wird validiert, aber ein Logic Bypass ist über Parameter Pollution oder Währungswechsel möglich  
- [ ] Nein — der Transaktionsbetrag kann in der clientseitigen Anfrage modifiziert werden und wird vom Backend übernommen  

### Kann die Artikelmenge so manipuliert werden, dass eine Summe von Null oder ein negativer Gesamtbetrag entsteht?
- [ ] Nein — negative oder Null-Mengen werden durch die Validierungslogik der Anwendung abgelehnt  
- [ ] Ja — negative Mengen werden im Warenkorb akzeptiert, aber der Gesamtbetrag wird beim Checkout korrigiert  
- [ ] Ja — negative Mengen können verwendet werden, um den Gesamtrechnungsbetrag zu reduzieren oder eine Gutschrift zu erhalten *(Kritisch)*  

### Verarbeitet die Anwendung Payment-Gateway-Callbacks oder Webhooks sicher?
- [ ] Ja — Callbacks erfordern eine gültige kryptografische Signatur, die nicht vorgetäuscht werden kann  
- [ ] Ja — Signaturen werden geprüft, aber das Secret ist schwach oder die Verifizierung kann umgangen werden  
- [ ] Nein — die Anwendung verlässt sich auf nicht authentifizierte oder nicht verifizierte Callbacks, um den Zahlungsstatus zu bestätigen  

### Ist die Anwendung während der Checkout- oder Zahlungsphase anfällig für Race Conditions?
- [ ] Nein — Transaktionsintegrität und Datenbank-Locking-Mechanismen sind aktiviert  
- [ ] Ja — Race Conditions sind möglich, haben aber keinen Einfluss auf den Endpreis oder die Auftragsabwicklung  
- [ ] Ja — gleichzeitige Anfragen können zu mehreren Auftragsabwicklungen für eine einzige Zahlung oder zum „Double-Spending“ von Geschenkkarten führen  

### Sind Rabattcodes oder Treuepunkte anfällig für Manipulation oder Wiederverwendung?
- [ ] Nein — Codes werden für die einmalige Verwendung validiert und Logikbeschränkungen werden angewendet  
- [ ] Ja — Codes können wiederverwendet oder auf nicht berechtigte Artikel angewendet werden, aber die Auswirkungen sind begrenzt  
- [ ] Ja — Angreifer können mehrere widersprüchliche Codes anwenden oder Ablaufdaten und Nutzungslimits umgehen

---

## WSTG-CLNT-01 — Testen auf DOM-basiertes Cross Site Scripting

DOM-basiertes Cross-Site Scripting (DOM XSS) tritt auf, wenn eine Anwendung clientseitiges JavaScript enthält, das Daten aus einer nicht vertrauenswürdigen Quelle auf unsichere Weise verarbeitet und diese Daten meist über einen gefährlichen Sink (Senke) zurück in das Document Object Model (DOM) schreibt. Im Gegensatz zu reflektiertem oder gespeichertem XSS existiert die Sicherheitslücke vollständig im clientseitigen Code; der Payload wird oft gar nicht an den Server gesendet, da er von Sinks wie `innerHTML`, `eval()` oder `document.write()` verarbeitet wird. Angreifer nutzen dies aus, indem sie URLs mit bösartigen Fragmenten oder Parametern präparieren, die beim Laden im Browser des Opfers beliebiges JavaScript im Kontext der Benutzersitzung ausführen. Dies kann zur Exfiltration von Session-Token, zum Diebstahl sensibler Daten und zu unbefugten Aktionen im Namen des authentifizierten Benutzers führen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Werden nicht vertrauenswürdige Quellen verwendet, um benutzergesteuerte Daten in JavaScript zu erfassen?
- [ ] Nein — JavaScript verwendet **kein** `location.hash`, `location.search` oder `document.referrer`  
- [ ] Ja — nicht vertrauenswürdige Quellen werden verwendet, aber die Daten werden **nicht** an Execution Sinks übergeben  
- [ ] Ja — nicht vertrauenswürdige Quellen werden verwendet und die Daten fließen direkt in die clientseitige Logik  

### Werden benutzergesteuerte Daten an gefährliche DOM-Sinks übergeben?
- [ ] Nein — Sinks wie `innerHTML`, `outerHTML`, `document.write()` oder `eval()` werden **nicht** verwendet  
- [ ] Ja — gefährliche Sinks sind vorhanden, aber die Daten werden mit einer sicheren Bibliothek wie DOMPurify strikt **bereinigt** (Sanitization)  
- [ ] Ja — gefährliche Sinks sind vorhanden und die Daten werden mit **unzureichender** oder benutzerdefinierter Regex-basierter Filterung verarbeitet  
- [ ] Ja — gefährliche Sinks sind vorhanden und die Daten werden **ohne** jegliche Bereinigung oder Kodierung übergeben *(Kritisch)*  

### Kann der Execution Sink über einen URL-basierten Payload erreicht werden?
- [ ] Nein — der Source-to-Sink-Datenfluss **kann nicht** über externe Eingaben ausgelöst werden  
- [ ] Ja — der Datenfluss ist **möglich**, erfordert jedoch eine komplexe Benutzerinteraktion  
- [ ] Ja — der Datenfluss ist **möglich** und kann über ein einfaches URL-Fragment oder einen Query-Parameter ausgelöst werden  

### Neutralisieren moderne Sicherheits-Header oder browserbasierte Abwehrmaßnahmen das Risiko?
- [ ] Ja — eine strikte `Content-Security-Policy` (CSP) mit `script-src` und `trusted-types` ist **aktiviert** und verhindert die Ausführung  
- [ ] Ja — eine CSP ist vorhanden, aber `unsafe-inline` oder schwache Hashes machen einen Bypass **möglich**  
- [ ] Nein — es ist keine CSP vorhanden, um die DOM-basierte Ausführung zu unterbinden  

### Verwendet die Anwendung clientseitige Frameworks mit integrierten Schutzmechanismen?
- [ ] Ja — das Framework (z. B. Angular, React) kodiert Daten automatisch und ein Bypass ist **nicht möglich**  
- [ ] Ja — ein Framework wird verwendet, aber „Escape Hatches“ wie `dangerouslySetInnerHTML` sind **aktiviert**  
- [ ] Nein — die Anwendung verwendet reines JavaScript (Vanilla JS) oder veraltete Bibliotheken (z. B. alte jQuery-Versionen) ohne automatische Maskierung (Auto-Escaping)

---

## WSTG-CLNT-02 — Prüfung auf JavaScript-Ausführung

Die Prüfung auf JavaScript-Ausführung konzentriert sich auf die Identifizierung von Instanzen, in denen eine Anwendung vom Benutzer bereitgestellte Daten unsachgemäß verarbeitet, was zur Ausführung nicht autorisierter Skripte im Browserkontext des Opfers führt. Diese Schwachstelle resultiert in der Regel in Cross-Site Scripting (XSS), welches es Angreifern ermöglicht, Session-Cookies zu exfiltrieren, das Document Object Model (DOM) zu manipulieren oder Aktionen im Namen des Benutzers durchzuführen. Penetration Tester untersuchen Sinks wie `innerHTML`, `document.write()` und `eval()`, in denen unbereinigte Daten aus Quellen (Sources) wie URL-Fragmenten, `localStorage` oder `window.name` verarbeitet werden könnten. Eine erfolgreiche Ausnutzung beeinträchtigt die Integrität und Vertraulichkeit der Benutzersitzung und kann als Ausgangspunkt (Pivot) für komplexere clientseitige Angriffe dienen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Tools:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### Verarbeitet die Anwendung vom Benutzer bereitgestellte Daten in gefährlichen JavaScript-Sinks?
- [ ] Nein — alle Daten werden bereinigt (sanitized) oder in sicheren Sinks wie `textContent` verwendet  
- [ ] Ja — Daten werden in gefährlichen Sinks verarbeitet, aber eine Bereinigung/Kodierung **wird angewendet**  
- [ ] Ja — Daten werden in gefährlichen Sinks verarbeitet und eine Bereinigung **wird nicht angewendet**  

### Ist eine Content Security Policy (CSP) vorhanden, um die Skriptausführung zu unterbinden?
- [ ] Ja — eine restriktive CSP ist **aktiviert** und ein Bypass **ist nicht möglich**  
- [ ] Ja — eine CSP ist **aktiviert**, aber ein Bypass **ist möglich** über `unsafe-inline` oder unsichere CDNs  
- [ ] Nein — es ist keine CSP **aktiviert**  

### Kann JavaScript über URL-Parameter oder Fragmente ausgeführt werden (DOM-basiertes XSS)?
- [ ] Nein — die Eingabe wird korrekt kodiert und eine Ausführung **ist nicht möglich**  
- [ ] Ja — die Eingabe wird im DOM reflektiert, aber die Ausführung **wird verhindert** durch Schutzmechanismen moderner Frameworks  
- [ ] Ja — die Eingabe wird direkt im Browser ausgeführt und eine Ausnutzung (Exploitation) **ist möglich**  

### Sind Event-Handler oder URI-Schemata anfällig für Script Injection?
- [ ] Nein — die Eingabe wird bereinigt, um Event-Attribute und `javascript:`-Schemata zu entfernen  
- [ ] Ja — Event-Handler **können** injiziert werden, aber Filter begrenzen die Komplexität des Payloads  
- [ ] Ja — die Ausführung von beliebigem JavaScript über `onerror`-, `onload`- oder `href`-Attribute **ist möglich**

---

## WSTG-CLNT-03 — HTML Injection

HTML-Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Eingaben nicht ordnungsgemäß bereinigt, bevor diese im Browser gerendert werden. Dies ermöglicht es einem Angreifer, beliebige HTML-Tags in das Document Object Model (DOM) der Webseite einzuschleusen. Obwohl sie dem Cross-Site Scripting (XSS) ähnelt, besteht das Hauptziel der HTML-Injection darin, die visuelle Darstellung der Seite zu manipulieren oder Phishing-Angriffe durch das Einschleusen bösartiger Formulare und täuschender Inhalte zu erleichtern. Angreifer zielen auf Parameter ab, die in der Benutzeroberfläche (UI) reflektiert werden, wie Suchbegriffe, Benutzerprofilfelder oder Fehlermeldungen, um Opfer zur Preisgabe sensibler Anmeldedaten (Credentials) oder zur Interaktion mit nicht autorisierten Drittanbieter-Links zu verleiten. Diese Schwachstelle stellt ein erhebliches Risiko für die Integrität der Benutzeroberfläche der Anwendung dar und kann ein Vorläufer für komplexere Social Engineering- oder sitzungsbezogene Angriffe sein.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Werden vom Benutzer kontrollierte Eingaben ohne ordnungsgemäße HTML-Kodierung im DOM reflektiert?
- [ ] Nein — alle reflektierten Eingaben werden vor dem Rendern strikt HTML-kodiert  
- [ ] Ja — einige Zeichen **werden** kodiert, aber Bypasses für bestimmte Tags **sind möglich**  
- [ ] Ja — die Eingabe wird roh reflektiert, und HTML-Injection **ist möglich**  

### Können strukturelle HTML-Elemente injiziert werden, um das Seitenlayout zu verändern?
- [ ] Nein — die Anwendung filtert oder entfernt strukturelle Tags wie `<div>`, `<table>` oder `<iframe>`  
- [ ] Ja — strukturelle Elemente **können** injiziert werden, haben aber **keinen** signifikanten Einfluss auf die UI  
- [ ] Ja — ein signifikantes UI-Defacement **ist möglich** über injizierte Tags  

### Ist es möglich, funktionale Phishing-Elemente wie Formulare oder bösartige Links zu injizieren?
- [ ] Nein — `<form>`, `<input>` und `<a>` Tags werden effektiv blockiert oder bereinigt  
- [ ] Ja — bösartige Links **können** injiziert werden, um Benutzer auf externe Seiten umzuleiten  
- [ ] Ja — funktionale `<form>` Elemente **können** injiziert werden, um Anmeldedaten abzugreifen *(Mittel)*  

### Mildern Sicherheits-Header oder Content Security Policies (CSP) die Auswirkungen der Injection ab?
- [ ] Ja — eine restriktive CSP ist implementiert und `form-action` oder `frame-src` **wird angewendet**  
- [ ] Nein — Sicherheits-Header sind vorhanden, aber die CSP **ist deaktiviert** oder enthält unsafe-inline/Wildcards  
- [ ] Nein — keine CSP oder relevanten Sicherheits-Header **sind aktiviert**

---

## WSTG-CLNT-04 — Prüfung auf Client-seitige URL-Weiterleitung

Client-seitige URL-Weiterleitung (Client-Side URL Redirect) tritt auf, wenn eine Webanwendung benutzergesteuerte Eingaben akzeptiert – in der Regel über URL-Parameter oder Hash-Fragmente – und diese innerhalb eines client-seitigen Redirection Sinks wie `window.location` oder `document.location.href` verwendet. Diese Schwachstelle wird primär von Angreifern ausgenutzt, um hochentwickelte Phishing-Kampagnen durchzuführen, da das Opfer zunächst der legitimen Domain vertraut, bevor es unbemerkt auf eine bösartige Seite weitergeleitet wird. Über Phishing hinaus können client-seitige Weiterleitungen oft verkettet werden, um sensible Daten wie OAuth Access Tokens oder Session-Identifikatoren zu exfiltrieren, falls die Weiterleitung erfolgt, während diese Werte in der URL oder im `Referer`-Header vorhanden sind. In modernen Single Page Applications (SPAs) tritt diese Schwachstelle häufig in der Routing-Logik, in „return to“-Parametern nach der Authentifizierung oder in Funktionen zum Sprachwechsel auf.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel* |

> *Der Schweregrad erhöht sich auf „Hoch“, wenn die Weiterleitung zur Exfiltration von OAuth-Token, Session-IDs oder zur Umgehung von Sicherheitskontrollen in einem Authentifizierungsfluss genutzt werden kann.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Verwendet die Anwendung client-seitige Skripte, um Weiterleitungen basierend auf Benutzereingaben zu verarbeiten?
- [ ] Nein — die Weiterleitung erfolgt ausschließlich serverseitig über `3xx` HTTP-Statuscodes  
- [ ] Ja — die Anwendung verwendet JavaScript-Sinks wie `window.location`, um basierend auf URL-Parametern zu navigieren  
- [ ] Ja — die Anwendung verwendet einen Router eines client-seitigen Frameworks (z. B. `React Router`, `Vue Router`), der vom Benutzer bereitgestellte Pfade verarbeitet  

### Sind Eingabevalidierungen oder Whitelisting-Kontrollen für die Ziel-URL implementiert?
- [ ] Ja — eine strikte **Whitelist** erlaubter Domains/Pfade wird erzwungen und ein Bypass ist **nicht möglich**  
- [ ] Ja — eine Validierung ist vorhanden, basiert jedoch auf schwachen regulären Ausdrücken (Regex) oder Blacklists, und ein Bypass ist **möglich**  
- [ ] Nein — die Anwendung akzeptiert beliebige Zeichenfolgen als Weiterleitungsziel  

### Kann die Weiterleitungslogik mit gängigen Obfuskationstechniken umgangen werden?
- [ ] Nein — die Kontrollen verarbeiten kodierte Zeichen, protokollrelative URLs (`//`) und Backslashes korrekt  
- [ ] Ja — ein Bypass ist mittels URL-Encoding oder Double-Encoding **möglich**  
- [ ] Ja — ein Bypass ist mittels protokollrelativer URLs oder des `@`-Zeichens (z. B. `https://expected.com@attacker.com`) **möglich**  
- [ ] Ja — ein Bypass ist durch spezifische Browser-Eigenheiten (Quirks) oder Null-Byte-Injection **möglich**  

### Werden durch den Weiterleitungsprozess sensible Daten an die Zielseite exponiert?
- [ ] Nein — es befinden sich keine sensiblen Daten in der URL und es werden keine sensiblen Daten während der Weiterleitung über Header übertragen  
- [ ] Ja — sensible Daten (z. B. Session-Token, API-Keys) werden über den `Referer`-Header an die externe Seite **geleakt**  
- [ ] Ja — sensible Daten im URL-Fragment (`#`) sind für die Skripte der Zielseite **zugänglich**  

### Ist es möglich, JavaScript über den Redirection Sink auszuführen (DOM-based XSS)?
- [ ] Nein — der Sink erlaubt nur die Navigation zu `http`- oder `https`-Protokollen  
- [ ] Ja — der Redirection Sink erlaubt `javascript:`- oder `data:`-URIs, wodurch DOM-based XSS **möglich** ist

---

## WSTG-CLNT-05 — Prüfung auf CSS Injection

CSS Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Eingaben erlaubt, die Cascading Style Sheets (CSS) einer Webseite zu beeinflussen, ohne eine ordnungsgemäße Bereinigung (Sanitization) oder Maskierung (Escaping) durchzuführen. Während dies oft als kosmetisches Problem wahrgenommen wird, können Angreifer CSS Injection nutzen, um sensible Daten wie CSRF-Token oder Session IDs zu exfiltrieren, indem sie Attribut-Selektoren (Attribute Selectors) und `background-image`-Eigenschaften verwenden, um externe Anfragen auszulösen. Diese Schwachstelle tritt typischerweise an Endpunkten auf, an denen Benutzereinstellungen (z. B. benutzerdefinierte Themes, Schriftfarben) innerhalb von `<style>`-Blöcken oder Inline-`style`-Attributen reflektiert werden. Aus der Sicht eines Angreifers kann eine erfolgreiche Ausnutzung zu UI Redressing, Phishing durch unbefugte Layoutänderungen oder heimlicher Datenexfiltration in Umgebungen führen, in denen strenge Content Security Policies (CSP) ansonsten JavaScript-basierte Angriffe blockieren könnten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als Hoch eingestuft, wenn der Injektionspunkt die Exfiltration sensibler Attribute wie CSRF-Token ermöglicht oder wenn er für UI Redressing in sensiblen Workflows verwendet werden kann.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Reflektiert die Anwendung benutzergesteuerte Eingaben innerhalb von CSS-Kontexten?
- [ ] Nein — Eingaben werden **nicht** in Style-Tags, Attributen oder CSS-Dateien reflektiert  
- [ ] Ja — Eingaben werden reflektiert, sind aber strikt auf sichere alphanumerische Werte beschränkt  
- [ ] Ja — Eingaben werden innerhalb eines `style`-Attributs oder eines `<style>`-Blocks reflektiert  

### Werden CSS-spezifische Metazeichen und Funktionen ordnungsgemäß bereinigt?
- [ ] Ja — Zeichen wie `{`, `}`, `:` und Funktionen wie `url()` werden **ordnungsgemäß maskiert**  
- [ ] Ja — Bereinigung ist vorhanden, aber ein Bypass **ist möglich** über Kodierung oder CSS-Kommentare  
- [ ] Nein — es wird keine Bereinigung auf CSS-Metazeichen angewendet  

### Ist eine Datenexfiltration mittels CSS-Selektoren möglich?
- [ ] Nein — Selektoren **können keine** externen Anfragen auslösen oder Daten leaken  
- [ ] Ja — eine teilweise Datenexfiltration **ist möglich** über Attribut-Selektoren und `background-image`  
- [ ] Ja — die vollständige Exfiltration sensibler Token **ist möglich** unter Verwendung automatisierter Brute-Force-Techniken  

### Mildert eine Content Security Policy (CSP) das Risiko einer CSS Injection?
- [ ] Ja — `style-src` ist auf 'self' beschränkt und `img-src` oder `connect-src` **ist eingeschränkt**  
- [ ] Ja — eine CSP ist vorhanden, verwendet jedoch 'unsafe-inline' oder erlaubt Platzhalter für externe Domains  
- [ ] Nein — es ist keine CSP implementiert, um das Laden externer Ressourcen über CSS zu verhindern  

### Kann die Schwachstelle für UI Redressing oder Phishing genutzt werden?
- [ ] Nein — der Injektionsbereich ist zu begrenzt, um das Layout signifikant zu verändern  
- [ ] Ja — eine UI-Modifikation **ist möglich**, was das Überlagern mit bösartigen Elementen erlaubt  
- [ ] Ja — die vollständige Kontrolle über das Seitenlayout **ist möglich**, was hochgradig überzeugende Phishing-Angriffe erleichtert

---

## WSTG-CLNT-06 — Testing for Client-Side Resource Manipulation

Clientseitige Ressourcenmanipulation (Client-Side Resource Manipulation) tritt auf, wenn eine Anwendung zulässt, dass benutzergesteuerte Eingaben die Quelle oder den Pfad von Ressourcen beeinflussen, die vom Browser geladen werden, wie z. B. Skripte, Stylesheets oder Iframes. Durch die Manipulation von URI-Fragmenten, Query-Parametern oder anderen DOM-Quellen kann ein Angreifer die Anwendung dazu zwingen, bösartige externe Inhalte zu laden oder nicht autorisierten Code auszuführen. Diese Schwachstelle findet sich typischerweise in der JavaScript-Logik, die `src`- oder `href`-Attribute von HTML-Elementen dynamisch füllt, ohne eine ausreichende Validierung gegen eine vertrauenswürdige Allow-List durchzuführen. Aus Sicht eines Angreifers wird dies ausgenutzt, indem eine URL erstellt wird, die das Laden von Ressourcen auf einen vom Angreifer kontrollierten Server umleitet, was potenziell zu DOM-basiertem Cross-Site Scripting (XSS), der Exfiltration sensibler Daten oder UI-Redressing führen kann.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### Nutzt die Anwendung clientseitige Sinks, um Ressourcen dynamisch zu laden oder einzubetten?
- [ ] Nein — Ressourcen werden ausschließlich über statisches HTML oder serverseitige Logik geladen  
- [ ] Ja — clientseitiges JavaScript füllt Ressourcen-Attribute wie `src`, `href` oder `data` dynamisch  

### Sind Validierungskontrollen oder Allow-Lists für Ressourcen-URI-Quellen implementiert?
- [ ] Ja — strenge Allow-Lists und Origin-Validierungen **werden auf alle** dynamischen Ressourcen angewendet  
- [ ] Ja — eine Validierung ist vorhanden, stützt sich jedoch auf schwache Blacklists oder leicht zu umgehende Regex  
- [ ] Nein — es werden keine Validierungskontrollen auf benutzergesteuerte Ressourcenpfade angewendet  

### Ist es möglich, die URI-Validierung durch alternative Protokolle oder Kodierungen zu umgehen?
- [ ] Nein — eine Umgehung der Ressourcenfilter ist **nicht möglich**  
- [ ] Ja — eine Umgehung **ist möglich** durch die Verwendung verschiedener Protokolle wie `data:`, `vbscript:` oder `javascript:`  
- [ ] Ja — eine Umgehung **ist möglich** durch die Verwendung von kodierten Zeichen oder Null-Bytes, um einfache String-Abgleiche zu umgehen  

### Kann ein Angreifer das Laden von Ressourcen auf eine externe, nicht vertrauenswürdige Domain umleiten?
- [ ] Nein — Ressourcenpfade sind auf denselben Ursprung (Same-Origin) oder ein festes Unterverzeichnis beschränkt  
- [ ] Ja — die Anwendung **kann** gezwungen werden, Skripte oder Styles von einer beliebigen externen Domain zu laden  

### Was ist die maximale Auswirkung der Ressourcenmanipulation?
- [ ] Niedrig — die Manipulation betrifft nur nicht ausführbare Elemente wie Bilder oder unkritische Texte  
- [ ] Mittel — die Manipulation ermöglicht CSS Injection oder erhebliches UI-Redressing  
- [ ] Hoch — die Manipulation führt zu DOM-basiertem XSS oder der Ausführung von beliebigem JavaScript *(Kritisch)*

---

## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) ist ein browserbasierter Mechanismus, der es einem Server ermöglicht, den Zugriff auf seine Ressourcen von verschiedenen Ursprüngen (Origins) explizit zu erlauben, wodurch die Same-Origin Policy (SOP) effektiv gelockert wird. Fehlkonfigurationen treten auf, wenn eine Anwendung willkürlichen Ursprüngen übermäßig vertraut, häufig durch Reflektieren des `Origin`-Headers im `Access-Control-Allow-Origin`-Antwort-Header oder durch die Verwendung schlecht implementierter Regex-Whitelists. Aus der Sicht eines Angreifers kann eine permissive CORS-Policy ausgenutzt werden, indem ein bösartiges Skript auf einer kontrollierten Domain gehostet wird, das authentifizierte Anfragen an die verwundbare Anwendung stellt. Dies erlaubt es dem Angreifer, sensible Daten wie CSRF-Token oder PII (Personally Identifiable Information) direkt aus dem Antwort-Body zu exfiltrieren. Diese Schwachstelle ist besonders kritisch bei API-Endpunkten, die authentifizierte Sitzungen verarbeiten und nicht-öffentliche Informationen zurückgeben.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Tools:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Implementiert die Anwendung CORS-Header auf authentifizierten Endpunkten?
- [ ] Nein — CORS ist **nicht aktiviert**; der Browser verwendet standardmäßig die strikte Same-Origin Policy  
- [ ] Ja — CORS-Header sind vorhanden und auf eine strikte **Whitelist** beschränkt  
- [ ] Ja — CORS-Header sind vorhanden, aber **permissiv** konfiguriert  

### Wie verarbeitet der Server den `Access-Control-Allow-Origin` (ACAO) Header?
- [ ] ACAO ist auf eine spezifische, **vertrauenswürdige** statische Domain festgelegt  
- [ ] ACAO ist auf `*` (Wildcard) gesetzt und Credentials **können nicht** gesendet werden  
- [ ] ACAO ist auf `*` (Wildcard) gesetzt und Credentials **können** gesendet werden *(Hinweis: Die meisten Browser blockieren dies)*  
- [ ] ACAO **reflektiert dynamisch** den Wert des `Origin`-Headers aus der Anfrage  

### Ist der `Access-Control-Allow-Credentials` (ACAC) Header auf `true` gesetzt?
- [ ] Nein — ACAC ist **nicht vorhanden** oder auf `false` gesetzt  
- [ ] Ja — ACAC ist auf `true` gesetzt, aber ACAO ist auf vertrauenswürdige Ursprünge **beschränkt**  
- [ ] Ja — ACAC ist auf `true` gesetzt und ACAO **reflektiert** beliebige Ursprünge *(Kritisch)*  

### Kann die Origin-Whitelist durch Subdomain- oder Null-Origin-Manipulation umgangen werden?
- [ ] Nein — die Whitelist wird strikt erzwungen und **kann nicht** umgangen werden  
- [ ] Ja — die Whitelist akzeptiert **jede** Subdomain des Ziels (z. B. `attacker.target.com`)  
- [ ] Ja — die Whitelist akzeptiert die `null` Origin über sandboxed Iframes  
- [ ] Ja — die Whitelist ist anfällig für Regex-Bypasses (z. B. `target.com.attacker.com` oder `target.com-safe.com`)  

### Ermöglicht die CORS-Konfiguration die Exfiltration sensibler Daten?
- [ ] Nein — die exponierten Endpunkte enthalten **keine** sensiblen oder sitzungsspezifischen Daten  
- [ ] Ja — authentifizierte Endpunkte geben PII, Credentials oder CSRF-Token zurück, die von einem nicht autorisierten Ursprung gelesen werden **können**

---

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

## WSTG-CLNT-09 — Prüfen auf Clickjacking

Clickjacking, auch bekannt als UI Redressing, tritt auf, wenn ein Angreifer transparente oder opake Ebenen verwendet, um einen Benutzer dazu zu verleiten, auf eine Schaltfläche oder einen Link auf einer anderen Seite zu klicken, während dieser beabsichtigte, auf die oberste Ebene der Seite zu klicken. Diese Schwachstelle beeinträchtigt die Integrität von Benutzeraktionen und kann zu unbefugten Datenänderungen, Kontoänderungen oder Finanztransfers führen, die im Namen des Opfers durchgeführt werden. Angreifer nutzen dies in der Regel aus, indem sie die Ziel-Website in einem `<iframe>` auf einer kontrollierten bösartigen Website einbetten und diese mit täuschenden Inhalten oder verlockenden Ködern überlagern. Dies tritt am häufigsten auf Seiten auf, die sensible zustandsändernde Aktionen durchführen, wie z. B. „Konto löschen“-Schaltflächen, Formulare zum Zurücksetzen von Passworten oder administrative Konfigurationspanels.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn sensible Aktionen (z. B. Geldtransfers, Kontolöschung oder Privilegieneskalation) via Clickjacking ausgelöst werden können.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Werkzeuge:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Implementiert die Anwendung serverseitige Header, um unbefugtes Framing zu verhindern?
- [ ] Ja — `X-Frame-Options` oder `Content-Security-Policy` **werden angewendet** und verhindern Framing *(Am sichersten)*  
- [ ] Ja — Header sind vorhanden, aber **falsch konfiguriert** (z. B. `X-Frame-Options: ALLOW-FROM`, was an mangelnder breiter Browserunterstützung leidet)  
- [ ] Nein — es **werden keine** Header zum Schutz vor Framing angewendet  

### Ist die `Content-Security-Policy` (CSP) `frame-ancestors`-Direktive implementiert?
- [ ] Ja — `frame-ancestors 'none'` oder `'self'` **wird angewendet**  
- [ ] Ja — `frame-ancestors` ist vorhanden, **erlaubt** jedoch nicht vertrauenswürdige oder zu weit gefasste Ursprünge (Origins)  
- [ ] Nein — die Direktive **fehlt** oder CSP ist nicht implementiert  

### Ist der `X-Frame-Options` (XFO) Header für die Unterstützung älterer Browser korrekt konfiguriert?
- [ ] Ja — `DENY` oder `SAMEORIGIN` **wird angewendet**  
- [ ] Ja — XFO ist vorhanden, wird aber von modernen Browsern **ignoriert**, da eine CSP `frame-ancestors`-Direktive existiert  
- [ ] Nein — der XFO-Header **fehlt** oder ist auf einen unsicheren Wert gesetzt  

### Können die Framing-Schutzmaßnahmen mit bekannten Techniken umgangen werden?
- [ ] Nein — ein Bypass ist mit Standardtechniken (Double-Framing etc.) **nicht möglich**  
- [ ] Ja — der Schutz **kann** aufgrund schwacher Regex oder Logik in der `frame-ancestors`-Whitelist umgangen werden  
- [ ] Ja — der Schutz **kann** umgangen werden, da die Anwendung ausschließlich auf JavaScript-basiertem Frame-Busting basiert  

### Ist eine sensible Aktion über einen Clickjacking Proof-of-Concept ausnutzbar?
- [ ] Nein — Framing wird blockiert oder es sind keine sensiblen Aktionen erreichbar  
- [ ] Ja — UI Redressing **ist möglich**, erfordert jedoch komplexe Benutzerinteraktion  
- [ ] Ja — UI Redressing **ist möglich** und erlaubt sofortige zustandsändernde Aktionen *(Kritisch)*

---

## WSTG-CLNT-10 — Testing WebSockets

WebSocket-Tests konzentrieren sich auf den Vollduplex-Kommunikationskanal (Full-Duplex), der zwischen Client und Server aufgebaut wird und herkömmliche HTTP-Request-Response-Muster umgeht. Pentester bewerten die Sicherheit des Handshake-Prozesses, das Vorhandensein von Schutzmechanismen gegen Cross-Site WebSocket Hijacking (CSWSH) und die Strenge der Eingabevalidierung für Message Payloads. Die Ausnutzung umfasst in der Regel das Hijacking einer aktiven Sitzung, um Daten in Echtzeit zu exfiltrieren, oder das Injizieren bösartiger Payloads, die serverseitige Schwachstellen wie Command Injection oder SQLi über den persistenten Socket auslösen. Da viele herkömmliche Web Application Firewalls (WAFs) den WebSocket-Verkehr nicht effektiv prüfen, bietet dieses Protokoll oft einen unauffälligen Vektor zur Umgehung von Perimeterschutzmaßnahmen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Tools:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### Wird die WebSocket-Verbindung über einen sicheren Kanal hergestellt?
- [ ] Ja — `wss://` (WebSocket Secure) wird für alle Verbindungen erzwungen  
- [ ] Nein — `ws://` wird verwendet, was Person-in-the-middle (PITM) Lauschangriffe ermöglicht  

### Ist die Anwendung gegen Cross-Site WebSocket Hijacking (CSWSH) geschützt?
- [ ] Nein — der Server validiert den `Origin`-Header während des Handshakes strikt  
- [ ] Ja — die Validierung des `Origin`-Headers ist schwach oder **kann** über Null- oder gefälschte Origins umgangen werden  
- [ ] Ja — es wird keine `Origin`-Header-Validierung durchgeführt, was Cross-Site-Angreifern ermöglicht, eine Verbindung zu initiieren *(Kritisch)*  

### Werden Eingabevalidierung und Bereinigung (Sanitization) auf WebSocket Message Payloads angewendet?
- [ ] Ja — alle eingehenden Nachrichten werden bereinigt und gegen ein Schema validiert  
- [ ] Ja — es existiert eine gewisse Validierung, aber eine Umgehung **ist möglich** durch kodierte oder nicht standardisierte Payloads  
- [ ] Nein — Nachrichten werden direkt verarbeitet, was Injection-Angriffe (XSS, SQLi, RCE) oder Logikmanipulation ermöglicht  

### Werden Authentifizierungs- und Autorisierungsprüfungen bei jeder WebSocket-Nachricht durchgeführt?
- [ ] Ja — die Sitzung wird für jede Nachricht überprüft oder das Protokoll ist zustandslos (stateless) mit Token  
- [ ] Ja — die Authentifizierung erfolgt beim Handshake, aber die Autorisierung für spezifische Aktionen wird pro Nachricht **nicht** erzwungen  
- [ ] Nein — die Authentifizierung wird nur beim Handshake geprüft, was Session Hijacking ermöglicht, um vollen Zugriff zu gewähren  

### Implementiert der Server Rate Limiting oder Ressourcenbeschränkungen für den WebSocket?
- [ ] Ja — Rate Limiting und maximale Nachrichtengrößen sind **aktiviert** und werden erzwungen  
- [ ] Nein — es existieren keine Beschränkungen, was Denial of Service (DoS) durch Message Flooding oder große Payload-Größen ermöglicht

---

## WSTG-CLNT-11 — Test Web Messaging

Web Messaging, oder `postMessage`, ermöglicht die Cross-Origin-Kommunikation zwischen Window-Objekten, wie z. B. einer Parent-Seite und einem erzeugten Pop-up oder einem eingebetteten iframe. Dieser Mechanismus ist für moderne Web-Funktionalitäten entscheidend, birgt jedoch erhebliche Risiken, wenn die empfangende Anwendung die Identität des Absenders nicht verifiziert oder den Message-Payload unsachgemäß verarbeitet. Angreifer nutzen diese Schwachstellen aus, indem sie eine bösartige Website hosten, welche die Zielanwendung einbettet und manipulierte Nachrichten sendet, um unbeabsichtigte Aktionen auszulösen, sensible Daten zu exfiltrieren oder DOM-based Cross-Site Scripting (XSS) auszuführen. Eine erfolgreiche Ausnutzung erfolgt, wenn Message-Listener die `origin`-Eigenschaft nicht strikt validieren oder `message.data` direkt an gefährliche Execution Sinks übergeben.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Werkzeuge:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### Nutzt die Anwendung die `postMessage` API für die dokumentübergreifende Kommunikation?
- [ ] Nein — `postMessage` Listener sind im clientseitigen Code **nicht** vorhanden  
- [ ] Ja — `postMessage` wird für die interne Kommunikation zwischen Komponenten verwendet  
- [ ] Ja — `postMessage` wird verwendet, um mit Drittanbieter-Domains oder Widgets zu kommunizieren  

### Wird die Origin eingehender Nachrichten strikt validiert?
- [ ] Ja — die Origin wird gegen eine strikte Whitelist mittels `===` oder identischem Vergleich geprüft *(Am sichersten)*  
- [ ] Ja — die Origin wird mittels Regex validiert, aber die Logik **ist** anfällig für einen Bypass (z. B. `^https://trusted.com`)  
- [ ] Nein — die `origin`-Eigenschaft wird **nicht** geprüft, was jeder Domain das Senden von Nachrichten erlaubt  
- [ ] Nein — ein Wildcard `*` wird in der `targetOrigin` des Absenders verwendet, was potenziell Daten an bösartige Listener abfließen lässt  

### Wird der Message-Payload bereinigt (sanitized), bevor er von der Anwendung verarbeitet wird?
- [ ] Ja — Daten werden als Klartext behandelt und es werden keine gefährlichen Sinks verwendet  
- [ ] Ja — Daten werden geparst (z. B. `JSON.parse`) und vor der Verwendung validiert, ein Bypass ist **nicht möglich**  
- [ ] Nein — Message-Daten werden direkt an gefährliche Sinks wie `innerHTML`, `eval()` oder `setTimeout()` übergeben  
- [ ] Nein — Message-Daten werden verwendet, um im Fenster zu navigieren oder die `location.href` ohne Validierung zu ändern  

### Kann ein Angreifer durch eine bösartige Nachricht unbefugte Aktionen auslösen oder Daten exfiltrieren?
- [ ] Nein — selbst mit einer gefälschten Nachricht können keine sensiblen Aktionen ausgelöst oder Daten abgerufen werden  
- [ ] Ja — eine manipulierte Nachricht **kann** sensible Zustandsänderungen auslösen (z. B. Passwortänderung, Profilaktualisierung)  
- [ ] Ja — eine manipulierte Nachricht **kann** zu DOM-based XSS oder zur Exfiltration von CSRF-Tokens/Sitzungsdaten führen

---

## WSTG-CLNT-12 — Testen von Browser-Speichern

Das Testen von Browser-Speichern umfasst die Analyse, wie eine Anwendung clientseitige Mechanismen wie LocalStorage, SessionStorage, IndexedDB und WebSQL nutzt, um Daten zu persistieren. Unsicher gespeicherte sensible Informationen – einschließlich PII, Authentifizierungs-Token oder Sitzungs-Identifier – können kompromittiert werden, wenn ein Angreifer Cross-Site Scripting (XSS) erreicht oder physischen Zugriff auf das Gerät des Benutzers erhält. Pentester müssen feststellen, ob Daten im Klartext gespeichert werden, ob sie nach dem Logout zugänglich bleiben und ob der Storage-Lebenszyklus angemessen verwaltet wird, um Datenlecks zu verhindern. Aus der Sicht eines Angreifers sind diese Speichermechanismen lukrative Ziele für das Exfiltrieren von zustandserhaltenden Informationen, die Session Hijacking oder langfristige Account-Persistenz ermöglichen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel / Hoch* |

> *Die Kritikalität wird als „Hoch“ eingestuft, wenn Sitzungs-Token oder sensible PII in LocalStorage gespeichert werden und über XSS zugänglich sind.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### Speichert die Anwendung sensible Informationen im Browser-Speicher?
- [ ] Nein — die Anwendung verwendet **keinen** Browser-Speicher oder speichert nur unkritische UI-Zustände  
- [ ] Ja — sensible Daten werden gespeichert, sind jedoch mit robuster clientseitiger Kryptographie **verschlüsselt**  
- [ ] Ja — sensible Daten (Tokens, PII, Geheimnisse) werden im **Klartext** gespeichert *(Mittel)*  

### Sind gespeicherte Daten für unbefugte Skripte zugänglich (XSS-Risiko)?
- [ ] Nein — Daten werden in HttpOnly-Cookies gespeichert (nicht im Browser-Speicher) oder der Speicher wird **nicht genutzt**  
- [ ] Ja — LocalStorage/SessionStorage wird verwendet, wodurch Daten über jeden XSS-Payload **vollständig zugänglich** sind *(Hoch)*  

### Wird der Browser-Speicher beim Logout des Benutzers ordnungsgemäß gelöscht?
- [ ] Ja — der gesamte anwendungsbezogene Speicher wird während des Logout-Prozesses **explizit gelöscht**  
- [ ] Nein — der Speicher bleibt nach dem Logout bestehen, enthält aber **keine** sensiblen Sitzungsdaten  
- [ ] Nein — Authentifizierungs-Token oder PII **verbleiben** nach Beendigung der Sitzung im Speicher *(Mittel)*  

### Verwendet die Anwendung IndexedDB oder WebSQL für große/sensible Datensätze?
- [ ] Nein — diese Speichermechanismen werden **nicht verwendet**  
- [ ] Ja — Daten werden gespeichert und **ordnungsgemäß bereinigt (sanitized)**, bevor sie im DOM gerendert werden  
- [ ] Ja — sensible Datensätze werden im **Klartext** in IndexedDB/WebSQL-Strukturen gespeichert  

### Kann die Integrität der gespeicherten Daten manipuliert werden, um die Anwendungslogik zu beeinflussen?
- [ ] Nein — die Anwendung validiert oder signiert Daten, bevor sie aus dem Speicher verarbeitet werden  
- [ ] Ja — das Ändern von Speicherwerten (z. B. Benutzerrollen, Flags) **ist möglich** und führt zu einem veränderten Anwendungsverhalten

---

## WSTG-CLNT-13 — Prüfung auf Cross-Site Script Inclusion

Cross-Site Script Inclusion (XSSI) tritt auf, wenn eine Webanwendung sensible Daten innerhalb einer dynamisch generierten JavaScript-Datei oder einer JSONP-Antwort exportiert, welche eine vom Angreifer kontrollierte Website anschließend über ein `<script>`-Tag einbinden kann. Da die Einbindung von Skripten die Same-Origin Policy (SOP) umgeht, kann ein Angreifer das Skript in seinem eigenen Kontext ausführen und globale Objekte oder Variablen überschreiben, um die sensiblen Informationen zu exfiltrieren. Diese Schwachstelle manifestiert sich typischerweise in authentifizierten Endpunkten, die benutzerspezifische Daten wie E-Mail-Adressen, API-Schlüssel oder Sitzungsmetadaten im Skriptformat zurückgeben. Aus der Perspektive eines Angreifers beinhaltet die Ausnutzung das Erstellen einer bösartigen Seite, die auf das verwundbare Skript verweist und die resultierenden Änderungen an globalen Variablen oder Funktionsaufrufe erfasst.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch |

> *Der Schweregrad wird als Hoch eingestuft, wenn die geleakten Daten CSRF-Token, Sitzungsidentifikatoren oder hochsensible PII (Personally Identifiable Information) enthalten.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Geben authentifizierte Endpunkte dynamisches JavaScript oder JSONP zurück, das sensible Informationen enthält?
- [ ] Nein — alle Skripte sind statisch und können **keine** benutzerspezifischen Daten enthalten  
- [ ] Ja — dynamische Skripte existieren, enthalten aber **keine** sensiblen Informationen  
- [ ] Ja — dynamische Skripte enthalten PII oder Token und **sind** herkunftsübergreifend (cross-origin) zugänglich  

### Ist der Header `X-Content-Type-Options: nosniff` bei Antworten mit sensiblen Skripten implementiert?
- [ ] Ja — der Header wird **angewendet** und verhindert MIME-Type-Sniffing  
- [ ] Nein — der Header **fehlt**, was potenziell dazu führen kann, dass Nicht-Skript-Inhalte als Skript ausgeführt werden  

### Sind sensible Skripte durch Anti-XSSI-Mechanismen wie nicht ausführbare Präfixe oder benutzerdefinierte Header geschützt?
- [ ] Ja — Skripte erfordern einen benutzerdefinierten Header oder ein Token, das **nicht** über ein standardmäßiges `<script>`-Tag gesendet werden kann  
- [ ] Ja — Skripte verwenden nicht ausführbare Präfixe (z. B. `)]}'`), die **nicht** einfach umgangen werden können  
- [ ] Nein — Skripte sind direkt über GET-Anfragen unter Verwendung von Umgebungscredentials (Ambient Credentials) zugänglich *(Kritisch)*  

### Ist die Exfiltration sensibler Daten durch das Überschreiben globaler Variablen oder Prototypen möglich?
- [ ] Nein — sensible Daten sind lokal begrenzt (scoped) oder durch moderne Browser-Funktionen geschützt  
- [ ] Ja — sensible Daten werden globalen Variablen zugewiesen und **können** erfasst werden  
- [ ] Ja — Daten werden über JSONP-Callbacks geleakt, die abgefangen werden **können**

---

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

## WSTG-APIT-99 — Testen von GraphQL

GraphQL ist eine Abfragesprache für APIs, die es Clients ermöglicht, spezifische Datenstrukturen anzufordern. Fehlkonfigurationen führen jedoch häufig zu erheblichen Sicherheitsrisiken, einschließlich Information Disclosure und Resource Exhaustion. Schwachstellen entstehen typischerweise durch aktiviertes Introspection, das Fehlen von Limits für Query Depth/Complexity und Broken Object-Level Authorization (BOLA) innerhalb der Resolver-Funktionen. Angreifer nutzen diese Endpunkte aus, indem sie Introspection verwenden, um das gesamte Schema abzubilden, zirkuläre Fragmente einsetzen, um einen Denial of Service (DoS) auszulösen, oder die Autorisierung umgehen, indem sie über verschachtelte Abfragen auf unbefugte Felder zugreifen. Das Testen konzentriert sich auf die Endpunkte `/graphql`, `/v1/graphql` oder `/graphiql`, um sicherzustellen, dass die Implementierung strenge Zugriffskontrollen und Ressourcenmanagement erzwingt.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch* |

> *Der Schweregrad wird als Kritisch eingestuft, wenn Broken Object-Level Authorization (BOLA) den unbefugten Zugriff auf PII oder administrative Mutationen ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Tools:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### Ist das GraphQL-Introspection-System aktiviert?
- [ ] Nein — Introspection ist **deaktiviert** und das Schema kann nicht abgebildet werden  
- [ ] Ja — Introspection ist **aktiviert**, erfordert jedoch eine administrative Authentifizierung  
- [ ] Ja — Introspection ist **aktiviert** und öffentlich zugänglich *(Hoch)*  

### Werden Ressourcenbeschränkungen (Tiefe und Komplexität) für Abfragen erzwungen?
- [ ] Ja — Strenge Limits für Query Depth und Complexity werden **angewendet** und erzwungen  
- [ ] Ja — Limits werden **angewendet**, können aber durch Aliase oder Fragmente umgangen werden  
- [ ] Nein — Es werden keine Limits **angewendet**, was rekursive Abfragen und DoS ermöglicht  

### Liegt Broken Object-Level Authorization (BOLA) in den Resolvern vor?
- [ ] Nein — Die Autorisierung wird für jede Abfrage auf Feld- und Objektebene **angewendet**  
- [ ] Ja — Die Autorisierung wird auf einige Felder **angewendet**, aber sensible Objekte sind exponiert  
- [ ] Ja — Die Autorisierung wird **nicht angewendet**, was den Zugriff auf beliebige Objekte über ID-Manipulation ermöglicht  

### Sind GraphQL-Mutationen ordnungsgemäß gegen unbefugte Nutzung geschützt?
- [ ] Nein — Mutationen sind im Schema **nicht** vorhanden  
- [ ] Ja — Mutationen erfordern eine gültige Authentifizierung und strenge Autorisierungsprüfungen  
- [ ] Ja — Mutationen sind für nicht authentifizierte Benutzer **möglich** oder es fehlt an Autorisierung  

### Geben Fehlermeldungen sensible Details zur Implementierung oder zum Schema preis?
- [ ] Nein — Fehlermeldungen sind generisch und geben **keine** interne Logik preis  
- [ ] Ja — Fehlermeldungen geben zugrunde liegende Datenbanktypen oder Vorschläge über „Did you mean...?“ preis  
- [ ] Ja — Vollständige Stack-Traces und sensible Konfigurationsdaten werden in den Antworten **offengelegt**