## WSTG-INPV-18 — Testing for Server-Side Template Injection

Il Server-Side Template Injection (SSTI) si verifica quando un'applicazione incorpora l'input controllato dall'utente all'interno di un motore di template lato server senza un'adeguata sanificazione o escaping. Ciò consente a un utente malintenzionato di iniettare direttive di template malevole, che vengono poi eseguite sul server, portando potenzialmente alla compromissione totale del sistema tramite Remote Code Execution (RCE). L'SSTI si manifesta tipicamente in funzionalità come la generazione dinamica di email, pagine di profilo e sistemi di notifica che utilizzano motori come Jinja2, Twig o FreeMarker. Dal punto di vista di un attaccante, questa vulnerabilità viene spesso sfruttata per sottrarre variabili d'ambiente sensibili, leggere file interni o eseguire comandi di sistema sfruttando gli oggetti e i metodi nativi del motore di template.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Strumenti:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### L'applicazione utilizza un motore di template lato server per elaborare l'input dell'utente?
- [ ] No — l'applicazione **non** utilizza template lato server per i contenuti dinamici  
- [ ] Sì — i template vengono utilizzati ma l'input dell'utente **non** è incorporato nelle direttive del template  
- [ ] Sì — l'input dell'utente è incorporato nei template **senza** una corretta sanificazione  

### Le espressioni aritmetiche di base vengono valutate quando vengono iniettate nei campi di input?
- [ ] No — payload come `{{7*7}}` o `${7*7}` vengono riflessi come stringhe letterali  
- [ ] Sì — le espressioni vengono valutate (ad esempio, restituendo `49`), confermando che il motore di template **è vulnerabile**  

### È possibile accedere agli oggetti o ai metodi integrati del motore di template?
- [ ] No — l'accesso a oggetti, metodi o configurazioni pericolosi è **limitato** o protetto da sandbox  
- [ ] Sì — gli oggetti integrati (ad esempio, `self`, `config`, `__class__`) **possono** essere consultati per sottrarre dati sensibili  
- [ ] Sì — la Remote Code Execution (RCE) tramite chiamate di sistema o librerie del sistema operativo **è possibile**  

### Esiste un meccanismo di sandbox o di allowlist che impedisce l'esecuzione di direttive pericolose?
- [ ] Sì — è presente una allowlist rigorosa o una sandbox sicura e il bypass **non è possibile**  
- [ ] Sì — è presente una sandbox ma il bypass **è possibile** tramite specifici payload dipendenti dal motore  
- [ ] No — nessuna sandbox o allowlist **è applicata** al motore di template  

---