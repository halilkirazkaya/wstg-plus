## WSTG-IDNT-02 — Test del Processo di Registrazione Utente

Il test del processo di registrazione utente consiste nel valutare il workflow attraverso il quale vengono create nuove identità per garantire che non possa essere abusato per accessi non autorizzati o sfruttamento automatizzato. Le vulnerabilità in questo processo si manifestano spesso come mancanza di verifica dell'identità (email/SMS), suscettibilità alla creazione massiva di account tramite bot, o Information Disclosure attraverso messaggi di errore descrittivi che rivelano nomi utente esistenti. Gli attaccanti sfruttano queste falle per eseguire User Enumeration, condurre campagne di spam su larga scala o manipolare i parametri di registrazione per ottenere privilegi elevati durante la fase di creazione dell'account. Questo test è fondamentale per proteggere l'integrità dell'applicazione e impedire agli aggressori di popolare il sistema con account fraudolenti o malevoli.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### È richiesta la verifica dell'identità prima che un nuovo account diventi attivo?
- [ ] Sì — la registrazione richiede un token univoco a scadenza temporale inviato via email o SMS  
- [ ] Sì — la verifica è richiesta ma i token sono **deboli** o **prevedibili**  
- [ ] No — gli account vengono attivati **immediatamente** senza alcuna fase di verifica  

### Il processo di registrazione previene la User Enumeration?
- [ ] Sì — l'applicazione restituisce risposte **identiche** sia per email/username nuovi che per quelli esistenti  
- [ ] No — i messaggi di errore (es. "Email già in uso") **permettono** a un attaccante di mappare gli utenti registrati  
- [ ] No — differenze temporali nelle risposte del server (**timing differences**) rivelano se un utente esiste  

### Sono implementati controlli anti-automazione per prevenire la registrazione massiva?
- [ ] Sì — meccanismi CAPTCHA o proof-of-work sono **presenti** ed **efficaci**  
- [ ] Sì — i controlli esistono ma il bypass **è possibile** tramite endpoint API o manipolazione degli header  
- [ ] No — nessun Rate Limiting o CAPTCHA è **applicato** all'endpoint di registrazione  

### I parametri di registrazione possono essere manipolati per scalare i privilegi?
- [ ] No — i ruoli utente sono assegnati **esclusivamente** dalla logica server-side  
- [ ] Sì — parametri come `role`, `admin`, o `group_id` **possono** essere modificati nella richiesta per ottenere un accesso superiore  

### La password policy è applicata durante la fase di registrazione?
- [ ] Sì — requisiti di password complessa sono **imposti** lato server  
- [ ] No — password deboli (es. "password123") sono **accettate** dal sistema  

---