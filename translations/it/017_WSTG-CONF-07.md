## WSTG-CONF-07 — Test HTTP Strict Transport Security

L'HTTP Strict Transport Security (HSTS) è un meccanismo di sicurezza che consente a un server web di informare i browser che l'accesso deve avvenire esclusivamente tramite HTTPS, prevenendo attacchi di protocol downgrade e il cookie hijacking. Imponendo una connessione crittografata, l'HSTS mitiga gli attacchi Man-in-the-Middle (MitM) come lo SSLStrip, in cui un attaccante intercetta una richiesta HTTP iniziale non sicura per impedire il passaggio a TLS. Dal punto di vista di un attaccante, l'assenza di questo header o una durata breve del `max-age` fornisce una finestra di opportunità per intercettare session token sensibili o credenziali durante l'handshake iniziale in chiaro. Questo test si concentra sulla verifica della presenza, della durata e dell'ambito dell'header `Strict-Transport-Security` su tutti gli endpoint sensibili dell'applicazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media* |

> *La gravità diventa Media se l'applicazione gestisce PII, session token o credenziali, poiché la mancanza di HSTS aumenta la percentuale di successo degli attacchi MitM.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### L'header `Strict-Transport-Security` è presente nelle risposte HTTPS?
- [ ] Sì — l'header **è presente** su tutti gli endpoint sensibili  
- [ ] Sì — l'header **è presente** ma solo sulla landing page  
- [ ] No — l'header **è assente** nell'intera applicazione  

### La direttiva `max-age` è impostata su una durata sufficiente?
- [ ] Sì — il `max-age` è impostato a un anno o più (es. `31536000`)  
- [ ] Sì — il `max-age` è impostato ma è **troppo breve** (es. meno di 6 mesi)  
- [ ] No — la direttiva `max-age` **è assente** o impostata a `0` *(Critico)*  

### La policy HSTS copre tutti i sottodomini?
- [ ] Sì — la direttiva `includeSubDomains` **è applicata**  
- [ ] No — la direttiva `includeSubDomains` **è assente**, lasciando i sottodomini vulnerabili al downgrade  
- [ ] No — la funzionalità non è applicabile in quanto non esistono sottodomini  

### L'applicazione è registrata per l'HSTS Preloading?
- [ ] Sì — la direttiva `preload` **è presente** e il dominio è nella lista di preload HSTS  
- [ ] Sì — la direttiva `preload` **è presente** ma il dominio **non** è ancora nella lista di preload  
- [ ] No — la direttiva `preload` **è assente**, consentendo una finestra per attacchi MitM alla prima visita  

### L'header HSTS viene inviato erroneamente su HTTP in chiaro?
- [ ] No — l'header viene inviato **solo** su connessioni HTTPS sicure  
- [ ] Sì — l'header **viene inviato** su HTTP, il che viene ignorato dai browser e potrebbe esporre dettagli di configurazione  

---