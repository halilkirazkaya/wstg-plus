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