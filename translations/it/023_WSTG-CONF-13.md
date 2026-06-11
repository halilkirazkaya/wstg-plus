## WSTG-CONF-13 — Path Confusion

La Path Confusion deriva da discrepanze nel modo in cui diversi componenti web, come reverse proxy, load balancer e server applicativi di backend, analizzano e interpretano i percorsi URL. Gli attaccanti sfruttano queste incoerenze iniettando caratteri specifici come punti e virgola, slash codificati o segmenti punto (dot-segments) per ingannare i filtri di sicurezza e accedere a endpoint limitati o risorse statiche. Questa vulnerabilità può portare a gravi falle di sicurezza, tra cui il bypass dell'autenticazione, l'accesso non autorizzato ai dati e il Web Cache Poisoning, poiché il front-end potrebbe applicare regole di sicurezza a un percorso che il back-end interpreta in modo diverso. Lo sfruttamento (Exploit) con successo si verifica tipicamente al confine architetturale dove la logica di routing delle richieste e la normalizzazione divergono all'interno dello stack tecnologico.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se la path confusion comporta un bypass dell'autenticazione o del controllo degli accessi per endpoint amministrativi sensibili.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### I diversi livelli architetturali interpretano i delimitatori di percorso (es. `;`, `#`, `?`) in modo coerente?
- [ ] Sì — tutti i livelli normalizzano e interpretano i delimitatori di percorso in modo identico  
- [ ] No — esistono incoerenze ma **non possono** essere utilizzate per accedere a risorse limitate  
- [ ] No — **è possibile** che le discrepanze consentano a un attaccante di bypassare i filtri front-end e raggiungere la logica di back-end  

### I controlli di accesso possono essere bypassati utilizzando sequenze di path traversal o caratteri codificati?
- [ ] No — la normalizzazione viene applicata in modo coerente prima dei controlli di sicurezza  
- [ ] Sì — il bypass è **possibile** tramite sequenze di segmenti punto (es. `/admin/..;/`)  
- [ ] Sì — il bypass è **possibile** tramite caratteri codificati in URL (es. `%2f`, `%2e%2e%2f`)  

### L'applicazione è suscettibile al Web Cache Poisoning tramite path confusion?
- [ ] No — la logica di caching non è influenzata dall'ambiguità del percorso o da informazioni di percorso extra  
- [ ] Sì — contenuti malevoli **possono** essere memorizzati in cache per percorsi legittimi a causa di discrepanze di mappatura  
- [ ] Sì — informazioni sensibili **possono** essere memorizzate in cache in directory pubbliche tramite tecniche di `RCD` (Relative Path Overwrite)  

### Il server gestisce le "informazioni di percorso extra" (PathInfo) in modo sicuro?
- [ ] No — la funzionalità è **disabilitata** o non esiste  
- [ ] Sì — `PathInfo` viene gestito e **non** interferisce con i filtri di sicurezza  
- [ ] No — `PathInfo` consente a un attaccante di aggiungere dati che confondono la logica di routing dell'applicazione  

---