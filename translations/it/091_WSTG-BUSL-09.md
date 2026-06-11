## WSTG-BUSL-09 — Test del Caricamento di File Malevoli

Le funzionalità di caricamento file consentono agli utenti di inviare dati al server, fornendo un vettore diretto per l'introduzione di contenuti malevoli nell'ambiente dell'applicazione. Gli attaccanti sfruttano logiche di validazione deboli per caricare web shell, malware o file appositamente creati — come SVG o HTML — per ottenere Remote Code Execution (RCE) o Cross-Site Scripting (XSS). Questa vulnerabilità si trova tipicamente in funzionalità quali impostazioni del profilo, sistemi di gestione documentale e gestori di allegati in cui il server non verifica correttamente i tipi di file, i contenuti e i permessi di esecuzione. Lo sfruttamento riuscito porta spesso alla compromissione totale del sistema, all'esfiltrazione di dati o all'uso del server come punto di appoggio (pivot point) per attacchi alla rete interna.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### L'applicazione fornisce funzionalità per il caricamento di file?
- [ ] No — non esiste alcuna funzionalità di caricamento file  
- [ ] Sì — la funzionalità esiste per gli utenti autenticati  
- [ ] Sì — la funzionalità è disponibile per gli utenti **non autenticati** *(Critico)*  

### La validazione dell'estensione del file è forzata lato server?
- [ ] Sì — viene applicata una allowlist rigorosa di estensioni e il bypass **non è possibile**  
- [ ] Sì — viene utilizzata una allowlist ma il bypass **è possibile** tramite case sensitivity o doppie estensioni  
- [ ] Sì — viene utilizzata una blocklist, che **può** essere bypassata con estensioni alternative (es. `.phtml`, `.asa`)  
- [ ] No — nessuna validazione dell'estensione è **applicata** lato server  

### Il contenuto del file viene validato oltre l'estensione o il tipo MIME?
- [ ] Sì — l'applicazione verifica i magic bytes ed esegue un'ispezione approfondita del contenuto  
- [ ] Sì — l'applicazione controlla solo l'header `Content-Type`, il quale **è** facilmente falsificabile  
- [ ] No — l'applicazione si affida interamente all'estensione del file per la validazione  

### I file caricati sono memorizzati in una directory con permessi di esecuzione?
- [ ] No — i file sono memorizzati su un servizio di storage dedicato (es. S3) o in una directory non eseguibile  
- [ ] Sì — i file sono memorizzati sul server web ma l'esecuzione è **disabilitata** tramite configurazione  
- [ ] Sì — i file sono memorizzati in una directory accessibile via web e l'esecuzione **è abilitata** *(Critico)*  

### I filtri di sicurezza possono essere bypassati tramite la manipolazione del nome del file?
- [ ] No — i nomi dei file vengono sanitizzati o rigenerati casualmente dal server  
- [ ] Sì — i filtri **possono** essere bypassati utilizzando null byte (es. `shell.php%00.jpg`)  
- [ ] Sì — i caratteri di path traversal (es. `../../`) **possono** essere utilizzati per caricare file in directory arbitrarie  

---