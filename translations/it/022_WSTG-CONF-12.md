## WSTG-CONF-12 — Content Security Policy (CSP)

La Content Security Policy (CSP) è un meccanismo critico di difesa in profondità (defense-in-depth) implementato tramite gli header di risposta HTTP per mitigare rischi come Cross-Site Scripting (XSS), clickjacking e attacchi di data injection. Definendo quali risorse dinamiche possono essere caricate, una CSP ben configurata limita la capacità di un utente malintenzionato di eseguire script non autorizzati o di esfiltrare dati sensibili verso domini esterni. I pentester valutano la policy alla ricerca di direttive eccessivamente permissive, dell'uso di parole chiave non sicure come `'unsafe-inline'` e dell'affidamento a CDN non sicure che potrebbero facilitare i bypass. Una CSP debole o mancante non crea direttamente una vulnerabilità, ma aumenta significativamente l'impatto di attacchi injection andati a buon fine, non fornendo un secondo livello di protezione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Strumenti:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### L'header Content Security Policy (CSP) è presente nelle risposte dell'applicazione?
- [ ] Sì — l'header `Content-Security-Policy` è **presente** ed **applicato**  
- [ ] Sì — `Content-Security-Policy-Report-Only` è **presente** per test  
- [ ] No — nessun header CSP è **presente** nelle risposte  

### Le direttive `script-src` o `default-src` sono correttamente limitate?
- [ ] Sì — le direttive utilizzano whitelist rigorose, nonce o hash e il bypass **non è possibile**  
- [ ] Sì — le direttive sono **correttamente configurate** ma la whitelist include CDN con bypass noti  
- [ ] No — le direttive utilizzano wildcard (`*`) o schemi `data:`, rendendo **possibile** il bypass  

### La policy consente l'esecuzione di script inline non sicuri o l'uso di eval?
- [ ] No — `'unsafe-inline'` e `'unsafe-eval'` **non sono presenti**  
- [ ] Sì — `'unsafe-inline'` è **presente** ma protetto da un `nonce` o `hash`  
- [ ] Sì — `'unsafe-inline'` o `'unsafe-eval'` sono **abilitati** senza protezioni aggiuntive *(Critico)*  

### L'applicazione è protetta contro il clickjacking tramite la direttiva `frame-ancestors`?
- [ ] Sì — `frame-ancestors` è impostato su `'none'` o `'self'`  
- [ ] Sì — `frame-ancestors` è **abilitato** ma consente specifici domini di terze parti fidati  
- [ ] No — `frame-ancestors` è **assente**, si affida al legacy `X-Frame-Options` o a nessuna protezione  

### Esiste un meccanismo per segnalare le violazioni della CSP?
- [ ] Sì — `report-uri` o `report-to` è **configurato** e attivo  
- [ ] No — il reporting delle violazioni è **disabilitato** o **non configurato**  

---