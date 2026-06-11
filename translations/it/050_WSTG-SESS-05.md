## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Il Cross-Site Request Forgery (CSRF) è una vulnerabilità in cui un attaccante induce il browser della vittima a eseguire un'azione indesiderata su un sito web differente nel quale la vittima è attualmente autenticata. Questo exploit sfrutta il comportamento del browser di allegare automaticamente le credenziali "ambientali", come i cookie di sessione o gli Authorization headers, alle richieste in uscita. Gli attaccanti solitamente mirano a operazioni che modificano lo stato (state-changing operations), come il cambio della password, l'aggiornamento degli indirizzi email o l'esecuzione di trasferimenti finanziari, ospitando script malevoli o moduli nascosti su un sito di terze parti. Una corretta esecuzione dell'exploit può portare al completo account takeover o alla modifica non autorizzata dei dati senza la conoscenza o il consenso dell'utente.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Media / Alta* |

> *La severità diventa Alta se l'azione vulnerabile consente l'account takeover, la privilege escalation o transazioni finanziarie non autorizzate.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Strumenti:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### I token anti-CSRF sono implementati per le richieste che modificano lo stato?
- [ ] Sì — token unici e crittograficamente forti sono richiesti per tutte le azioni che modificano lo stato  
- [ ] Sì — i token sono presenti ma **non** sono unici per sessione o sono prevedibili  
- [ ] No — i token anti-CSRF **non** sono implementati  

### La validazione lato server del token anti-CSRF è robusta?
- [ ] Sì — il server valida rigorosamente il token e il bypass **non è possibile**  
- [ ] Sì — la validazione viene eseguita ma **può** essere bypassata rimuovendo il parametro del token  
- [ ] Sì — la validazione viene eseguita ma **può** essere bypassata fornendo un token vuoto o fittizio (dummy token)  
- [ ] Sì — la validazione viene eseguita ma il token **non** è legato alla sessione dell'utente  

### L'applicazione si affida a metodi facilmente aggirabili per la protezione CSRF?
- [ ] No — l'applicazione non si affida esclusivamente a header deboli o controlli dell'origine  
- [ ] Sì — la protezione si basa esclusivamente sull'header `Referer` o `Origin` che **può** essere falsificato (spoofed) o rimosso  
- [ ] Sì — la protezione si basa sul controllo dell'header `X-Requested-With` che **può** essere bypassato tramite configurazioni errate di CORS  

### I cookie di sessione sono configurati con l'attributo `SameSite`?
- [ ] Sì — `SameSite` è impostato su `Strict` o `Lax` su tutti i cookie relativi alla sessione  
- [ ] No — l'attributo `SameSite` è **mancante**, lasciando spazio al comportamento predefinito del browser  
- [ ] No — `SameSite` è esplicitamente impostato su `None` senza il flag `Secure` o ulteriori mitigazioni  

### La protezione CSRF può essere bypassata cambiando il metodo della richiesta HTTP?
- [ ] No — la protezione viene applicata indipendentemente dal metodo HTTP utilizzato  
- [ ] Sì — i token sono validati solo sulle richieste `POST`, ma l'azione **può** essere eseguita tramite `GET`  
- [ ] Sì — il cambio del metodo (es. in `PUT` o `DELETE`) bypassa il controllo del token  

---