## WSTG-ATHN-08 — Verifica di risposte deboli alle domande di sicurezza

La verifica delle risposte deboli alle domande di sicurezza comporta la valutazione dei meccanismi di autenticazione basata sulla conoscenza (Knowledge-Based Authentication - KBA) utilizzati per verificare l'identità di un utente, tipicamente durante il recupero della password o i flussi di autenticazione a più fattori. Questo controllo è rilevante poiché le domande di sicurezza spesso si basano su informazioni statiche e non segrete che possono essere facilmente scoperte tramite Open-Source Intelligence (OSINT) o Social Engineering, portando a una sottrazione non autorizzata dell'account (Account Takeover). Queste vulnerabilità si verificano solitamente nei moduli "Password dimenticata" o "Sblocco account" in cui gli utenti forniscono risposte a domande preimpostate. Dal punto di vista di un attaccante, questo è un bersaglio primario per attacchi Brute Force automatizzati o ricerche mirate, poiché molti utenti forniscono risposte prevedibili o pubblicamente verificabili a domande comuni come "Qual è il nome da nubile di tua madre?" o "Quale scuola superiore hai frequentato?".


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se la domanda di sicurezza è l'unico requisito per un reset completo della password e l'account takeover senza ulteriore verifica.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Strumenti:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### L'applicazione implementa domande di sicurezza per la verifica dell'identità o il recupero della password?
- [ ] No — le domande di sicurezza **non sono utilizzate** per l'autenticazione o il recupero  
- [ ] Sì — le domande di sicurezza **sono utilizzate** come fattore secondario opzionale  
- [ ] Sì — le domande di sicurezza **sono utilizzate** come meccanismo di recupero primario/unico *(Alto Rischio)*  

### Le domande di sicurezza sono selezionate da un elenco fisso o sono definite dall'utente?
- [ ] No — le domande sono interamente definite dall'utente e richiedono un'elevata entropia  
- [ ] Sì — le domande sono fisse ma includono opzioni non individuabili tramite OSINT  
- [ ] Sì — le domande provengono da un elenco fisso di argomenti comuni individuabili tramite OSINT *(Vulnerabile)*  

### Vengono applicati il Rate Limiting o il blocco dell'account ai tentativi di risposta alle domande di sicurezza?
- [ ] Sì — vengono applicati un rigoroso Rate Limiting e il blocco dell'account per prevenire il Brute Force  
- [ ] Sì — il Rate Limiting **è applicato** ma l'aggiramento **è possibile** tramite rotazione degli IP o manipolazione degli header  
- [ ] No — **nessun Rate Limiting** è imposto sui tentativi di risposta  

### L'applicazione impone requisiti di complessità o validazione sul contenuto della risposta?
- [ ] Sì — la validazione impedisce l'uso di parole comuni e garantisce la complessità/univocità della risposta  
- [ ] Sì — è presente una validazione di base ma **può** essere aggirata con wordlist comuni o attacchi a dizionario  
- [ ] No — **non viene eseguita alcuna validazione**; sono ammesse risposte semplici o a carattere singolo  

### La sfida della domanda di sicurezza può essere aggirata attraverso falle logiche?
- [ ] No — il flusso di recupero richiede rigorosamente una risposta valida per procedere  
- [ ] Sì — il flusso di recupero **può** essere aggirato manipolando i codici di risposta HTTP o i parametri di sessione  
- [ ] Sì — la risposta è riflessa nel codice sorgente o nei campi nascosti della pagina  

---