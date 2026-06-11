## WSTG-ATHN-05 — Testing for Vulnerable Remember Password

La funzionalità "Remember Me" o di login persistente è progettata per mantenere lo stato autenticato di un utente attraverso i riavvii del browser, memorizzando un token o una credenziale sul lato client. Se questa funzionalità è implementata in modo errato — ad esempio memorizzando password in chiaro (plaintext), hash deboli o token a bassa entropia nei cookie — espone l'utente al furto dell'account (account takeover) tramite Cross-Site Scripting (XSS), accesso fisico o intercettazione di rete. I pentester valutano l'entropia di questi token di persistenza, i flag di sicurezza applicati al meccanismo di memorizzazione e il ciclo di vita della sessione per garantire che la comodità dell'accesso persistente non aggiri i controlli di sicurezza critici come la Multi-Factor Authentication (MFA) o l'invalidazione della sessione. L'Exploit spesso comporta la raccolta di questi token a lunga durata per ottenere un accesso non autorizzato agli account senza richiedere le credenziali originali.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se le credenziali in chiaro (plaintext) o gli hash senza salt (unsalted) vengono memorizzati nel cookie persistente, o se il token permette di bypassare la Multi-Factor Authentication (MFA).

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Strumenti:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### L'applicazione implementa una funzionalità "Remember Me" o "Resta collegato"?
- [ ] No — la funzionalità **non** è presente nell'applicazione  
- [ ] Sì — la funzionalità è presente e utilizza token crittograficamente forti e non prevedibili  
- [ ] Sì — la funzionalità è presente ma utilizza token prevedibili o a bassa entropia *(Media)*  
- [ ] Sì — la funzionalità è presente e memorizza credenziali in chiaro (plaintext) o password codificate in base64 *(Alta)*  

### I flag di sicurezza sono applicati correttamente al cookie persistente?
- [ ] Sì — i flag `HttpOnly`, `Secure` e `SameSite` sono **applicati**  
- [ ] Sì — alcuni flag sono applicati ma `HttpOnly` è **mancante**  
- [ ] Sì — alcuni flag sono applicati ma `Secure` è **mancante** su HTTPS  
- [ ] No — nessun flag di sicurezza è **applicato** al cookie persistente  

### Il token "Remember Me" rimane valido dopo un logout manuale?
- [ ] No — il token viene invalidato lato server immediatamente dopo il logout  
- [ ] Sì — il token rimane valido ma richiede una password per azioni sensibili  
- [ ] Sì — il token rimane valido e consente l'accesso completo all'account **senza** nuova autenticazione *(Media)*  

### La sessione persistente viene terminata in seguito a un cambio password?
- [ ] Sì — tutti i token persistenti vengono revocati quando l'utente cambia la propria password  
- [ ] No — il token "Remember Me" rimane valido dopo che un reset o un cambio password **è stato eseguito** *(Alta)*  

### Il token persistente aggira la Multi-Factor Authentication (MFA) nelle visite successive?
- [ ] No — la MFA è richiesta anche quando è presente un token "Remember Me"  
- [ ] Sì — la MFA viene aggirata, ma il token è legato a uno specifico fingerprint del dispositivo  
- [ ] Sì — la MFA viene **completamente aggirata** utilizzando solo il cookie persistente da qualsiasi dispositivo *(Alta)*  

---