## WSTG-INPV-05 — SQL Injection

La SQL Injection si verifica quando l'input fornito dall'utente viene incorporato nelle query SQL senza una corretta parametrizzazione o sanitizzazione, consentendo agli aggressori di manipolare la logica della query. L'exploitation andata a buon fine può portare all'esfiltrazione non autorizzata di dati, al bypass dell'autenticazione, alla modifica dei dati e, in alcuni casi, alla Remote Code Execution tramite funzionalità del database come `xp_cmdshell` o `LOAD_FILE()`. Questa vulnerabilità interessa comunemente i moduli di login, le funzionalità di ricerca, i parametri delle REST API, gli header HTTP e qualsiasi endpoint che costruisce query SQL dinamiche a partire da input controllati dall'utente. Dal punto di vista di un attaccante, l'identificazione di un punto di iniezione rappresenta il gate primario per la compromissione completa del database e il potenziale movimento laterale all'interno dell'infrastruttura.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Strumenti:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### I parametri controllabili dall'utente sono elaborati utilizzando metodi di accesso al database sicuri?
- [ ] Sì — l'applicazione utilizza esclusivamente ORM o query parametrizzate e il bypass **non è possibile**  
- [ ] Sì — vengono utilizzate query parametrizzate ma il bypass **è possibile** tramite casi limite (es. clausole `ORDER BY`)  
- [ ] No — viene utilizzata la concatenazione di stringhe grezze per costruire query SQL **senza** parametrizzazione  

### Il meccanismo di autenticazione può essere aggirato tramite SQL injection?
- [ ] No — i parametri di login **non** sono vulnerabili a injection  
- [ ] Sì — il bypass dell'autenticazione **è possibile** utilizzando payload basati su tautologie (es. `' OR 1=1 --`)  

### È possibile l'esfiltrazione di dati tramite tecniche in-band o out-of-band?
- [ ] No — nessuna esfiltrazione di dati identificata  
- [ ] Sì — l'esfiltrazione UNION-based o error-based **è possibile**  
- [ ] Sì — l'esfiltrazione blind (Boolean o Time-based) **è possibile**  
- [ ] Sì — l'esfiltrazione out-of-band (DNS/HTTP) **è possibile**  

### Le funzioni dell'applicazione presentano vulnerabilità di SQL injection di secondo ordine?
- [ ] No — i dati memorizzati sono gestiti in modo sicuro quando vengono recuperati per query successive  
- [ ] Sì — l'input dell'utente viene memorizzato e successivamente utilizzato in una query SQL non sicura **senza** validazione  

### I privilegi dell'account di servizio del database sono limitati al minimo necessario?
- [ ] Sì — l'account di servizio ha il **minimo privilegio** (es. limitato a tabelle/azioni specifiche)  
- [ ] No — l'account di servizio ha privilegi elevati (es. `DROP TABLE`, privilegi `FILE`, o `xp_cmdshell` **abilitato**)  

---