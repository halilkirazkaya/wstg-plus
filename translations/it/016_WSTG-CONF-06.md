## WSTG-CONF-06 — Test dei Metodi HTTP

Il test dei metodi HTTP consiste nell'identificare quali verbi sono supportati dal server web e dall'applicazione per garantire che venga esposta solo la funzionalità necessaria. Oltre ai protocolli standard `GET` e `POST`, i server potrebbero inavvertitamente abilitare metodi pericolosi come `PUT`, `DELETE` o `TRACE`, che possono consentire agli aggressori di caricare file dannosi, eliminare contenuti esistenti o esfiltrare cookie di sessione tramite Cross-Site Tracing (XST). I Pentester esaminano gli header di risposta come `Allow` o `Public` e tentano di aggirare le restrizioni utilizzando tecniche di method overriding o verb tampering per accedere a funzionalità non autorizzate. Questo test è fondamentale per il consolidamento (hardening) della superficie di attacco e per prevenire errori di configurazione che potrebbero portare alla compromissione del server o alla manipolazione non autorizzata dei dati.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media* |

> *La gravità diventa Alta se `PUT` o `DELETE` consentono la manipolazione non autorizzata di file o se `TRACE` facilita l'esfiltrazione dei token di sessione.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### I metodi HTTP pericolosi come `PUT` o `DELETE` sono disabilitati in tutta l'applicazione?
- [ ] Sì — solo i metodi sicuri (GET, POST, HEAD) **sono abilitati**  
- [ ] No — `PUT` o `DELETE` **sono abilitati** ma richiedono un'autenticazione valida  
- [ ] No — `PUT` o `DELETE` **sono abilitati** e accessibili senza autenticazione *(Critico)*  

### Il metodo `TRACE` è disabilitato per prevenire il Cross-Site Tracing (XST)?
- [ ] Sì — il metodo `TRACE` **è disabilitato** o restituisce un 405 Method Not Allowed  
- [ ] No — il metodo `TRACE` **è abilitato** ma non riflette header sensibili  
- [ ] No — il metodo `TRACE` **è abilitato** e riflette gli header `Cookie` o `Authorization` *(Media)*  

### L'HTTP Method Overriding (es. `X-HTTP-Method-Override`) è supportato dal server?
- [ ] No — il server **non** elabora gli header di method override  
- [ ] Sì — il server elabora gli header di override ma i controlli di accesso **sono applicati**  
- [ ] Sì — il server elabora gli header di override ed è possibile l'aggiramento (bypass) del controllo degli accessi  

### Il server risponde in modo sicuro a metodi HTTP arbitrari o malformati?
- [ ] Sì — il server restituisce un 405 Method Not Allowed o 501 Not Implemented  
- [ ] No — il server restituisce un 200 OK o 500 Error per metodi arbitrari come `JEFF` o `TEST`  
- [ ] No — l'uso di metodi arbitrari consente di aggirare i filtri di autenticazione o autorizzazione  

### Il metodo `OPTIONS` è limitato o configurato per ridurre al minimo la divulgazione di informazioni?
- [ ] Sì — `OPTIONS` è disabilitato o restituisce un header `Allow` minimo  
- [ ] No — `OPTIONS` rivela una vasta gamma di metodi abilitati, inclusi i verbi DAV o di debug  

---