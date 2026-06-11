## WSTG-INPV-15 — HTTP Splitting Smuggling

HTTP Splitting- en Smuggling-kwetsbaarheden ontstaan door discrepanties in de manier waarop frontend-proxy's en backend-servers HTTP-verzoekgrenzen interpreteren en verwerken, met name met betrekking tot de `Content-Length` en `Transfer-Encoding` headers. Door dubbelzinnige verzoeken te formuleren, kan een aanvaller een verborgen verzoek naar de backend "smokkelen" of CRLF-sequenties injecteren om een respons te splitsen, wat leidt tot cache poisoning, request hijacking of het omzeilen van security controls. Deze gebreken manifesteren zich doorgaans in complexe omgevingen die gebruikmaken van reverse proxies, load balancers of CDN's met inconsistente parsing-logica. Vanuit het perspectief van een aanvaller maakt dit het omleiden van gebruikersverkeer, diefstal van gevoelige sessietokens en het uitvoeren van ongeautoriseerde acties binnen de context van sessies van andere gebruikers mogelijk.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Is de omgeving vatbaar voor request smuggling via CL.TE- of TE.CL-discrepanties?
- [ ] Nee — frontend- en backend-servers verwerken verzoekgrenzen consistent  
- [ ] Ja — er bestaan discrepanties, maar exploitatie is **niet mogelijk** vanwege infrastructuur-mitigaties  
- [ ] Ja — CL.TE- of TE.CL-smuggling **is mogelijk**, waardoor verborgen verzoeken de backend kunnen bereiken *(Hoog)*  
- [ ] Ja — TE.TE (dubbele codering/obfuscatie) **is mogelijk** om frontend-filters te omzeilen *(Kritiek)*  

### Kan HTTP Response Splitting worden bereikt via CRLF-injectie in headers?
- [ ] Nee — de applicatie saneert CRLF-sequenties correct in alle header-inputs  
- [ ] Ja — CRLF-sequenties worden gereflecteerd in headers, maar response splitting is **niet mogelijk**  
- [ ] Ja — CRLF-injectie **is mogelijk**, wat header-injectie of cache poisoning toestaat  

### Worden security controls (WAF/ACLs) omzeild met behulp van gesmokkelde verzoeken?
- [ ] Nee — security controls zijn van toepassing op zowel de buitenste als de gesmokkelde verzoeken  
- [ ] Ja — gesmokkelde verzoeken **kunnen** frontend WAF-regels of op IP gebaseerde ACL's omzeilen  

### Is het mogelijk om sessies van andere gebruikers te kapen of hun verkeer om te leiden?
- [ ] Nee — verzoek/respons-stromen zijn geïsoleerd en kunnen niet worden gekruist  
- [ ] Ja — request smuggling **is mogelijk** en maakt het vastleggen van verzoeken van andere gebruikers mogelijk *(Kritiek)*  
- [ ] Ja — response splitting **is mogelijk** en maakt browser-side cache poisoning of XSS mogelijk  

---