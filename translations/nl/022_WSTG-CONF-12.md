## WSTG-CONF-12 — Content Security Policy (CSP)

Content Security Policy (CSP) is een cruciaal defense-in-depth-mechanisme dat wordt geïmplementeerd via HTTP-responseheaders om risico's zoals Cross-Site Scripting (XSS), clickjacking en data-injectie-aanvallen te mitigeren. Door te definiëren welke dynamische bronnen mogen worden geladen, beperkt een goed geconfigureerde CSP de mogelijkheden van een aanvaller om ongeautoriseerde scripts uit te voeren of gevoelige gegevens te exfiltreren naar externe domeinen. Pentesters beoordelen het beleid op overmatig permissieve directieven, het gebruik van onveilige trefwoorden zoals `'unsafe-inline'`, en het vertrouwen op onveilige CDN's die bypasses kunnen vergemakkelijken. Een zwakke of ontbrekende CSP vormt niet direct een kwetsbaarheid, maar verhoogt de impact van succesvolle injectie-aanvallen aanzienlijk door het ontbreken van een secundaire beschermingslaag.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Tools:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Is er een Content Security Policy (CSP) header aanwezig in de applicatieresponses?
- [ ] Ja — `Content-Security-Policy` header is **aanwezig** en wordt **afgedwongen**  
- [ ] Ja — `Content-Security-Policy-Report-Only` is **aanwezig** voor testdoeleinden  
- [ ] Nee — er is geen CSP-header **aanwezig** in de responses  

### Zijn de `script-src` of `default-src` directieven correct beperkt?
- [ ] Ja — directieven maken gebruik van strikte whitelisting, nonces of hashes en een bypass is **niet mogelijk**  
- [ ] Ja — directieven zijn **correct geconfigureerd**, maar de whitelist bevat bekende bypassbare CDN's  
- [ ] Nee — directieven gebruiken wildcards (`*`) of `data:` schema's, waardoor een bypass **mogelijk** is  

### Staat het beleid de uitvoering van onveilige inline scripts of eval toe?
- [ ] Nee — `'unsafe-inline'` en `'unsafe-eval'` zijn **niet aanwezig**  
- [ ] Ja — `'unsafe-inline'` is **aanwezig** maar beschermd door een `nonce` of `hash`  
- [ ] Ja — `'unsafe-inline'` of `'unsafe-eval'` zijn **ingeschakeld** zonder aanvullende bescherming *(Kritiek)*  

### Is de applicatie beschermd tegen clickjacking via de `frame-ancestors` directie?
- [ ] Ja — `frame-ancestors` is ingesteld op `'none'` of `'self'`  
- [ ] Ja — `frame-ancestors` is **ingeschakeld** maar staat specifieke vertrouwde domeinen van derden toe  
- [ ] Nee — `frame-ancestors` **ontbreekt**, er wordt vertrouwd op verouderde `X-Frame-Options` of er is geen bescherming  

### Is er een mechanisme om CSP-schendingen te rapporteren?
- [ ] Ja — `report-uri` of `report-to` is **geconfigureerd** en actief  
- [ ] Nee — rapportage van schendingen is **uitgeschakeld** of **niet geconfigureerd**  

---