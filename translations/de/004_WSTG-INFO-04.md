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