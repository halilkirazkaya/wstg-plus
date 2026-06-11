## WSTG-INPV-06 — Test per LDAP Injection

L'LDAP Injection si verifica quando un'applicazione incorpora dati forniti dall'utente nei filtri LDAP (Lightweight Directory Access Protocol) senza una corretta sanitizzazione o l'uso di escaping, consentendo a un utente malintenzionato di manipolare la logica della query. Iniettando caratteri speciali come asterischi, parentesi e operatori logici, gli attaccanti possono modificare il filtro di ricerca per bypassare i meccanismi di autenticazione o esfiltrare informazioni sensibili dalla directory, inclusi nomi utente, appartenenze a gruppi e attributi organizzativi. Questa vulnerabilità si manifesta tipicamente in funzionalità come le ricerche nelle directory aziendali, i portali per i dipendenti o i sistemi di Single Sign-On (SSO) che si interfacciano con Active Directory o OpenLDAP. Dal punto di vista di un attaccante, lo sfruttamento riuscito spesso comporta tecniche blind per enumerare la struttura della directory o i valori degli attributi bit per bit quando l'output diretto della query viene soppresso dall'applicazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Strumenti:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### L'applicazione utilizza LDAP per l'autenticazione o per le ricerche nella directory?
- [ ] No — LDAP **non è utilizzato** nell'architettura dell'applicazione  
- [ ] Sì — LDAP è utilizzato e tutti gli input sono rigorosamente sanitizzati o parametrizzati  
- [ ] Sì — LDAP è utilizzato e l'input dell'utente viene concatenato nei filtri **senza** un corretto escaping  

### I metacaratteri LDAP sono correttamente sottoposti a escaping o filtrati nell'input dell'utente?
- [ ] Sì — i caratteri come `(`, `)`, `&`, `|`, `*` e `\` **non sono consentiti** o sono correttamente sottoposti a escaping  
- [ ] No — i metacaratteri **possono** essere iniettati per alterare la struttura del filtro LDAP  

### Il meccanismo di autenticazione può essere bypassato tramite injection?
- [ ] No — la logica di login **non è suscettibile** alla manipolazione della query  
- [ ] Sì — l'autenticazione **può** essere bypassata utilizzando l'injection di OR logici (`|`) o wildcard (`*`) nei campi username o password  

### È possibile l'esfiltrazione cieca (blind) dei dati dal servizio di directory?
- [ ] No — i risultati della ricerca sono limitati e non si osservano differenze temporali (timing) o booleane  
- [ ] Sì — gli attributi della directory **possono** essere enumerati bit per bit tramite tecniche di blind injection basate su booleani  
- [ ] Sì — i record completi della directory **sono** riflessi nella risposta dell'applicazione a causa della manipolazione della query  

### I componenti secondari dell'applicazione (es. mailer, SSO) sono vulnerabili all'injection di attributi basata su LDAP?
- [ ] No — gli attributi LDAP sono gestiti in modo sicuro in tutti i componenti  
- [ ] Sì — un attaccante **può** modificare i propri attributi (es. email, appartenenza ai gruppi) tramite injection per ottenere un'escalation dei privilegi o reindirizzare le comunicazioni  

---