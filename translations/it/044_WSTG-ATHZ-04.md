## WSTG-ATHZ-04 — Testing for Insecure Direct Object References

Le Insecure Direct Object References (IDOR) si verificano quando un'applicazione fornisce l'accesso diretto agli oggetti in base all'input fornito dall'utente senza eseguire un controllo di autorizzazione per verificare i permessi del richiedente. Questa vulnerabilità ha un impatto significativo sulla riservatezza e sull'integrità, poiché consente agli aggressori di visualizzare, modificare o eliminare dati appartenenti ad altri utenti o al sistema. Si manifesta tipicamente nei parametri URL, nei dati del corpo POST o nelle chiavi JSON che si riferiscono a chiavi interne del database, nomi di file o identificatori di account. Dal punto di vista di un aggressore, l'exploitation comporta la manipolazione di questi identificatori — spesso attraverso l'incremento di numeri interi o il fuzzing di UUID — per accedere a risorse al di fuori del proprio ambito autorizzato.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Strumenti:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### L'applicazione utilizza identificatori diretti di oggetti nei parametri della richiesta?
- [ ] No — l'applicazione utilizza riferimenti indiretti o mappe crittografate *(Più sicuro)*  
- [ ] Sì — l'applicazione utilizza identificatori non prevedibili (es. UUID lunghi/hash)  
- [ ] Sì — l'applicazione utilizza identificatori prevedibili (es. interi incrementali o nomi utente)  

### Viene eseguita l'autorizzazione lato server per ogni richiesta che coinvolge un identificatore di oggetto?
- [ ] Sì — i controlli lato server **non possono** essere bypassati per alcun identificatore  
- [ ] Sì — i controlli lato server sono presenti ma il bypass **è possibile** tramite parameter pollution o manipolazione degli header  
- [ ] No — **non viene applicato** alcun controllo di autorizzazione per verificare la proprietà dell'oggetto richiesto  

### Un aggressore può accedere o modificare oggetti appartenenti ad altri utenti (Horizontal IDOR)?
- [ ] No — l'accesso ai dati di altri utenti **non è possibile**  
- [ ] Sì — la visualizzazione dei dati di altri utenti **è possibile** ma la modifica **non è possibile**  
- [ ] Sì — sia la visualizzazione che la modifica dei dati di altri utenti **sono possibili** *(Critico)*  

### È possibile accedere a oggetti di livello amministrativo o di sistema (Vertical IDOR)?
- [ ] No — l'accesso a oggetti con privilegi superiori **non è possibile**  
- [ ] Sì — l'accesso a configurazioni amministrative o file di sistema **è possibile**  

### L'applicazione espone identificatori di oggetti tramite ricerca, metadati o altri endpoint?
- [ ] No — gli identificatori **non sono esposti** involontariamente  
- [ ] Sì — gli identificatori di altri utenti/oggetti **possono** essere enumerati tramite endpoint secondari o profili pubblici  

---