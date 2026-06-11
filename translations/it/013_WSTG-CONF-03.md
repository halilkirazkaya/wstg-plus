## WSTG-CONF-03 — Verifica della Gestione delle Estensioni dei File per Informazioni Sensibili

L'attività di testing sulla gestione delle estensioni dei file consiste nell'identificare se il server web o l'application server riveli informazioni sensibili distribuendo file che dovrebbero essere limitati o eseguiti. Gli attaccanti cercano frequentemente file di backup, file di configurazione e frammenti di codice sorgente (ad es. `.bak`, `.old`, `.env`, `.inc`) che potrebbero essere rimasti nella web root e serviti come testo in chiaro a causa di una mancanza di mappatura specifica dei handler. Questa vulnerabilità è rilevante poiché può portare all'esposizione di credenziali di database, chiavi API e logica di business interna, facilitando uno sfruttamento più profondo dell'infrastruttura. Queste esposizioni si verificano solitamente in directory pubbliche, cartelle di caricamento (upload) o tramite impostazioni dei tipi MIME configurate in modo errato sul server.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se sono accessibili file di configurazione contenenti credenziali o l'intero codice sorgente.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Le estensioni dei file di backup o temporanei sensibili (ad es. `.bak`, `.old`, `.swp`, `~`) sono accessibili?
- [ ] No — il server restituisce 403 Forbidden o 404 Not Found per le comuni estensioni di backup  
- [ ] Sì — il server permette il download dei file di backup, ma questi **non** contengono dati sensibili  
- [ ] Sì — il server permette il download di file di backup contenenti dati sensibili o codice sorgente *(Alta)*  

### Il server espone file di configurazione di sistema o di ambiente (ad es. `.env`, `.config`, `.yml`, `.ini`)?
- [ ] No — i file di configurazione riservati **non** possono essere consultati e restituiscono 403/404  
- [ ] Sì — i file di configurazione sensibili **sono** accessibili e rivelano segreti interni o credenziali  

### È possibile recuperare il codice sorgente aggiungendo estensioni alternative (ad es. `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] No — il codice dell'applicazione **non** viene visualizzato come testo in chiaro indipendentemente dalla manipolazione dell'estensione  
- [ ] Sì — il codice sorgente viene rivelato perché il server **non** dispone di un handler per l'estensione aggiunta  

### Le directory amministrative o di metadati (ad es. `.git/`, `.svn/`, `.DS_Store`) sono protette dall'accesso pubblico?
- [ ] No — queste directory/file **non** esistono nella web root  
- [ ] Sì — l'accesso **non è possibile** grazie a regole di rewrite lato server o permessi  
- [ ] Sì — le directory dei metadati **sono** accessibili e consentono la ricostruzione completa del codice sorgente  

### In che modo il server gestisce le richieste per file con estensioni multiple (ad es. `file.php.jpg`)?
- [ ] No — il server assegna correttamente la priorità alla sicurezza dell'estensione interna o blocca la richiesta  
- [ ] Sì — il server elabora il file come se avesse la prima estensione, ma il bypass **non è possibile** per l'esecuzione  
- [ ] Sì — il server ignora l'estensione finale, rendendo **possibile** l'esecuzione di codice o la divulgazione del sorgente  

---