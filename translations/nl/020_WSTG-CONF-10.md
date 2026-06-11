## WSTG-CONF-10 — Subdomain Takeover

Subdomain takeover vindt plaats wanneer een DNS-vermelding (meestal een CNAME, maar soms A- of MX-records) verwijst naar een buiten gebruik gestelde of niet-bestaande resource bij een externe cloudprovider of hostingdienst. Aanvallers identificeren deze "dangling" DNS-records door te zoeken naar specifieke foutmeldingen van providers zoals AWS, GitHub of Azure die aangeven dat een resource niet langer wordt geclaimd. Door de verlaten resourcenaam te registreren op het platform van de provider, krijgt de aanvaller de volledige controle over het subdomein. Hierdoor kunnen zij kwaadaardige inhoud hosten, sessiecookies exfiltreren die gericht zijn op het basisdomein, en Content Security Policy (CSP) of CORS-beveiligingen omzeilen. Deze kwetsbaarheid is bijzonder gevaarlijk omdat het gebruikmaakt van het vertrouwen dat verbonden is aan de legitieme domeinstructuur van de organisatie om phishing en datadiefstal met een hoge impact te faciliteren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Hoog / Kritiek* |

> *De ernst wordt Kritiek als het subdomein kan worden gebruikt om sessiecookies van het basisdomein te kapen (hijack) of SSO/OAuth-omleidingen te omzeilen.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Tools:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### Maakt de applicatie gebruik van externe hosting of clouddiensten via subdomeinen?
- [ ] Nee — alle subdomeinen verwijzen naar IP-ruimte die door de organisatie zelf wordt beheerd  
- [ ] Ja — subdomeinen verwijzen naar externe providers (S3, GitHub Pages, Heroku, etc.)  

### Zijn er "dangling" DNS-records die verwijzen naar niet-bestaande resources?
- [ ] Nee — alle geïdentificeerde DNS-records verwijzen naar actieve, beheerde resources  
- [ ] Ja — er bestaan CNAME- of A-records voor resources die **niet meer bestaan** bij de provider  
- [ ] Ja — DNS-records verwijzen naar NXDOMAIN of providerspecifieke foutpagina's *(bijv. "NoSuchBucket")*  

### Kan de geïdentificeerde "dangling" resource succesvol worden geclaimd?
- [ ] Nee — de provider heeft beveiligingen tegen het claimen van verlaten namen of accountverificatie is vereist  
- [ ] Ja — de resourcenaam **kan** door elke gebruiker worden geregistreerd op het platform van de derde partij  

### Gebruikt het basisdomein cookies met een "Domain"-scope die gecompromitteerd kunnen worden?
- [ ] Nee — cookies zijn strikt beperkt tot specifieke subdomeinen of gebruiken `Host-Only` flags  
- [ ] Ja — gevoelige cookies (sessie-ID's, JWT's) zijn ingesteld voor het bovenliggende domein en **kunnen** worden geëxfiltreerd via een gekaapt subdomein  

### Kan het subdomein worden gebruikt om beveiligingsheaders of authenticatiestromen te omzeilen?
- [ ] Nee — er zijn geen beveiligingsafhankelijkheden op de geïdentificeerde subdomeinen  
- [ ] Ja — het subdomein staat op de whitelist in CSP `script-src` of `connect-src` richtlijnen  
- [ ] Ja — het subdomein is een geldige redirect URI voor OAuth- of SSO-configuraties  

---