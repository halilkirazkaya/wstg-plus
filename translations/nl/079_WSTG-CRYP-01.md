## WSTG-CRYP-01 — Testen op zwakke Transport Layer Security

Het testen op zwakke transport layer security omvat het analyseren van de implementatie en configuratie van SSL/TLS om de vertrouwelijkheid en integriteit van gegevens tijdens het transport te waarborgen. Aanvallers richten zich op misconfiguraties zoals verouderde protocollen (SSLv2, TLS 1.0), zwakke cipher suites (NULL, RC4 of anoniem) en ongeldige of zwak ondertekende certificaten om Man-in-the-Middle (MitM) aanvallen uit te voeren. Deze test richt zich doorgaans op de webserver of load balancer-endpoints van de applicatie waar versleutelde communicatie tot stand wordt gebracht. Succesvolle exploitatie stelt een tegenstander in staat om gevoelige gegevens te onderscheppen, gebruikerssessies te kapen of verbindingen te downgraden naar onveilige staten via protocol downgrade of traffic decryption.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Gemiddeld* |

> *De ernst is Hoog als gevoelige gegevens (PII, inloggegevens, session tokens) worden verzonden; Gemiddeld voor algemeen informatief verkeer.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Worden verouderde of onveilige TLS/SSL-protocollen ondersteund?
- [ ] Nee — alleen TLS 1.2 en TLS 1.3 zijn **ingeschakeld**  
- [ ] Ja — TLS 1.0 of TLS 1.1 zijn **ingeschakeld**  
- [ ] Ja — SSLv2 of SSLv3 zijn **ingeschakeld** *(Kritiek)*  

### Worden zwakke of onveilige cipher suites geaccepteerd door de server?
- [ ] Nee — alleen sterke, moderne ciphers (bijv. AES-GCM, ChaCha20) worden **ondersteund**  
- [ ] Ja — ciphers met zwakke versleuteling (bijv. 3DES, RC4) of lage bitlengte worden **ondersteund**  
- [ ] Ja — NULL, anonieme (ADH) of export ciphers worden **ondersteund** *(Kritiek)*  

### Is het servercertificaat geldig en vertrouwd?
- [ ] Ja — certificaat is geldig, komt overeen met het domein en is ondertekend door een **vertrouwde CA**  
- [ ] Nee — certificaat is **verlopen** of gebruikt een **zwak signature-algoritme** (bijv. SHA-1)  
- [ ] Nee — certificaat is **self-signed** of er is sprake van een **domeinnaam-mismatch**  

### Ondersteunt de server Secure Renegotiation en voorkomt deze Downgrade-aanvallen?
- [ ] Ja — Secure Renegotiation wordt **ondersteund** en TLS Fallback SCSV is **ingeschakeld**  
- [ ] Nee — Secure Renegotiation wordt **niet ondersteund**, wat mogelijk MitM-injectie toestaat  
- [ ] Nee — Server is kwetsbaar voor **POODLE** of **ROBOT** aanvallen  

### Is HTTP Strict Transport Security (HSTS) correct geïmplementeerd?

| Header | Aanwezig | Ontbrekend |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Ja — HSTS-header is aanwezig met een lange `max-age` en `includeSubDomains` is **ingeschakeld**  
- [ ] Ja — HSTS-header is aanwezig maar `includeSubDomains` ontbreekt of heeft een **korte max-age**  
- [ ] Nee — HSTS-header is **niet aanwezig**  

---