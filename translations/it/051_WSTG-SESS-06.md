## WSTG-SESS-06 — Testing for Logout Functionality

La funzionalità di logout è un controllo di sicurezza critico progettato per terminare la sessione di un utente e invalidare gli identificatori di sessione associati sia sul lato client che sul lato server. La mancata implementazione corretta del logout consente attacchi di Session Fixation o Hijacking, in quanto un attaccante che ottiene l'accesso a una macchina dopo che un utente ha effettuato il "logout" potrebbe potenzialmente riutilizzare il session token ancora attivo. I pentester valutano questo aspetto verificando se il session cookie viene rimosso dal browser, se lo stato della sessione lato server viene esplicitamente distrutto e se la navigazione tramite il pulsante "Back" del browser consente l'accesso a dati sensibili memorizzati nella cache. Un'implementazione sicura del logout garantisce che, una volta attivata l'azione, nessuna richiesta successiva effettuata con il vecchio session token venga autorizzata dall'applicazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Strumenti:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### È presente e accessibile un meccanismo di logout funzionante?
- [ ] Sì — il pulsante di logout esiste e attiva una richiesta di terminazione  
- [ ] No — il pulsante di logout è **mancante** o **non** attiva un evento di terminazione  

### L'identificativo di sessione viene invalidato lato server?
- [ ] Sì — il server rifiuta tutte le richieste successive che utilizzano il vecchio session token  
- [ ] No — il server **continua ad accettare** il vecchio session token dopo il logout  

### L'applicazione rimuove i cookie di sessione nel browser al momento del logout?
- [ ] Sì — i cookie vengono sovrascritti con una data di scadenza passata o un valore vuoto  
- [ ] Sì — i cookie rimangono ma **non sono più validi** sul server  
- [ ] No — i cookie rimangono nel browser e **mantengono** i loro valori originali  

### È possibile accedere a contenuti autenticati sensibili tramite il pulsante "Back" del browser dopo il logout?
- [ ] No — gli header `Cache-Control: no-store` o simili impediscono la visualizzazione di pagine sensibili post-logout  
- [ ] Sì — le pagine sensibili sono memorizzate nella cache e **possono** essere visualizzate tramite la navigazione dopo che la sessione è stata terminata  

---