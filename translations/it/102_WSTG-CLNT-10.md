## WSTG-CLNT-10 — Testing WebSockets

Il testing dei WebSocket si concentra sul canale di comunicazione full-duplex stabilito tra il client e il server, che elude i tradizionali pattern di richiesta-risposta HTTP. I pentester valutano la sicurezza del processo di handshake, la presenza di protezioni contro il Cross-Site WebSocket Hijacking (CSWSH) e il rigore della validazione dell'input applicata ai payload dei messaggi. L'exploitation in genere comporta l'hijacking di una sessione attiva per esfiltrare dati in tempo reale o l'iniezione di payload malevoli che innescano vulnerabilità server-side come Command Injection o SQLi attraverso la socket persistente. Poiché molti Web Application Firewall (WAF) tradizionali non ispezionano efficacemente il traffico WebSocket, questo protocollo fornisce spesso un vettore furtivo per bypassare le difese perimetrali.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Strumenti:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### La connessione WebSocket è stabilita su un canale sicuro?
- [ ] Sì — `wss://` (WebSocket Secure) è forzato per tutte le connessioni  
- [ ] No — viene utilizzato `ws://`, consentendo l'intercettazione tramite person-in-the-middle (PITM)  

### L'applicazione è protetta contro il Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] No — il server convalida rigorosamente l'header `Origin` durante l'handshake  
- [ ] Sì — la validazione dell'header `Origin` è debole o **può** essere bypassata tramite origin null o contraffatte (spoofed)  
- [ ] Sì — non viene eseguita alcuna validazione dell'header `Origin`, consentendo ad attaccanti cross-site di avviare una connessione *(Critico)*  

### La validazione e la sanificazione dell'input vengono applicate ai payload dei messaggi WebSocket?
- [ ] Sì — tutti i messaggi in entrata sono sanificati e validati rispetto a uno schema  
- [ ] Sì — esiste una parziale validazione, ma il bypass **è possibile** utilizzando payload codificati o non standard  
- [ ] No — i messaggi vengono elaborati direttamente, consentendo injection (XSS, SQLi, RCE) o manipolazione della logica  

### Vengono eseguiti controlli di autenticazione e autorizzazione su ogni messaggio WebSocket?
- [ ] Sì — la sessione viene verificata per ogni messaggio o il protocollo è stateless con token  
- [ ] Sì — l'autenticazione avviene durante l'handshake, ma l'autorizzazione per azioni specifiche **non** è applicata per singolo messaggio  
- [ ] No — l'autenticazione viene controllata solo all'handshake, consentendo al session hijacking di garantire l'accesso completo  

### Il server implementa rate limiting o restrizioni sulle risorse per il WebSocket?
- [ ] Sì — il rate limiting e le dimensioni massime dei messaggi sono **abilitati** e applicati  
- [ ] No — non esistono limiti, consentendo il Denial of Service (DoS) tramite flooding di messaggi o payload di grandi dimensioni  

---