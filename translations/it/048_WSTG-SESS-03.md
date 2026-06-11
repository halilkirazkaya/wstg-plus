## WSTG-SESS-03 — Session Fixation

La Session Fixation si verifica quando un'applicazione non riesce a invalidare o ruotare l'identificativo di sessione dopo che un utente si è autenticato con successo, consentendo a un attaccante di forzare un token di sessione noto su una vittima. Se l'applicazione preserva il Session ID di pre-autenticazione dopo il login, un attaccante può fornire a una vittima un link appositamente creato contenente un Session ID fisso e successivamente dirottare (hijack) la sessione autenticata. Questa vulnerabilità si manifesta tipicamente nei form di login o tramite identificativi di sessione accettati attraverso parametri URL e cookie. Dal punto di vista di un attaccante, l'Exploit comporta il "fissaggio" della sessione tramite un link malevolo o una vulnerabilità di Header Injection e l'attesa che la vittima fornisca le proprie credenziali, bypassando efficacemente la necessità di rubare un token attivo.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### L'identificativo di sessione cambia dopo l'autenticazione riuscita?
- [ ] Sì — un nuovo identificativo di sessione **viene emesso** e quello precedente viene invalidato *(Più sicuro)*  
- [ ] Sì — un nuovo identificativo di sessione **viene emesso** ma quello precedente **rimane valido**  
- [ ] No — l'identificativo di sessione **rimane lo stesso** prima e dopo l'autenticazione *(Critico)*  

### L'applicazione accetta identificativi di sessione forniti tramite parametri URL?
- [ ] No — gli ID di sessione sono gestiti **esclusivamente** tramite cookie  
- [ ] Sì — gli ID di sessione sono accettati nell'URL ma vengono **ignorati** o **ruotati** al momento del login  
- [ ] Sì — gli ID di sessione nell'URL vengono **accettati** e **mantenuti** dopo l'autenticazione  

### È possibile forzare nell'applicazione un Session ID definito dall'attaccante?
- [ ] No — l'applicazione **rifiuta** ID di sessione non validi o inesistenti forniti dall'utente  
- [ ] Sì — l'applicazione **accetta** e inizializza una sessione utilizzando un ID arbitrario fornito in un cookie  
- [ ] Sì — l'applicazione **accetta** e inizializza una sessione utilizzando un ID arbitrario fornito in un parametro URL  

### Le sessioni di pre-autenticazione vengono terminate correttamente?
- [ ] Sì — la sessione anonima viene **completamente distrutta** all'evento di login  
- [ ] No — la sessione anonima **non viene distrutta**, consentendo un potenziale harvesting delle sessioni  

### Il cookie di sessione è protetto contro l'injection lato client?
- [ ] Sì — i flag `HttpOnly` e `Secure` sono **applicati** per prevenire la fixation basata su script  
- [ ] No — i flag **non sono applicati**, consentendo l'impostazione degli Session ID tramite Cross-Site Scripting (XSS)  

---