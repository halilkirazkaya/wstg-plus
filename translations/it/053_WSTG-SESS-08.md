## WSTG-SESS-08 — Session Puzzling

Il Session Puzzling, noto anche come Session Variable Overloading, si verifica quando un'applicazione utilizza la stessa variabile di sessione per molteplici scopi in diversi moduli o non riesce a cancellare correttamente gli stati della sessione durante le transizioni del flusso di lavoro. Gli attaccanti sfruttano questo comportamento popolando le variabili di sessione in un contesto — come l'anteprima di un profilo non autenticato o una registrazione multi-step — per poi navigare in un'area protetta dove l'applicazione si fida erroneamente di quelle variabili preesistenti. Ciò può portare a impatti critici tra cui Authentication bypass, Privilege Escalation o accesso non autorizzato a dati sensibili, ingannando l'applicazione e facendole credere che una sessione si trovi in uno stato di privilegio superiore a quello reale. La vulnerabilità è più diffusa in applicazioni complesse e stateful dove la logica di gestione della sessione è applicata in modo incoerente tra i diversi componenti funzionali.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### L'applicazione utilizza gli stessi nomi di variabile di sessione per scopi diversi in moduli separati?
- [ ] No — i nomi delle variabili sono univoci o gli scope sono isolati  
- [ ] Sì — esistono nomi di variabili condivisi ma lo stato viene cancellato tra i moduli  
- [ ] Sì — esistono nomi di variabili condivisi e lo stato **non viene** cancellato  

### Le azioni non autenticate possono influenzare le variabili di sessione utilizzate in contesti autenticati?
- [ ] No — le variabili di sessione vengono inizializzate solo **dopo** un'autenticazione riuscita  
- [ ] Sì — le variabili di sessione possono essere impostate prima del login ma **non influiscono** sullo stato post-login  
- [ ] Sì — le variabili di sessione impostate durante la pre-autenticazione **sono ritenute affidabili** dopo l'autenticazione *(Critico)*  

### L'applicazione reinizializza l'identificatore di sessione e le relative variabili associate in caso di variazione del livello di privilegio?
- [ ] Sì — la sessione viene completamente resettata e **viene emesso** un nuovo ID  
- [ ] Parziale — viene emesso un nuovo ID ma alcune variabili di sessione **persistono**  
- [ ] No — l'ID di sessione e tutte le variabili **rimangono** invariati  

### È possibile ottenere privilegi amministrativi manipolando le variabili di sessione tramite un account non privilegiato?
- [ ] No — le variabili di privilegio **non possono** essere modificate dagli utenti  
- [ ] Sì — le variabili possono essere modificate ma l'applicazione le **valida** rispetto al database di backend  
- [ ] Sì — le variabili possono essere modificate e l'applicazione **si fida** implicitamente dello stato della sessione *(Critico)*  

### Lo stato della sessione viene cancellato immediatamente al completamento di uno specifico flusso di lavoro (es. reset della password, checkout)?
- [ ] Sì — le variabili di sessione per il flusso di lavoro vengono distrutte al completamento  
- [ ] No — le variabili del flusso di lavoro **persistono** e possono essere riutilizzate in altre aree dell'applicazione  

---