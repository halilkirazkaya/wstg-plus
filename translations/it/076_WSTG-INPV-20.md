## WSTG-INPV-20 — Mass Assignment

Il Mass Assignment, noto anche come Overposting o Insecure Deserialization dei parametri, si verifica quando un'applicazione web associa automaticamente i parametri delle richieste HTTP in entrata alle proprietà di oggetti interni senza un filtraggio adeguato degli attributi. Questa vulnerabilità è diffusa nei moderni framework MVC e nelle API RESTful dove i dati JSON, XML o form-encoded vengono deserializzati direttamente nei modelli di dati lato applicazione. Gli attaccanti sfruttano questo comportamento iniettando parametri aggiuntivi non elencati in una richiesta — come `isAdmin`, `role` o `account_balance` — per modificare campi sensibili che gli sviluppatori non intendevano esporre al controllo dell'utente. L'exploitation riuscita si traduce tipicamente in privilege escalation, modifica non autorizzata dei dati o bypass dei flussi critici della business logic.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Media* |

> *La gravità diventa Alta se è possibile modificare attributi amministrativi, livelli di permesso o campi di fatturazione.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### L'applicazione utilizza il binding automatico per i parametri delle richieste in entrata?
- [ ] No — i parametri sono mappati manualmente a variabili specifiche o oggetti sicuri  
- [ ] Sì — viene utilizzato il binding automatico ma vengono applicate **white-list** rigorose o DTO (Data Transfer Objects)  
- [ ] Sì — viene utilizzato il binding automatico e gli attributi interni sensibili **possono** essere modificati  

### È possibile iniettare con successo attributi amministrativi o relativi ai permessi?
- [ ] No — i parametri iniettati vengono ignorati, rimossi o causano un errore di validazione  
- [ ] Sì — i parametri extra vengono accettati ma **non** influenzano lo stato dell'oggetto nel backend  
- [ ] Sì — l'iniezione di parametri come `is_admin`, `role` o `permissions` permette **con successo** la privilege escalation *(Critico)*  

### È possibile modificare la business logic sensibile o i campi finanziari tramite overposting?
- [ ] No — campi come `price`, `balance` o `status` sono protetti dal mass assignment  
- [ ] Sì — gli attributi di business sensibili **possono** essere modificati aggiungendoli al corpo della richiesta JSON o form  

### L'applicazione rivela strutture di oggetti interni che facilitano il parameter guessing?
- [ ] No — le risposte includono solo i campi pubblici previsti e la documentazione è limitata  
- [ ] Sì — i campi degli oggetti interni sono visibili nelle risposte GET, rendendo **possibile** il parameter discovery  
- [ ] Sì — la documentazione delle API (Swagger/OpenAPI) elenca esplicitamente le proprietà interne che **possono** essere assegnate  

---