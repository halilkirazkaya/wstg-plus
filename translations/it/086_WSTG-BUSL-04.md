## WSTG-BUSL-04 — Test for Process Timing

Gli attacchi di process timing prevedono la misurazione del tempo impiegato da un'applicazione per elaborare specifiche richieste al fine di inferire informazioni sensibili sugli stati interni o sull'esistenza di dati. Analizzando discrepanze a livello di millisecondi nei tempi di risposta — spesso causate da logiche condizionali con uscita anticipata (early-exit), ricerche nel database o confronti crittografici non a tempo costante — gli attaccanti possono eseguire un'analisi side-channel per enumerare username validi, verificare frammenti di password o confermare l'esistenza di record specifici. Queste vulnerabilità si verificano tipicamente negli endpoint di autenticazione, nei moduli di reset della password e nei filtri API ad alta intensità di risorse dove la logica di backend si dirama in base alla validità dell'input. Dal punto di vista di un attaccante, queste differenze temporali fungono da "boolean oracle" che rivela verità sui dati di backend anche quando l'applicazione restituisce messaggi di errore generici e identici all'utente.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Strumenti:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### L'applicazione presenta tempi di risposta variabili per richieste di risorse valide rispetto a quelle non valide?
- [ ] No — i tempi di risposta sono identici indipendentemente dalla validità dell'input  
- [ ] Sì — esistono differenze di temporizzazione trascurabili ma **non** sono statisticamente significative  
- [ ] Sì — differenze di temporizzazione coerenti sono **osservabili** e forniscono un oracolo affidabile  

### Sono implementate funzioni di confronto a tempo costante o ritardi artificiali per normalizzare i tempi di risposta?
- [ ] Sì — algoritmi a tempo costante sono **applicati** a tutte le operazioni di confronto sensibili *(Più sicuro)*  
- [ ] Sì — ritardi artificiali o jitter sono **abilitati**, ma la logica sottostante rimane non a tempo costante  
- [ ] No — non è **applicata** alcuna mitigazione della temporizzazione, consentendo la misurazione diretta dei rami logici  

### Un attaccante può utilizzare l'analisi statistica per isolare il segnale di temporizzazione dal rumore di rete?
- [ ] No — il jitter di rete e il rumore dell'applicazione rendono l'analisi della temporizzazione **non possibile**  
- [ ] Sì — con una dimensione del campione sufficiente, il segnale di temporizzazione **può** essere isolato dal rumore  
- [ ] Sì — il delta di temporizzazione è sufficientemente grande che la normalizzazione statistica **non** è richiesta  

### L'account enumeration è possibile tramite le funzionalità di login o di reset della password?
- [ ] No — l'esistenza dell'account non può essere determinata tramite la temporizzazione  
- [ ] Sì — le discrepanze di temporizzazione consentono a un attaccante remoto di effettuare l'**enumeration** di username o email validi  

### Le operazioni ad alta intensità di risorse (es. elaborazione di file, ricerche complesse) espongono l'esistenza di dati tramite la temporizzazione?
- [ ] No — il consumo di risorse è uniforme indipendentemente dal fatto che un record venga trovato o meno  
- [ ] Sì — l'esistenza di record sensibili **può** essere inferita misurando la durata dell'elaborazione backend  

---