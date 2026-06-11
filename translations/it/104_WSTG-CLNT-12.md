## WSTG-CLNT-12 — Test del Browser Storage

Il test del browser storage consiste nell'analizzare come un'applicazione utilizzi meccanismi lato client come LocalStorage, SessionStorage, IndexedDB e WebSQL per la persistenza dei dati. Le informazioni sensibili memorizzate in modo non sicuro — inclusi PII, token di autenticazione o identificativi di sessione — possono essere compromesse se un attaccante realizza un attacco di Cross-Site Scripting (XSS) o ottiene l'accesso fisico al dispositivo dell'utente. I pentester devono determinare se i dati sono memorizzati in chiaro (cleartext), se rimangono accessibili dopo il logout e se il ciclo di vita dell'archiviazione è gestito in modo appropriato per prevenire il data leakage. Dal punto di vista di un attaccante, questi meccanismi di archiviazione sono obiettivi lucrativi per l'esfiltrazione di informazioni di stato che consentono il session hijacking o la persistenza dell'account a lungo termine.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se i token di sessione o PII sensibili sono memorizzati nel LocalStorage e risultano accessibili tramite XSS.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### L'applicazione memorizza informazioni sensibili nel browser storage?
- [ ] No — l'applicazione **non** utilizza il browser storage o memorizza solo stati dell'interfaccia utente (UI) non sensibili  
- [ ] Sì — i dati sensibili sono memorizzati ma **sono cifrati** utilizzando una crittografia lato client robusta  
- [ ] Sì — dati sensibili (token, PII, segreti) sono memorizzati in **chiaro** (cleartext) *(Media)*  

### I dati memorizzati sono accessibili a script non autorizzati (rischio XSS)?
- [ ] No — i dati sono memorizzati in cookie HttpOnly (non nel browser storage) o lo storage **non è utilizzato**  
- [ ] Sì — viene utilizzato LocalStorage/SessionStorage, rendendo i dati **completamente accessibili** tramite qualsiasi payload XSS *(Alta)*  

### Il browser storage viene pulito correttamente al logout dell'utente?
- [ ] Sì — tutto lo storage relativo all'applicazione viene **esplicitamente pulito** durante il processo di logout  
- [ ] No — lo storage persiste dopo il logout, ma **non** contiene dati di sessione sensibili  
- [ ] No — i token di autenticazione o i PII **persistono** nello storage dopo il termine della sessione *(Media)*  

### L'applicazione utilizza IndexedDB o WebSQL per dataset grandi o sensibili?
- [ ] No — questi meccanismi di archiviazione **non sono utilizzati**  
- [ ] Sì — i dati sono memorizzati e **correttamente sanitizzati** prima di essere renderizzati nel DOM  
- [ ] Sì — dataset sensibili sono memorizzati in **chiaro** (cleartext) all'interno delle strutture IndexedDB/WebSQL  

### L'integrità dei dati memorizzati può essere manipolata per influenzare la logica dell'applicazione?
- [ ] No — l'applicazione convalida o firma i dati prima di elaborarli dallo storage  
- [ ] Sì — la modifica dei valori nello storage (es. ruoli utente, flag) **è possibile** e comporta un'alterazione del comportamento dell'applicazione  

---