## WSTG-INPV-12 — Command Injection

La Command Injection si verifica quando un'applicazione passa dati non sicuri forniti dall'utente, come input di moduli, header HTTP o cookie, a una shell di sistema. Questa vulnerabilità consente a un malintenzionato di eseguire comandi arbitrari del sistema operativo sul server, tipicamente con i privilegi del processo dell'applicazione web. Dal punto di vista di un attaccante, questa viene sfruttata utilizzando metacaratteri della shell come punti e virgola, pipe o backtick per concatenare comandi malevoli al flusso di esecuzione previsto. L'impatto è frequentemente catastofico, portando alla compromissione totale del sistema, all'esfiltrazione non autorizzata di dati o al movimento laterale all'interno della rete interna.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Strumenti:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### L'applicazione richiama comandi a livello di sistema o eseguibili della shell?
- [ ] No — l'applicazione **non** interagisce con la shell del sistema operativo  
- [ ] Sì — l'applicazione utilizza API interne o librerie di alto livello per i compiti di sistema  
- [ ] Sì — l'applicazione richiama direttamente comandi della shell tramite chiamate di sistema come `system()`, `exec()` o `popen()`  

### L'input dell'utente viene validato o sanificato prima di essere passato alle chiamate di sistema?
- [ ] Sì — viene applicata una validazione dell'input e una allow-list rigorosa  
- [ ] Sì — viene utilizzata la sanificazione o l'escaping, ma il bypass **è possibile**  
- [ ] No — l'input dell'utente viene passato direttamente alla shell **senza** validazione  

### I metacaratteri della shell possono essere utilizzati per manipolare il flusso di esecuzione dei comandi?
- [ ] No — i metacaratteri sono correttamente filtrati o sottoposti a escaping  
- [ ] Sì — l'esecuzione in-band, dove l'output del comando viene riflesso nella risposta, **è possibile**  
- [ ] Sì — l'esecuzione blind, dove nessun output viene riflesso, **è possibile**  

### L'esfiltrazione out-of-band (OOB) è possibile dall'host?
- [ ] No — il server non ha traffico in uscita o i segnali OOB (DNS/HTTP) falliscono  
- [ ] Sì — l'esfiltrazione di dati tramite segnali OOB basati su DNS o HTTP **è possibile**  

### Il processo dell'applicazione viene eseguito con privilegi di sistema elevati?
- [ ] No — l'applicazione viene eseguita con un account di servizio dedicato a bassi privilegi  
- [ ] Sì — l'applicazione viene eseguita come `root`, `Administrator` o un utente con privilegi elevati *(Critico)*  

---