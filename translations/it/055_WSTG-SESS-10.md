## WSTG-SESS-10 — Testing JSON Web Tokens

I JSON Web Tokens (JWT) sono comunemente utilizzati per la gestione delle sessioni stateless e la propagazione dell'identità, ma la loro sicurezza dipende interamente dalla corretta implementazione degli algoritmi di firma e da una rigorosa verifica dei claim. Gli attaccanti tentano spesso di bypassare l'autenticazione sfruttando le falle dell'algoritmo "none", effettuando il brute-forcing di chiavi segrete HS256 deboli o eseguendo attacchi di algorithm-switching, in cui una chiave pubblica asimmetrica viene utilizzata come segreto HMAC simmetrico. Queste vulnerabilità si manifestano tipicamente nei cookie di sessione o negli header di Authorization, permettendo potenzialmente a un attaccante di elevare i propri privilegi o impersonare qualsiasi utente modificando claim come `role` o `user_id` senza invalidare l'integrità crittografica del token.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Strumenti:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### L'algoritmo "none" è accettato dal server?
- [ ] No — il server **rifiuta** i token che utilizzano `alg: none` nell'header  
- [ ] Sì — il server **accetta** i token con `alg: none` e una firma vuota *(Critico)*  

### Come viene verificata la firma JWT e protetta contro il brute-force?
- [ ] Sì — vengono utilizzate chiavi asimmetriche forti (RS256/ES256) o segreti HMAC ad alta entropia  
- [ ] Sì — viene utilizzato HS256 ma il segreto **può** essere forzato tramite brute-force a causa della bassa entropia o di valori predefiniti  
- [ ] No — la verifica della firma **non è applicata** o **può** essere aggirata tramite algorithm switching (da RS256 a HS256)  

### I claim standard (exp, nbf, iat) sono applicati rigorosamente dal backend?
- [ ] Sì — `exp` (scadenza) è presente e **non può** essere aggirato  
- [ ] Sì — `exp` è presente ma il server **non lo applica** durante la validazione  
- [ ] No — i claim di scadenza sono **mancanti** o ignorati, consentendo il replay indefinito della sessione  

### L'implementazione previene la key injection basata su header (kid, jku, x5u)?
- [ ] Sì — gli header sono sanitizzati e sono **consentite** solo chiavi/fonti interne affidabili  
- [ ] No — l'header `kid` (Key ID) è vulnerabile a directory traversal o SQL injection per puntare a un file noto  
- [ ] No — gli header `jku` o `x5u` **possono** essere puntati verso URL controllati dall'attaccante per caricare chiavi malevole  

---