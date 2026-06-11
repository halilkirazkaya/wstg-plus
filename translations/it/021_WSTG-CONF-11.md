## WSTG-CONF-11 — Test Cloud Storage

Il testing delle errate configurazioni dello storage cloud prevede l'identificazione e l'audit dei servizi di object storage accessibili pubblicamente come Amazon S3, Azure Blobs o Google Cloud Storage. Queste risorse contengono spesso dati sensibili inclusi backup, codice sorgente, PII o file di configurazione che risultano esposti a causa di Access Control Lists (ACLs) o policy di Identity and Access Management (IAM) eccessivamente permissive. Gli attaccanti enumerano i nomi dei bucket attraverso la reconnaissance di file JavaScript, sorgenti HTML e tecniche di brute-force per trovare punti di ingresso per la data exfiltration o la modifica non autorizzata dei file. Lo sfruttamento può portare a un impatto significativo, inclusi data breach completi o la possibilità di distribuire file malevoli da un dominio attendibile.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica* |

> *La gravità diventa Critica se PII, credenziali o backup di produzione sono accessibili, o se è abilitato l'accesso in scrittura non autenticato.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Gli endpoint dello storage cloud (bucket/container) sono stati identificati ed enumerati?
- [ ] No — non vengono utilizzati o non sono individuabili endpoint di storage cloud  
- [ ] Sì — gli endpoint di storage sono identificati tramite analisi del codice o monitoraggio del traffico  
- [ ] Sì — gli endpoint di storage sono individuati tramite brute-force sulla convenzione di denominazione  

### Gli utenti non autenticati possono elencare i contenuti dello storage cloud?
- [ ] No — l'elenco dei bucket (listing) è **disabilitato** e restituisce 403 Forbidden o 404 Not Found  
- [ ] Sì — l'elenco è **abilitato** ma non sono presenti file sensibili  
- [ ] Sì — l'elenco è **abilitato** e i nomi/metadati dei file sensibili sono visibili  

### È possibile scaricare file sensibili dai bucket identificati?
- [ ] No — i file sono protetti da policy IAM o è richiesta un'autenticazione valida  
- [ ] Sì — i file sono accessibili ma richiedono un URL firmato (signed URL) che **non può** essere facilmente indovinato  
- [ ] Sì — i file sensibili (backup, `.env`, PII) sono **accessibili pubblicamente** per il download *(Critica)*  

### Il caricamento o la modifica di file senza autenticazione sono consentiti?
- [ ] No — i caricamenti o le modifiche non autenticati **non sono possibili**  
- [ ] Sì — è richiesto il caricamento autenticato ma il bypass **è possibile** tramite condizioni di policy deboli  
- [ ] Sì — la scrittura, sovrascrittura o eliminazione non autenticata di file è **possibile** *(Critica)*  

### Le policy Cross-Origin Resource Sharing (CORS) sull'endpoint di storage sono restrittive?
- [ ] Sì — il CORS è **disabilitato** o limitato a specifici origin attendibili  
- [ ] No — il CORS è **abilitato** con un origin wildcard `*`, consentendo l'accesso non autorizzato basato sul web  

---