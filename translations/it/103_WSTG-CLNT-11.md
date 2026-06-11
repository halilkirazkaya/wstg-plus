## WSTG-CLNT-11 — Test Web Messaging

Il Web Messaging, o `postMessage`, consente la comunicazione cross-origin tra oggetti window, come una pagina genitore e un popup generato o un iframe incorporato. Questo meccanismo è fondamentale per le moderne funzionalità web, ma introduce rischi significativi se l'applicazione ricevente non riesce a verificare l'identità del mittente o gestisce in modo improprio il payload del messaggio. Gli aggressori sfruttano queste vulnerabilità ospitando un sito dannoso che incorpora l'applicazione target e invia messaggi appositamente creati per innescare azioni indesiderate, esfiltrare dati sensibili o eseguire Cross-Site Scripting (XSS) basato su DOM. Uno sfruttamento (Exploit) riuscito si verifica quando i listener dei messaggi non convalidano rigorosamente la proprietà `origin` o passano `message.data` direttamente in sink di esecuzione pericolosi.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Medio / Alto |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Strumenti:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### L'applicazione utilizza l'API `postMessage` per la comunicazione tra documenti?
- [ ] No — i listener `postMessage` **non** sono presenti nel codice lato client  
- [ ] Sì — `postMessage` è utilizzato per la comunicazione tra componenti interni  
- [ ] Sì — `postMessage` è utilizzato per comunicare con domini o widget di terze parti  

### L'origine dei messaggi in arrivo è validata rigorosamente?
- [ ] Sì — l'origine viene verificata rispetto a una whitelist rigorosa utilizzando `===` o un confronto identico *(Più sicuro)*  
- [ ] Sì — l'origine è validata tramite una regex, ma la logica **è** suscettibile di bypass (es. `^https://trusted.com`)  
- [ ] No — la proprietà `origin` **non** viene controllata, consentendo a qualsiasi dominio di inviare messaggi  
- [ ] No — viene utilizzato un wildcard `*` nel `targetOrigin` del mittente, rischiando potenzialmente la fuga di dati verso listener dannosi  

### Il payload del messaggio viene sanificato prima di essere elaborato dall'applicazione?
- [ ] Sì — i dati sono trattati come testo semplice e non vengono utilizzati sink pericolosi  
- [ ] Sì — i dati vengono analizzati (es. `JSON.parse`) e validati prima dell'uso, e il bypass **non è possibile**  
- [ ] No — i dati del messaggio vengono passati direttamente in sink pericolosi come `innerHTML`, `eval()` o `setTimeout()`  
- [ ] No — i dati del messaggio vengono utilizzati per navigare nella finestra o modificare `location.href` senza convalida  

### Un attaccante può innescare azioni non autorizzate o esfiltrare dati tramite un messaggio dannoso?
- [ ] No — anche con un messaggio contraffatto, non è possibile accedere ad azioni o dati sensibili  
- [ ] Sì — un messaggio appositamente creato **può** innescare modifiche di stato sensibili (es. cambio password, aggiornamento profilo)  
- [ ] Sì — un messaggio appositamente creato **può** causare XSS basato su DOM o l'esfiltrazione di token CSRF/dati di sessione  

---