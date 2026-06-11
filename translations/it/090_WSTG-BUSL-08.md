## WSTG-BUSL-08 — Test del Caricamento di Tipi di File Non Previsti

Il caricamento di tipi di file non previsti consente agli aggressori di bypassare i vincoli della business logic inviando file che l'applicazione non è destinata a elaborare, portando potenzialmente a Remote Code Execution, Cross-Site Scripting (XSS) o Denial of Service. Gli aggressori sondano queste vulnerabilità modificando le estensioni dei file, i tipi MIME e i magic bytes per indurre la logica di validazione lato server ad accettare contenuti malevoli. Ciò si verifica tipicamente nei caricamenti delle immagini del profilo, nei sistemi di gestione documentale o negli allegati dei ticket di supporto dove non esiste una validazione sufficiente sul back-end. Uno sfruttamento (Exploit) riuscito può compromettere il server ospite se il file caricato viene eseguito, o portare ad attacchi lato client se il file viene restituito ad altri utenti con un `Content-Type` errato.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### L'applicazione limita il caricamento dei file a un set specifico di estensioni?
- [ ] Sì — viene applicata una whitelist rigorosa di estensioni e il bypass **non è possibile**  
- [ ] Sì — è presente una whitelist di estensioni ma il bypass **è possibile** tramite doppie estensioni o null bytes  
- [ ] No — qualsiasi estensione di file **può** essere caricata  

### Il contenuto del file viene validato oltre l'estensione del file?
- [ ] Sì — la logica lato server verifica i Magic Bytes ed esegue un'ispezione approfondita del contenuto  
- [ ] Sì — la logica lato server verifica il tipo MIME solo tramite l'header `Content-Type`  
- [ ] No — nessuna validazione del contenuto **viene applicata** dopo il controllo dell'estensione  

### Un aggressore può eseguire codice caricando una web shell o uno script?
- [ ] No — i file caricati sono memorizzati in una directory non eseguibile o in un bucket di archiviazione (S3/Azure Blob)  
- [ ] Sì — i file sono memorizzati in una directory accessibile via web ma l'esecuzione **è disabilitata** tramite la configurazione del server  
- [ ] Sì — gli script malevoli caricati **possono** essere eseguiti direttamente tramite un percorso URL prevedibile *(Critica)*  

### Sono possibili attacchi lato client come XSS tramite i tipi di file caricati?
- [ ] No — i file vengono serviti con `Content-Disposition: attachment` e `X-Content-Type-Options: nosniff`  
- [ ] Sì — i file SVG o HTML **possono** essere caricati e renderizzati nel browser, portando a XSS  
- [ ] Sì — i metadati dell'immagine (EXIF) **vengono elaborati** e riflessi nella UI senza sanitizzazione  

---