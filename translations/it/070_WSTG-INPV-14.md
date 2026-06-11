## WSTG-INPV-14 — Testing for Incubated Vulnerabilities

Le vulnerabilità incubate (incubated vulnerabilities), chiamate anche vulnerabilità persistenti o di secondo ordine (second-order vulnerabilities), si verificano quando un payload malevolo viene memorizzato dall'applicazione in uno stato "freddo" e successivamente eseguito o elaborato in un contesto differente. Questo pattern di attacco colpisce tipicamente i sistemi di back-end, le console amministrative o gli strumenti di reportistica automatizzata che recuperano i dati da un database o un filesystem condiviso senza un'adeguata ri-validazione o output encoding. I pentester devono tracciare il ciclo di vita dell'input dell'utente dal punto di ingresso (l'"incubatore") a ogni posizione in cui quel dato viene infine renderizzato o utilizzato da altri componenti o applicazioni secondarie. Un exploit riuscito porta spesso a conseguenze ad alto impatto come l'administrative session hijacking tramite stored XSS o l'esfiltrazione di dati attraverso second-order SQL injection nei moduli di reportistica interna.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Strumenti:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Esistono endpoint in cui l'input controllato dall'utente viene memorizzato per un successivo recupero da parte di altri utenti o processi?
- [ ] No — l'applicazione **non** memorizza l'input dell'utente per un uso successivo  
- [ ] Sì — l'input **è** memorizzato in campi del database, file di log o impostazioni di configurazione  

### Viene applicata la validazione o la sanitizzazione dell'input prima che i dati vengano scritti nel layer di storage persistente?
- [ ] Sì — una validazione rigorosa basata su allow-list **è applicata** al punto di ingresso  
- [ ] Sì — una sanitizzazione generica **è applicata** ma il bypass **è possibile** tramite encoding o caratteri non standard  
- [ ] No — l'input viene memorizzato nella sua forma grezza (raw) e non validata  

### I dati memorizzati sono correttamente codificati o sanitizzati quando vengono renderizzati in interfacce amministrative o secondarie?
- [ ] Sì — l'output encoding contestuale (context-aware) **è applicato** ovunque i dati vengano renderizzati  
- [ ] Sì — l'encoding **è applicato** in alcune posizioni ma **manca** in altre (ad es., dashboard amministrativa interna)  
- [ ] No — i dati vengono renderizzati senza alcun encoding o sanitizzazione, permettendo l'esecuzione del payload  

### I payload memorizzati nel database possono influenzare i processi batch di back-end o i sistemi di monitoraggio out-of-band?
- [ ] No — i processi di back-end utilizzano metodi di parsing sicuri o query parametrizzate (parameterized queries)  
- [ ] Sì — i payload memorizzati **possono** innescare interazioni nella logica di back-end (ad es., CSV injection, command injection nei log)  
- [ ] Sì — i payload memorizzati **possono** innescare richieste out-of-band (OOB) verso server controllati dall'attaccante  

---