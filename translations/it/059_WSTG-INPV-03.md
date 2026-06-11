## WSTG-INPV-03 — Testing for HTTP Verb Tampering

L'HTTP Verb Tampering sfrutta le vulnerabilità nel modo in cui i server web e i framework delle applicazioni autorizzano l'accesso a risorse specifiche basandosi sul metodo HTTP utilizzato. Gli attaccanti tentano di bypassare i vincoli di sicurezza sostituendo i metodi standard come `GET` o `POST` con alternative quali `HEAD`, `PUT`, `OPTIONS` o persino stringhe arbitrarie e non standard che il backend potrebbe elaborare in modo incoerente. Questa vulnerabilità si verifica tipicamente quando le configurazioni di sicurezza — come i filtri web.xml di Java EE o le regole di autorizzazione .NET — elencano esplicitamente i metodi consentiti ma non tengono conto degli altri, oppure quando viene utilizzato un approccio "deny-by-method" invece di "deny-all". Un exploit riuscito può comportare l'accesso non autorizzato a funzioni amministrative, la modifica dei dati o la divulgazione di informazioni relative alla configurazione interna del server.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se è possibile eseguire funzioni amministrative o modifiche dei dati (PUT/DELETE) senza autenticazione.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### L'applicazione risponde a metodi HTTP non standard o arbitrari?
- [ ] No — l'applicazione rifiuta tutti i metodi tranne quelli esplicitamente richiesti  
- [ ] Sì — l'applicazione risponde ai metodi standard (OPTIONS, TRACE) ma **non è possibile** bypassare la sicurezza  
- [ ] Sì — l'applicazione accetta stringhe arbitrarie (es. FOO, TEST) e le elabora come richieste `GET` o `POST`  

### È possibile bypassare l'autenticazione/autorizzazione cambiando il metodo HTTP?
- [ ] No — i controlli di sicurezza sono applicati globalmente indipendentemente dal verbo HTTP  
- [ ] Sì — i controlli sono presenti ma il bypass **è possibile** utilizzando il metodo `HEAD`  
- [ ] Sì — i controlli di sicurezza **non sono applicati** ai metodi alternativi, consentendo l'accesso non autorizzato a endpoint riservati  

### I metodi pericolosi come `PUT` o `DELETE` sono abilitati sul server?
- [ ] No — i metodi pericolosi sono **disabilitati** o restituiscono un errore 405 Method Not Allowed  
- [ ] Sì — i metodi sono abilitati ma richiedono un'autenticazione valida con privilegi elevati  
- [ ] Sì — i metodi sono **abilitati** e consentono il caricamento non autorizzato di file o l'eliminazione di risorse *(Critico)*  

### Il metodo `OPTIONS` rivela informazioni sensibili sui verbi consentiti?
- [ ] No — `OPTIONS` è **disabilitato** o restituisce una risposta generica  
- [ ] Sì — `OPTIONS` restituisce l'header `Allow` ma non espone metodi interni riservati  
- [ ] Sì — `OPTIONS` rivela metodi interni o amministrativi che facilitano ulteriori exploit  

### Il metodo `TRACE` è abilitato, consentendo potenzialmente il Cross-Site Tracing (XST)?
- [ ] No — i metodi `TRACE` e `TRACK` sono **disabilitati**  
- [ ] Sì — `TRACE` è **abilitato** e riflette gli header HTTP, inclusi cookie o token sensibili  

---