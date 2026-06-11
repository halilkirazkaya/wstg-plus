## WSTG-ATHN-06 — Test per la Debolezza della Cache del Browser

La debolezza della cache del browser si verifica quando informazioni sensibili vengono memorizzate nella cache locale del browser e possono essere recuperate da un utente non autorizzato con accesso alla stessa macchina fisica. Questa mancanza di adeguati header di controllo della cache consente a dati potenzialmente sensibili, come informazioni personali, dettagli dell'account o identificativi di sessione, di persistere dopo che l'utente ha effettuato il logout o ha chiuso la sessione. Gli attaccanti o gli utenti successivi su terminali condivisi o pubblici possono sfruttare questa vulnerabilità navigando nella cronologia del browser, utilizzando il pulsante "Indietro" o ispezionando i file della cache locale per esfiltrare le risposte memorizzate. Garantire l'implementazione di direttive rigorose come `Cache-Control: no-store` e `Pragma: no-cache` è fondamentale per qualsiasi applicazione che gestisca contenuti autenticati, al fine di assicurare che i dati sensibili non vengano mai scritti su disco.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media* |

> *La severità diventa Media se l'applicazione è consultata frequentemente da chioschi condivisi/pubblici o contiene dati PII/PHI altamente sensibili.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Strumenti:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Sono presenti header cache-control appropriati sulle pagine autenticate o sensibili?
- [ ] Sì — `Cache-Control: no-store, no-cache` e `Pragma: no-cache` sono **presenti** e **configurati correttamente**  
- [ ] Sì — alcuni header sono presenti ma `no-store` è **mancante**, consentendo il caching su disco  
- [ ] No — non è **applicato** alcun header relativo alla cache, affidandosi al comportamento predefinito del browser  

### L'applicazione utilizza la direttiva `private` per i dati specifici dell'utente?
- [ ] Sì — `Cache-Control: private` è **abilitato** per tutti i contenuti personalizzati per impedire il caching da parte dei proxy  
- [ ] No — il contenuto è contrassegnato come `public` o manca della direttiva `private`, rendendolo **archiviabile in cache** dai proxy intermedi  

### Le informazioni sensibili sono accessibili tramite il pulsante "Indietro" del browser dopo un logout riuscito?
- [ ] No — il browser richiede la ri-autenticazione o la pagina **non viene caricata** dalla cache locale  
- [ ] Sì — le informazioni sensibili sono **ancora visibili** tramite il pulsante Indietro dopo che la sessione è stata terminata  

### I file non-HTML sensibili (es. PDF, esportazioni CSV, risposte API JSON) sono protetti dal caching?
- [ ] No — non vengono generati o gestiti file non-HTML sensibili dall'applicazione  
- [ ] Sì — header rigorosi di controllo della cache sono **applicati** a tutti i download di file sensibili e agli endpoint API  
- [ ] No — i download sensibili o le risposte API vengono **memorizzati** nella cache locale del browser o in directory temporanee  

### L'applicazione utilizza `Expires: 0` o una data passata per impedire l'uso di dati obsoleti?
- [ ] Sì — l'header `Expires` è impostato su `0` o su una data passata, **garantendo** che il browser tratti il contenuto come immediatamente obsoleto  
- [ ] No — l'header `Expires` è **mancante** o impostato su una data futura per le pagine sensibili  

---