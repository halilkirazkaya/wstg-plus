## WSTG-CONF-01 — Testen van de netwerkinfrastructuurconfiguratie

Het testen van de netwerkinfrastructuurconfiguratie omvat het identificeren van zwakheden in de onderliggende server- en netwerkcomponenten die de webapplicatie ondersteunen. Aanvallers richten zich op verkeerd geconfigureerde services, verouderde protocollen en onnodige open poorten om voet aan de grond te krijgen, zich zijwaarts door het netwerk te bewegen (Lateral Movement) of verkenningen (Reconnaissance) uit te voeren. Veelvoorkomende bevindingen zijn onveilige TLS-versies, uitgebreide (verbose) service banners en blootgestelde administratieve interfaces die beperkt zouden moeten blijven tot interne netwerken. Door deze tekortkomingen op infrastructuurniveau te misbruiken, kan een tegenstander de vertrouwelijkheid van gegevens in transit in gevaar brengen of ongeautoriseerde toegang verkrijgen tot het host-besturingssysteem.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Tools:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Zijn er onnodige open poorten of services blootgesteld op de applicatiehost?
- [ ] Nee — alleen vereiste poorten (bijv. 443) zijn **toegankelijk**  
- [ ] Ja — aanvullende services zijn blootgesteld maar volgen een **veilige** configuratie  
- [ ] Ja — niet-essentiële services (bijv. FTP, Telnet, SMB) zijn publiekelijk **blootgesteld**  

### Lekken service banners of headers gevoelige versie-informatie?
- [ ] Nee — banners zijn verwijderd of generiek  
- [ ] Ja — banners onthullen serversoftware (bijv. Apache/nginx) maar **geen** versienummers  
- [ ] Ja — verbose banners onthullen specifieke softwareversies, wat **Exploit**-pogingen vergemakkelijkt  

### Volgt de TLS-configuratie de moderne security best practices?
- [ ] Ja — alleen TLS 1.2/1.3 is **ingeschakeld** met sterke cipher suites  
- [ ] Ja — verouderde protocollen (TLS 1.0/1.1) zijn **ingeschakeld**  
- [ ] Nee — zwakke ciphers of onveilige protocollen (SSLv2/v3) zijn **ingeschakeld**  

### Zijn administratieve of beheerinterfaces beperkt tot geautoriseerde netwerken?
- [ ] Ja — interfaces (bijv. SSH, RDP, controlepanelen) zijn **niet toegankelijk** vanaf het publieke internet  
- [ ] Nee — interfaces zijn publiekelijk **toegankelijk** maar vereisen multi-factor authenticatie  
- [ ] Nee — interfaces zijn publiekelijk **toegankelijk** met single-factor authenticatie of standaardgegevens (default credentials)  

---