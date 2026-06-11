## WSTG-APIT-02 — API Broken Object Level Authorization

Il Broken Object Level Authorization (BOLA), noto anche come Insecure Direct Object Reference (IDOR), si verifica quando un'API non riesce a verificare se un utente disponga dei permessi appropriati per accedere o manipolare una specifica risorsa identificata da un ID. Gli attaccanti sfruttano questa vulnerabilità enumerando o indovinando sistematicamente gli identificatori — come ID numerici o UUID — nei percorsi delle richieste (request paths), nei parametri di query o nei corpi JSON per accedere a dati appartenenti ad altri utenti. Questa vulnerabilità è il problema più comune e impattante nella sicurezza delle API moderne, potendo portare a esfiltrazioni di dati di massa, modifiche non autorizzate o un completo account takeover. Dal punto di vista di un attaccante, l'obiettivo è identificare gli endpoint che accettano identificatori di oggetto e testare se la logica di autorizzazione lato server sia mancante o possa essere soggetta a bypass manipolando tali ID.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Strumenti:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Gli identificatori di oggetto sono prevedibili o enumerabili all'interno delle richieste API?
- [ ] No — gli identificatori sono lunghi, casuali e crittograficamente sicuri (es. UUIDv4)  
- [ ] Sì — gli identificatori sono interi sequenziali (es. `101`, `102`)  
- [ ] Sì — gli identificatori seguono un pattern prevedibile o derivano da informazioni pubbliche  

### L'API esegue la validazione lato server della proprietà dell'oggetto per ogni richiesta?
- [ ] Sì — i controlli di autorizzazione sono applicati a ogni richiesta e il bypass **non è possibile**  
- [ ] Sì — i controlli sono applicati ma il bypass **è possibile** tramite parameter pollution o method tunneling  
- [ ] No — l'applicazione si affida esclusivamente all'autenticazione dell'utente senza controllare la proprietà della risorsa specifica  

### È possibile accedere o modificare la risorsa di un altro utente cambiando l'identificatore?
- [ ] No — l'accesso non autorizzato alle risorse di altri utenti **non è possibile**  
- [ ] Sì — l'accesso in sola **lettura** non autorizzato (Horizontal BOLA) **è possibile**  
- [ ] Sì — la **modifica** o l'**eliminazione** non autorizzata (Horizontal BOLA) **è possibile**  

### L'API consente l'accesso a oggetti a livello amministrativo o di sistema sostituendo gli ID?
- [ ] No — le risorse amministrative sono protette da livelli di autorizzazione secondari  
- [ ] Sì — l'accesso o la modifica di risorse a livello di sistema (Vertical BOLA) **è possibile**  

### Il controllo di autorizzazione può essere aggirato spostando l'identificatore in una parte diversa della richiesta?
- [ ] No — la logica di autorizzazione è coerente indipendentemente dalla posizione del parametro  
- [ ] Sì — il bypass **è possibile** quando l'ID viene spostato dal percorso URL al corpo JSON o agli header  
- [ ] Sì — il bypass **è possibile** quando vengono forniti più ID e il server elabora quello non autorizzato  

---