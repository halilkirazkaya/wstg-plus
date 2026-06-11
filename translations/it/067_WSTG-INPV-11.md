## WSTG-INPV-11 — Code Injection

La Code Injection si verifica quando un'applicazione valuta frammenti di codice forniti dall'utente all'interno del proprio ambiente di runtime, tipicamente attraverso funzioni specifiche del linguaggio non sicure come `eval()`, `exec()` o `system()`. Questa vulnerabilità differisce dalla Command Injection poiché colpisce il contesto di esecuzione del linguaggio di programmazione (es. PHP, Python, Node.js) anziché la shell del sistema operativo sottostante. Gli aggressori sfruttano questi punti per eseguire logica arbitraria, esfiltrare variabili d'ambiente o ottenere una Remote Code Execution (RCE) completa sul server. I pentester dovrebbero analizzare le funzionalità che elaborano espressioni matematiche, template lato server o parametri di configurazione dinamici dove l'input potrebbe essere interpretato come codice eseguibile.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Le funzionalità dell'applicazione valutano l'input dinamico tramite funzioni di valutazione specifiche del linguaggio?
- [ ] No — le funzioni di valutazione dinamica **non sono** presenti nel codice sorgente  
- [ ] Sì — vengono utilizzate funzioni come `eval()`, `exec()` o `include()` ma **non** accettano input dall'utente  
- [ ] Sì — le funzioni vengono utilizzate e processano input fornito dall'utente  

### Vengono applicati meccanismi di validazione o sanificazione dell'input prima della valutazione?
- [ ] Sì — l'input è rigorosamente validato rispetto a una **whitelist predefinita**  
- [ ] Sì — l'input è sanificato tramite blacklisting, ma il bypass **è possibile**  
- [ ] No — l'input viene passato direttamente alla funzione di valutazione e **non sono applicati controlli**  

### L'ambiente di esecuzione è protetto da sandbox o limitato per impedire l'accesso a livello di sistema?
- [ ] Sì — l'esecuzione avviene in una sandbox altamente restrittiva dove le chiamate di sistema **non possono** essere effettuate  
- [ ] Sì — esistono alcune restrizioni, ma l'evasione dalla sandbox (sandbox escape) **è possibile**  
- [ ] No — l'ambiente consente il pieno accesso alle API del linguaggio sottostante e alle funzioni del sistema operativo *(Critica)*  

### La Code Injection può essere utilizzata per esfiltrare informazioni sensibili?
- [ ] No — l'esecuzione è "blind" e **non è possibile** alcuna comunicazione out-of-band  
- [ ] Sì — le variabili d'ambiente o i file locali **possono** essere esfiltrati tramite richieste HTTP/DNS  
- [ ] Sì — i risultati del codice eseguito sono **riflessi direttamente** nella risposta dell'applicazione  

### L'applicazione consente l'inclusione di file remoti o il caricamento dinamico di librerie?
- [ ] No — possono essere eseguiti solo percorsi di codice locali e predefiniti  
- [ ] Sì — l'applicazione **può** essere forzata a caricare ed eseguire codice da una sorgente remota o controllata dall'aggressore  

---