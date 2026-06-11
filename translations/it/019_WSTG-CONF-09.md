## WSTG-CONF-09 — Test File Permission

Il testing dei permessi dei file comporta l'audit dei livelli di controllo degli accessi assegnati ai file e alle directory sul server web per garantire che le risorse sensibili non siano eccessivamente esposte a utenti o processi non autorizzati. Permessi configurati in modo errato possono consentire agli attaccanti di leggere file di configurazione sensibili contenenti credenziali di database, visualizzare codice sorgente proprietario o modificare script lato server per ottenere la Remote Code Execution. Questa vulnerabilità si verifica tipicamente durante la fase di deployment quando i flag predefiniti "world-readable" o "world-writable" vengono lasciati su directory critiche dell'applicazione, come percorsi di configurazione, cartelle di log o repository di caricamento. Dal punto di vista di un attaccante, la scoperta di un file non adeguatamente protetto funge spesso da catalizzatore primario per la Privilege Escalation o la compromissione totale del sistema.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se i file di configurazione sensibili (ad es., `.env`, `web.config`) sono leggibili o se è possibile l'accesso in scrittura alla web root.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Tool:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### I file di configurazione sensibili dell'applicazione sono protetti da accessi non autorizzati?
- [ ] Sì — i file di configurazione (ad es., `.env`, `config.php`) **non sono accessibili** tramite richieste web  
- [ ] Sì — i file sono presenti ma l'accesso è **limitato** ai soli utenti locali autorizzati  
- [ ] No — i file di configurazione sensibili **sono accessibili** e divulgano credenziali o segreti  

### Un attaccante può modificare file o directory all'interno della web root?
- [ ] No — tutte le directory della web root sono in **sola lettura** per l'utente del server web  
- [ ] Sì — esistono directory scrivibili ma l'esecuzione di script è **disabilitata**  
- [ ] Sì — esistono directory world-writable e **consentono** il caricamento e l'esecuzione di script *(Critico)*  

### I metadati del controllo di versione o i file di backup sono esposti a causa di permessi permissivi?
- [ ] No — `.git`, `.svn` e i file di backup (ad es., `.bak`, `~`) **non sono presenti** o sono protetti  
- [ ] Sì — le directory dei metadati esistono ma il directory listing è **disabilitato**  
- [ ] Sì — i metadati sensibili o i backup sono **completamente accessibili**, consentendo il recupero del codice sorgente  

### L'utente del server web opera secondo il principio del minimo privilegio (least privilege)?
- [ ] Sì — il processo del server web viene eseguito come utente **dedicato a basso privilegio**  
- [ ] No — il processo del server web viene eseguito con **privilegi non necessari** (ad es., `root` o `Administrator`)  

### I file di log sono protetti da lettura o modifica non autorizzata?
- [ ] Sì — i file di log sono **riservati** agli utenti amministrativi  
- [ ] Sì — i file di log sono leggibili ma **non** contengono token di sessione o PII  
- [ ] No — i file di log sono **pubblicamente leggibili** e divulgano dati sensibili sulle transazioni o informazioni sugli utenti  

---