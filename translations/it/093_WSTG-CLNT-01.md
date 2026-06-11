## WSTG-CLNT-01 — Testing for DOM Based Cross Site Scripting

Il Cross-Site Scripting basato su DOM (DOM XSS) si verifica quando un'applicazione contiene JavaScript lato client che elabora dati provenienti da una sorgente non attendibile in modo non sicuro, solitamente scrivendo i dati nel Document Object Model (DOM) tramite un sink pericoloso. A differenza del Cross-Site Scripting (XSS) riflesso o memorizzato, la vulnerabilità risiede interamente nel codice lato client; il Payload spesso non viene affatto inviato al server, poiché viene elaborato da sink come `innerHTML`, `eval()` o `document.write()`. Gli attaccanti sfruttano questa vulnerabilità creando URL con frammenti o parametri malevoli che, quando caricati dal browser della vittima, eseguono JavaScript arbitrario nel contesto della sessione dell'utente. Ciò può portare all'esfiltrazione di token di sessione, al furto di dati sensibili e ad azioni non autorizzate eseguite per conto dell'utente autenticato.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Strumenti:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Vengono utilizzate sorgenti non attendibili per catturare dati controllati dall'utente in JavaScript?
- [ ] No — JavaScript **non** utilizza `location.hash`, `location.search` o `document.referrer`  
- [ ] Sì — vengono utilizzate sorgenti non attendibili ma i dati **non** vengono passati ad alcun sink di esecuzione  
- [ ] Sì — vengono utilizzate sorgenti non attendibili e i dati fluiscono direttamente nella logica lato client  

### I dati controllati dall'utente vengono passati a sink DOM pericolosi?
- [ ] No — sink come `innerHTML`, `outerHTML`, `document.write()` o `eval()` **non** vengono utilizzati  
- [ ] Sì — sono presenti sink pericolosi ma i dati vengono rigorosamente **sanificati** utilizzando una libreria sicura come DOMPurify  
- [ ] Sì — sono presenti sink pericolosi e i dati vengono elaborati con un filtraggio **insufficiente** o basato su regex personalizzate  
- [ ] Sì — sono presenti sink pericolosi e i dati vengono passati **senza** alcuna sanificazione o codifica *(Critico)*  

### Il sink di esecuzione può essere raggiunto tramite un payload basato su URL?
- [ ] No — il flusso da sorgente a sink (source-to-sink) **non** può essere attivato tramite input esterno  
- [ ] Sì — il flusso è **possibile** ma richiede un'interazione complessa dell'utente  
- [ ] Sì — il flusso è **possibile** e può essere attivato tramite un semplice frammento URL o parametro di query  

### Gli header di sicurezza moderni o le mitigazioni basate sul browser stanno neutralizzando il rischio?
- [ ] Sì — una `Content-Security-Policy` (CSP) rigorosa con `script-src` e `trusted-types` è **abilitata** e impedisce l'esecuzione  
- [ ] Sì — è presente una CSP ma `unsafe-inline` o hash deboli rendono **possibile** un bypass  
- [ ] No — non è presente alcuna CSP per mitigare l'esecuzione basata su DOM  

### L'applicazione utilizza framework lato client con protezioni integrate?
- [ ] Sì — il framework (ad es. Angular, React) codifica automaticamente i dati e il bypass **non è possibile**  
- [ ] Sì — il framework viene utilizzato ma sono abilitati "escape hatches" come `dangerouslySetInnerHTML`  
- [ ] No — l'applicazione utilizza JavaScript puro (vanilla JavaScript) o librerie legacy (ad es. vecchie versioni di jQuery) senza auto-escaping  

---