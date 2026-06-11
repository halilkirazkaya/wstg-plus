## WSTG-APIT-01 — API Reconnaissance

La ricognizione API è il processo sistematico di identificazione e mappatura della superficie di attacco delle API per scoprire endpoint, metodi supportati e strutture dati sottostanti. Gli attaccanti eseguono questa fase per scoprire API non documentate (shadow API), versioni deprecate con vulnerabilità legacy e file di documentazione esposti, come definizioni Swagger o OpenAPI, che rivelano la logica di business interna. Analizzando pattern URL prevedibili, file JavaScript lato client e risultati di brute-force di directory, un avversario può costruire una mappa completa dell'API per identificare obiettivi per attacchi di Broken Object Level Authorization (BOLA) o Mass Assignment. Questa ricognizione tipicamente prende di mira percorsi comuni come `/api/v1/`, `/swagger.json` o `/graphql` per ottenere una roadmap delle funzionalità backend dell'applicazione.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Informativa / Bassa |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Strumenti:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### I file di documentazione API (Swagger, OpenAPI, WSDL) sono accessibili pubblicamente?
- [ ] No — La documentazione API non è accessibile o è limitata da autenticazione  
- [ ] Sì — La documentazione è accessibile ma richiede credenziali valide  
- [ ] Sì — La documentazione API (es. `/swagger-ui.html`) è **accessibile pubblicamente** senza autenticazione  

### È possibile scoprire endpoint API non documentati o "shadow" tramite fuzzing?
- [ ] No — Il fuzzing di directory ed endpoint non ha prodotto percorsi non documentati  
- [ ] Sì — Sono stati trovati endpoint non documentati, ma **non** è possibile accedervi senza autorizzazione  
- [ ] Sì — Sono stati trovati endpoint non documentati e **possono** essere consultati senza una valida autorizzazione  

### L'applicazione rivela versioni multiple dell'API (es. /v1/, /v2/, /beta/)?
- [ ] No — È accessibile solo la versione API corrente e protetta  
- [ ] Sì — Esistono versioni legacy, ma implementano i **medesimi** controlli di sicurezza della versione corrente  
- [ ] Sì — Le versioni legacy (es. `/v1/`) sono accessibili e **mancano** dei controlli di sicurezza delle versioni più recenti  

### Le risorse lato client (JavaScript/App Mobile) espongono strutture di endpoint API o chiavi?
- [ ] No — Il codice lato client **non** contiene percorsi API cablati (hardcoded) o chiavi sensibili  
- [ ] Sì — Il codice lato client contiene mappe degli endpoint API ma **nessuna** chiave sensibile  
- [ ] Sì — Chiavi API, segreti o URL di endpoint esclusivamente interni **sono** cablati (hardcoded) nelle risorse lato client  

### Sono individuabili parametri o header nascosti su endpoint noti?
- [ ] No — Il fuzzing dei parametri (es. tramite `Arjun`) **non** ha rivelato input nascosti  
- [ ] Sì — Sono stati scoperti parametri nascosti (es. `debug=true`, `admin=1`), ma **non** sono funzionali  
- [ ] Sì — Sono stati scoperti parametri nascosti che **possono** essere utilizzati per alterare il comportamento dell'applicazione  

---