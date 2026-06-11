## WSTG-CRYP-03 — Testen op gevoelige informatie verzonden via niet-versleutelde kanalen

Het verzenden van gevoelige gegevens via niet-versleutelde kanalen, zoals onversleuteld HTTP, stelt informatie bloot aan onderschepping en wijziging via Man-in-the-Middle (MITM) aanvallen. Deze kwetsbaarheid treedt doorgaans op wanneer webapplicaties nalaten Transport Layer Security (TLS) af te dwingen voor alle eindpunten, met name in legacy-componenten, inlogformulieren of tijdens de initiële overgang van HTTP naar HTTPS. Vanuit het perspectief van een aanvaller maakt dit het passief sniffen van authenticatiegegevens, sessietokens en persoonlijk identificeerbare informatie (PII) mogelijk op gecompromitteerde of onbetrouwbare netwerksegmenten zoals openbare wifi. Het waarborgen dat alle gevoelige gegevens zijn ingekapseld binnen een robuuste TLS-tunnel is fundamenteel voor het behoud van de vertrouwelijkheid en integriteit van gebruikersinteracties en het voorkomen van session hijacking.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Gemiddeld |

> *De ernst wordt Hoog als authenticatiegegevens, sessie-identificatoren of PII in cleartext worden verzonden.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### Staat de applicatie communicatie via niet-versleuteld HTTP toe voor gevoelige eindpunten?
- [ ] Nee — al het verkeer van de applicatie wordt strikt gedwongen over HTTPS *(Meest veilig)*  
- [ ] Ja — niet-gevoelige pagina's zijn toegankelijk via HTTP, maar gevoelige eindpunten **niet**  
- [ ] Ja — gevoelige eindpunten (login, checkout, profiel) **zijn** toegankelijk via niet-versleuteld HTTP *(Hoog risico)*  

### Is er een globale omleiding van HTTP naar HTTPS?
- [ ] Ja — de applicatie voert een 301/302 redirect uit naar HTTPS voor alle HTTP-verzoeken  
- [ ] Ja — omleiding wordt alleen uitgevoerd voor specifieke gevoelige eindpunten  
- [ ] Nee — er wordt **geen** omleiding van HTTP naar HTTPS toegepast  

### Is HTTP Strict Transport Security (HSTS) geïmplementeerd?
- [ ] Ja — HSTS is **ingeschakeld** met een lange `max-age` en bevat de `includeSubDomains` en `preload` directives  
- [ ] Ja — HSTS is **ingeschakeld** maar mist de `includeSubDomains` of `preload` directives  
- [ ] Nee — HSTS is **niet ingeschakeld** op de applicatieserver  

### Zijn gevoelige cookies beschermd tegen verzending in cleartext?

| Cookienaam | Secure Flag aanwezig | Secure Flag ontbreekt |
|------------|:--------------------:|:---------------------:|
| `SessionID` |                      |                       |
| `AuthToken` |                      |                       |
| `CSRF-Token`|                      |                       |

### Verzendt de applicatie gevoelige gegevens naar API's van derden via niet-versleutelde kanalen?
- [ ] Nee — alle externe API-calls worden uitgevoerd via versleutelde (HTTPS) tunnels  
- [ ] Ja — sommige niet-gevoelige telemetrie wordt verzonden via HTTP  
- [ ] Ja — API-keys, PII of inloggegevens **worden** naar derden verzonden via niet-versleuteld HTTP  

---