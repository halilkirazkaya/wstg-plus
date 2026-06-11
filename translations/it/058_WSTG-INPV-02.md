## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Lo Stored Cross Site Scripting (XSS), noto anche come XSS persistente, si verifica quando un'applicazione riceve dati da un utente e li memorizza in un database persistente, in un file system o in un altro supporto di memorizzazione senza un'adeguata validazione o codifica. Quando una vittima naviga successivamente in una pagina che recupera e visualizza questi dati non sanificati, lo script malevolo viene eseguito nel contesto del suo browser sotto l'origine dell'applicazione. Questa vulnerabilità è particolarmente pericolosa perché non richiede che la vittima clicchi su un link appositamente creato; qualsiasi utente che visualizzi la pagina interessata diventa un bersaglio, portando potenzialmente a session hijacking, azioni non autorizzate o credential harvesting. Gli attaccanti solitamente prendono di mira sezioni commenti, profili utente, bacheche e log amministrativi in cui l'input viene costantemente renderizzato per altri utenti o amministratori.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Strumenti:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Esistono punti di ingresso che memorizzano l'input dell'utente per una successiva visualizzazione ad altri utenti?
- [ ] No — l'applicazione non memorizza input controllabile dall'utente per un rendering successivo  
- [ ] Sì — l'input viene memorizzato (es. profili, commenti) ma **non** viene renderizzato in un contesto HTML  
- [ ] Sì — l'input viene memorizzato e renderizzato ad altri utenti o amministratori  

### Viene applicata la codifica dell'output o la sanificazione quando i dati memorizzati vengono renderizzati?
- [ ] Sì — la codifica dell'output context-aware **è applicata** e il bypass **non è possibile** *(Più sicuro)*  
- [ ] Sì — la sanificazione (es. `DOMPurify`) **è applicata** ma il bypass **è possibile** tramite errori di configurazione  
- [ ] No — i dati vengono renderizzati direttamente nel DOM **senza** codifica o sanificazione *(Alta)*  

### È possibile bypassare la validazione dell'input o la sanificazione utilizzando payload alternativi?
- [ ] No — i filtri gestiscono in modo sicuro varie codifiche, tag non standard e gestori di eventi (event handlers)  
- [ ] Sì — il bypass **è possibile** utilizzando la codifica HTML entity o tag specifici (es. `<svg>`, `<img>`)  
- [ ] Sì — il bypass **è possibile** tramite payload poliglotti o mutation-based XSS (mXSS)  

### Una Content Security Policy (CSP) fornisce un secondo livello di difesa?
- [ ] Sì — una CSP rigorosa **è abilitata** e impedisce l'esecuzione di script inline e da sorgenti non autorizzate  
- [ ] Sì — una CSP **è abilitata** ma è configurata in modo errato (es. `unsafe-inline` o `script-src` troppo ampio)  
- [ ] No — nessuna CSP **è abilitata** per l'applicazione  

### È possibile ottenere un "Stored XSS to RCE" o altre catene ad alto impatto (Blind XSS)?
- [ ] No — l'impatto è limitato al contesto del browser lato client  
- [ ] Sì — lo stored XSS **può** essere utilizzato per colpire gli amministratori (Blind XSS) nei pannelli interni  
- [ ] Sì — lo stored XSS **può** essere concatenato con altre vulnerabilità (es. CSRF, esfiltrazione di dati sensibili)  

---