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