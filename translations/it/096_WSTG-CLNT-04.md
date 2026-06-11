## WSTG-CLNT-04 — Testing for Client-Side URL Redirect

La redirezione URL lato client (Client-side URL redirection) si verifica quando un'applicazione web accetta input controllati dall'utente — tipicamente attraverso parametri URL o frammenti hash — e li utilizza all'interno di un sink di redirezione lato client come `window.location` o `document.location.href`. Questa vulnerabilità viene sfruttata principalmente dagli attaccanti per condurre campagne di phishing sofisticate, poiché la vittima inizialmente si fida del dominio legittimo prima di essere silenziosamente reindirizzata a un sito malevolo. Oltre al phishing, i redirect lato client possono spesso essere concatenati per esfiltrare dati sensibili come token di accesso OAuth o identificativi di sessione, qualora la redirezione avvenga mentre tali valori sono presenti nell'URL o nell'header `Referer`. Nelle moderne Single Page Applications (SPA), questa vulnerabilità appare frequentemente nella logica di routing, nei parametri "return to" dopo l'autenticazione o nelle funzionalità di cambio lingua.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media* |

> *La gravità diventa Alta se la redirezione può essere utilizzata per esfiltrare token OAuth, ID di sessione o bypassare controlli di sicurezza in un flusso di autenticazione.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Strumenti:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### L'applicazione utilizza script lato client per gestire le redirezioni basate sull'input dell'utente?
- [ ] No — la redirezione è gestita esclusivamente lato server tramite codici di stato HTTP `3xx`  
- [ ] Sì — l'applicazione utilizza sink JavaScript come `window.location` per la navigazione basata su parametri URL  
- [ ] Sì — l'applicazione utilizza un router del framework lato client (es. React Router, Vue Router) che elabora percorsi forniti dall'utente  

### Sono implementati controlli di validazione dell'input o whitelist per l'URL di destinazione?
- [ ] Sì — viene applicata una **whitelist** rigorosa di domini/percorsi consentiti e il bypass **non è possibile**  
- [ ] Sì — la validazione è presente ma si basa su regex deboli o blacklist, e il bypass **è possibile**  
- [ ] No — l'applicazione accetta qualsiasi stringa arbitraria come destinazione della redirezione  

### La logica di redirezione può essere bypassata utilizzando comuni tecniche di offuscamento?
- [ ] No — i controlli gestiscono correttamente i caratteri codificati, gli URL relativi al protocollo (`//`) e i backslash  
- [ ] Sì — il bypass **è possibile** utilizzando URL encoding o double-encoding  
- [ ] Sì — il bypass **è possibile** utilizzando URL relativi al protocollo o caratteri `@` (es. `https://expected.com@attacker.com`)  
- [ ] Sì — il bypass **è possibile** sfruttando specifiche peculiarità (quirks) del browser o null byte injection  

### Il processo di redirezione espone dati sensibili al sito di destinazione?
- [ ] No — nessun dato sensibile è presente nell'URL o trasmesso tramite header durante la redirezione  
- [ ] Sì — dati sensibili (es. token di sessione, chiavi API) **vengono esposti** tramite l'header `Referer` al sito esterno  
- [ ] Sì — i dati sensibili presenti nel frammento dell'URL (`#`) **sono accessibili** agli script del sito di destinazione  

### È possibile eseguire JavaScript tramite il sink di redirezione (DOM-based XSS)?
- [ ] No — il sink consente solo la navigazione verso i protocolli `http` o `https`  
- [ ] Sì — il sink di redirezione consente URI `javascript:` o `data:`, rendendo possibile il **DOM-based XSS**  

---