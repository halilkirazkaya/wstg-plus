## WSTG-SESS-11 — Testing for Concurrent Sessions

Il test per le sessioni concorrenti valuta se un'applicazione permette a un singolo account utente di mantenere più sessioni attive simultanee attraverso diversi browser, dispositivi o indirizzi IP. La mancata limitazione delle sessioni concorrenti aumenta la finestra di opportunità per gli attaccanti di utilizzare session token rubati o credenziali compromesse senza allertare l'utente legittimo o terminare le sessioni esistenti. Questo comportamento viene tipicamente valutato autenticandosi da più sorgenti e monitorando la stabilità della sessione, rivelando spesso debolezze nella logica di gestione delle sessioni o una mancanza di regole di business focalizzate sulla sicurezza. Dal punto di vista di un attaccante, l'assenza di controllo sulla concorrenza permette una persistenza furtiva dopo una compromissione iniziale e facilita attacchi automatizzati paralleli.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Le sessioni concorrenti sono limitate dalla policy dell'applicazione?
- [ ] Sì — è permessa solo una sessione per utente; i nuovi login invalidano quelli vecchi *(Più sicuro)*  
- [ ] Sì — viene imposto un numero fisso di sessioni concorrenti che non può essere superato  
- [ ] No — è possibile un numero illimitato di sessioni concorrenti  

### Come risponde l'applicazione quando viene raggiunto il limite di sessioni?
- [ ] La sessione attiva più vecchia **viene terminata automaticamente** (mitigazione per session fixation/hijacking)  
- [ ] L'utente **viene invitato** a terminare le sessioni esistenti prima che la nuova sessione venga autorizzata  
- [ ] Il nuovo tentativo di login **viene bloccato** finché non viene effettuato un logout manuale da un altro dispositivo  
- [ ] Nessuna azione intrapresa — più sessioni rimangono **abilitate** simultaneamente  

### L'applicazione fornisce un'interfaccia di gestione delle sessioni per l'utente?
- [ ] Sì — gli utenti **possono** visualizzare le sessioni attive (IP, dispositivo, ora) e terminarle da remoto  
- [ ] Sì — gli utenti **possono** visualizzare le sessioni attive ma **non possono** terminarle da remoto  
- [ ] No — gli utenti **non possono** vedere o gestire altre sessioni attive associate al proprio account  

### L'utente viene notificato quando viene rilevata un'attività di login concorrente?
- [ ] Sì — viene generato un avviso (email, SMS o in-app) per i login da nuovi dispositivi o posizioni  
- [ ] No — non viene fornita alcuna notifica quando vengono stabilite sessioni concorrenti  

---