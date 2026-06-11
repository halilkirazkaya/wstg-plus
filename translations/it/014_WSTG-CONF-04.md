## WSTG-CONF-04 — Analisi di vecchi backup e file non referenziati per informazioni sensibili

L'analisi di vecchi backup e file non referenziati consiste nell'identificare file dimenticati, temporanei o nascosti su un server web il cui accesso non è destinato al pubblico. Questi file includono spesso backup del codice sorgente (`.zip`, `.bak`), file di swap di editor di testo (`.swp`, `~`) o metadati del controllo di versione (`.git`, `.svn`) che possono causare la perdita di informazioni sensibili. Gli aggressori utilizzano wordlist automatizzate e strumenti di discovery per eseguire il Brute Force sulle convenzioni di denominazione comuni, con l'obiettivo di esfiltrare credenziali di database, chiavi API hardcoded o logiche che facilitano ulteriori exploit. Questa vulnerabilità deriva solitamente dalla manutenzione manuale del server o da pipeline CI/CD difettose che non riescono a sanificare la web root di produzione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se vengono scoperti file di configurazione, archivi del codice sorgente o credenziali.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Il directory listing è abilitato sul server web?
- [ ] No — il directory listing è **disabilitato** e restituisce un errore 403 Forbidden o un errore personalizzato  
- [ ] Sì — il directory listing è **abilitato** su directory non sensibili  
- [ ] Sì — il directory listing è **abilitato** su directory contenenti codice sorgente o file sensibili *(Critico)*  

### I file di backup con estensioni comuni (es. .bak, .old, .save) sono individuabili?
- [ ] No — le estensioni di backup comuni **non sono state trovate** o sono bloccate dalla configurazione del server  
- [ ] Sì — i file di backup esistono ma **non** contengono informazioni sensibili  
- [ ] Sì — i file di backup della configurazione o del codice sorgente **sono** accessibili *(Alta)*  

### Le directory del controllo di versione (es. .git, .svn) o i metadati sono esposti?
- [ ] No — i metadati del controllo di versione **non sono presenti** o sono correttamente bloccati  
- [ ] Sì — i metadati esistono ma l'intero repository **non può** essere ricostruito  
- [ ] Sì — l'intero repository del codice sorgente **può** essere esfiltrato tramite i metadati esposti  

### Sono presenti archivi compressi (es. .zip, .tar.gz, .rar) dell'applicazione nella web root?
- [ ] No — nessun archivio sensibile è stato individuato tramite Brute Force o enumerazione  
- [ ] Sì — sono stati trovati archivi, ma sono protetti da password o contengono asset pubblici  
- [ ] Sì — archivi non crittografati contenenti il codice sorgente o i dati dell'applicazione **sono** accessibili pubblicamente  

### L'applicazione espone file temporanei creati da editor di testo o IDE?
- [ ] No — i file temporanei come `.swp`, `~` o `.DS_Store` **non sono presenti**  
- [ ] Sì — sono **presenti** file temporanei non sensibili  
- [ ] Sì — i file temporanei rivelano frammenti di codice sorgente o percorsi interni  

---