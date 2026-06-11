## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Le vulnerabilità di Directory Traversal e File Inclusion si verificano quando un'applicazione utilizza input controllabili dall'utente per costruire percorsi verso file o directory senza una sufficiente validazione o sanitizzazione. Gli attaccanti sfruttano queste falle iniettando sequenze come `../` per navigare al di fuori della directory prevista, accedendo potenzialmente a file di sistema sensibili, dati di configurazione o codice sorgente dell'applicazione. In casi più gravi che coinvolgono Local File Inclusion (LFI) o Remote File Inclusion (RFI), un attaccante può ottenere la remote code execution includendo script malevoli o sfruttando il log poisoning e i wrapper PHP. Queste vulnerabilità risiedono tipicamente nei parametri utilizzati per il caricamento dinamico di contenuti, motori di template o endpoint di recupero immagini in cui la logica lato server gestisce i percorsi del file system.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Strumenti:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### I parametri accettano nomi di file o percorsi per l'elaborazione lato server?
- [ ] No — nessun parametro sembra interagire con il file system  
- [ ] Sì — i parametri esistono ma utilizzano una allowlist rigorosa di identificatori di file  
- [ ] Sì — i parametri accettano nomi di file o percorsi diretti  

### L'input viene sanitizzato per prevenire sequenze di directory traversal?
- [ ] Sì — l'input è validato rispetto a una allowlist rigorosa e le sequenze di traversal **non sono possibili**  
- [ ] Sì — l'input viene sanitizzato rimuovendo le sequenze `../`, ma il bypass ricorsivo **è possibile**  
- [ ] No — nessuna sanitizzazione o validazione **viene applicata** all'input relativo ai percorsi  

### È possibile accedere a file esterni alla directory limitata tramite sequenze di traversal?
- [ ] No — l'applicazione o il sistema operativo impediscono l'accesso ai file al di fuori dell'ambito definito  
- [ ] Sì — l'accesso ai file all'interno della web root **è possibile**  
- [ ] Sì — l'accesso a file di sistema sensibili (es. `/etc/passwd`, `C:\Windows\win.ini`) **è possibile** *(Critico)*  

### Il server consente l'inclusione di URL remoti (RFI)?
- [ ] No — la Remote File Inclusion è **disabilitata** a livello di server/applicazione  
- [ ] Sì — i file remoti **possono** essere inclusi ma l'esecuzione **non è possibile**  
- [ ] Sì — i file remoti **possono** essere inclusi ed eseguiti sul server *(Critico)*  

### I filtri possono essere aggirati utilizzando codifiche o caratteri speciali?
- [ ] No — i filtri gestiscono efficacemente varie codifiche e null byte  
- [ ] Sì — il bypass **è possibile** utilizzando URL encoding, double URL encoding o Unicode a 16 bit  
- [ ] Sì — il bypass **è possibile** utilizzando l'iniezione di null byte (`%00`) o wrapper del filesystem (es. `php://filter`)  

---