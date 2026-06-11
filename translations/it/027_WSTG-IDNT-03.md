## WSTG-IDNT-03 — Test del Processo di Provisioning degli Account

Il processo di provisioning degli account disciplina il modo in cui le nuove identità vengono create, verificate e assegnate a specifici privilegi all'interno di un ambiente applicativo. Gli attaccanti mirano a questo workflow per eseguire registrazioni di massa automatizzate finalizzate allo spam o al Denial-of-Service (DoS), eludere i passaggi di verifica dell'identità per creare account fraudolenti o manipolare i parametri per concedersi permessi elevati durante la fase di registrazione (signup). Le vulnerabilità si manifestano spesso nei moduli di registrazione self-service, nei sistemi basati su invito o nelle console di provisioning amministrativo, dove i difetti della Business Logic consentono la creazione di account non autorizzati o l'Escalation dei Privilegi (Privilege Escalation). Dal punto di vista di un attaccante, la compromissione del flusso di provisioning fornisce un punto di appoggio legittimo che può essere utilizzato come base per exploitation più profonde, esfiltrazione di dati o attacchi di Ingegneria Sociale (Social Engineering).


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La Gravità diventa Critica se un attaccante può effettuare il provisioning di account amministrativi o eludere le restrizioni globali di registrazione.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### La registrazione autonoma (self-registration) è strettamente controllata o limitata agli utenti autorizzati?
- [ ] Sì — la registrazione è **disabilitata** o richiede l'approvazione amministrativa manuale  
- [ ] Sì — la registrazione è aperta ma limitata a domini email **autorizzati** o a token di invito  
- [ ] No — la registrazione è **aperta** a qualsiasi utente senza restrizioni di dominio o token  

### È richiesto un meccanismo robusto di verifica dell'identità (ad es. email/SMS) prima dell'attivazione dell'account?
- [ ] Sì — la verifica è **richiesta** e **non può** essere elusa  
- [ ] Sì — la verifica è **richiesta** ma **può** essere elusa tramite la manipolazione dei parametri (Parameter Tampering) o token prevedibili  
- [ ] No — gli account sono **attivi immediatamente** dopo l'invio senza alcuna verifica  

### L'utente può manipolare i parametri relativi ai ruoli o ai permessi durante la richiesta di registrazione?
- [ ] No — i ruoli sono **assegnati lato server (server-side)** e non esistono parametri lato client  
- [ ] Sì — i parametri del ruolo esistono ma la manipolazione **non è possibile** a causa della validazione lato server  
- [ ] Sì — i parametri di ruolo/permesso (ad es. `role=admin`, `is_staff=true`) **possono** essere modificati per ottenere un accesso privilegiato *(Critico)*  

### Sono implementati controlli di Rate Limiting o anti-automazione per prevenire la creazione di massa di account?
- [ ] Sì — il Rate Limiting e i CAPTCHA sono **attivi** ed efficaci  
- [ ] Sì — i controlli esistono ma l'elusione **è possibile** tramite lo spoofing degli header o servizi di risoluzione CAPTCHA  
- [ ] No — non viene **applicato** alcun Rate Limiting o controllo anti-automazione  

### Il processo di provisioning espone informazioni sugli account esistenti (User Enumeration)?
- [ ] No — i messaggi di risposta sono **identici** per utenti esistenti e non esistenti  
- [ ] Sì — risposte differenti o variazioni temporali (timing variations) **consentono** l'enumerazione degli utenti esistenti  

---