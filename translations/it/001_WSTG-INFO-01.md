## WSTG-INFO-01 — Condurre la Ricerca e il Riconoscimento tramite Motori di Ricerca per Perdite di Informazioni

La scoperta tramite motori di ricerca (Search engine discovery) consiste nell'utilizzare motori di ricerca pubblici, pagine in cache e servizi di indicizzazione per identificare informazioni che l'organizzazione target ha esposto involontariamente. Gli attaccanti utilizzano operatori di ricerca avanzati (Google Dorks) e servizi di indicizzazione di terze parti per scoprire file sensibili, directory, portali di login, messaggi di errore e metadati che non dovrebbero essere accessibili pubblicamente. Questa fase di riconoscimento (reconnaissance) è critica perché non richiede alcuna interazione diretta con il target, rendendola virtualmente non rilevabile pur rivelando potenzialmente punti di ingresso di alto valore come pannelli di amministrazione esposti, file di configurazione, dump di database e documentazione interna.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Strumenti:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### I file o le directory sensibili sono indicizzati dai motori di ricerca?
- [ ] No — nessun contenuto sensibile trovato nei risultati dei motori di ricerca  
- [ ] Sì — i file di configurazione, i backup o i documenti interni **sono** indicizzati *(Critico)*  

### Il Google Dorking rivela interfacce amministrative o di login esposte?
- [ ] No — i pannelli di amministrazione e le pagine di login **non sono** indicizzati  
- [ ] Sì — i pannelli di amministrazione **sono** indicizzati ma richiedono un'autenticazione a più fattori forte  
- [ ] Sì — i pannelli di amministrazione **sono** indicizzati e accessibili pubblicamente **senza** controlli sufficienti  

### Le versioni in cache delle pagine espongono dati sensibili che nel frattempo sono stati rimossi?
- [ ] No — nessun dato sensibile trovato negli snapshot in cache  
- [ ] Sì — le pagine in cache contengono credenziali, indirizzi IP interni o informazioni sensibili  

### Il file `robots.txt` sta involontariamente rivelando percorsi sensibili?
- [ ] No — `robots.txt` **non** rivela strutture di directory sensibili  
- [ ] Sì — `robots.txt` elenca percorsi sensibili che facilitano il riconoscimento dell'attaccante (reconnaissance)  
- [ ] Non esiste alcun file `robots.txt`  

### I servizi di terze parti (Shodan, Censys, Wayback Machine) rivelano esposizioni storiche o attuali?
- [ ] No — nessun risultato significativo dai servizi di indicizzazione di terze parti  
- [ ] Sì — gli snapshot storici o le scansioni dei servizi rivelano metadati sensibili o banner dei servizi  

---