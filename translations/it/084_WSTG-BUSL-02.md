## WSTG-BUSL-02 — Test Ability to Forge Requests

La falsificazione delle richieste (Forging requests) all'interno di un contesto di business logic comporta la manipolazione di parametri definiti dall'applicazione per eseguire azioni al di fuori del workflow previsto o del livello di autorizzazione assegnato. Gli attaccanti prendono di mira processi multi-fase, come i sistemi di checkout o la registrazione di account, in cui lo stato viene mantenuto tramite parametri lato client che il server non riesce a ri-validare rispetto a una sorgente backend fidata. Alterando campi nascosti, valori JSON o parametri REST come prezzi, quantità o flag di stato interni, un attaccante può eludere i requisiti di pagamento, ottenere un'escalation dei privilegi o innescare cambiamenti di stato non intenzionali. Questa vulnerabilità si manifesta tipicamente in form web stateful o API in cui il server confida implicitamente nella rappresentazione dello stato di business fornita dal client durante una transazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica* |

> *La gravità diventa Critica se la richiesta falsificata consente perdite finanziarie, azioni amministrative non autorizzate o il completo account takeover.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### L'applicazione convalida i parametri transazionali rispetto a una "sorgente di verità" lato server?
- [ ] Sì — tutti i parametri sensibili (prezzo, ruolo, ID) sono validati rispetto al database e il bypass **non è possibile**  
- [ ] Sì — la validazione è applicata ma può essere elusa tramite parameter pollution o encoding alternativi  
- [ ] No — l'applicazione si fida dei valori lato client per determinare il costo o l'esito di una transazione *(Critico)*  

### I valori critici per il business (es. quantità, importi) possono essere modificati in valori non autorizzati?
- [ ] No — valori negativi, valori zero o valori eccessivamente alti vengono rifiutati dal server  
- [ ] Sì — il server accetta valori modificati ma solo entro un intervallo limitato  
- [ ] Sì — valori negativi o zero **possono** essere inviati, portando a bypass finanziari o di logica  

### Vengono utilizzati campi nascosti o parametri API per mantenere lo stato attraverso processi multi-fase?
- [ ] No — lo stato è mantenuto in modo sicuro tramite sessioni lato server o token firmati  
- [ ] Sì — lo stato viene passato tramite il client ma i controlli di integrità (HMAC/Firme) sono **abilitati** e robusti  
- [ ] Sì — lo stato viene passato tramite il client e i controlli di integrità sono **disabilitati** o possono essere rimossi  

### È possibile falsificare richieste che saltano passaggi intermedi obbligatori in un workflow?
- [ ] No — il server impone una sequenza rigorosa di operazioni per ogni transazione  
- [ ] Sì — i passaggi **possono** essere saltati ma l'azione finale richiede comunque dati validi dai passaggi precedenti  
- [ ] Sì — la richiesta finale di "invio" o "completamento" **può** essere falsificata direttamente per eludere le fasi di autorizzazione o pagamento  

---