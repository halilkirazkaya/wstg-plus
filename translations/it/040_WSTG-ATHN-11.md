## WSTG-ATHN-11 — Testing Multi-Factor Authentication (MFA)

Il testing della Multi-Factor Authentication (MFA) valuta la robustezza del secondo livello di sicurezza progettato per prevenire l'accesso non autorizzato anche quando le credenziali primarie sono compromesse. Gli attaccanti tentano frequentemente di bypassare la MFA identificando endpoint in cui non è applicata in modo coerente, come versioni legacy delle API, backend mobile o flussi di ripristino password. L'Exploit spesso comporta la manipolazione delle risposte del server, il Brute Force di codici a breve durata (OTP) o lo sfruttamento di race conditions e falle nella gestione delle sessioni che consentono a un utente di passare a uno stato autenticato senza fornire il secondo fattore.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### La MFA è applicata in modo coerente su tutti i portali di autenticazione?
- [ ] Sì — la MFA è richiesta per tutti i tentativi di login via web, mobile e basati su API  
- [ ] Sì — ma la MFA **non è applicata** su endpoint specifici (es. `/api/v1/login` legacy o portali specifici per il mobile)  
- [ ] No — la MFA **non è implementata** nell'applicazione *(Critica)*  

### È possibile bypassare la fase di verifica MFA tramite navigazione diretta dell'URL o manipolazione della risposta?
- [ ] No — la navigazione diretta o il Parameter Tampering **non possono** bypassare la challenge  
- [ ] Sì — la navigazione diretta verso URL di dashboard interne **è possibile** senza completare la challenge MFA  
- [ ] Sì — la manipolazione della risposta del server (es. cambiare un `401 Unauthorized` in `200 OK`) **è possibile** per ottenere l'accesso  

### Il processo di verifica del codice MFA è protetto contro attacchi di Brute Force?
- [ ] Sì — un Rate Limiting rigoroso o il blocco dell'account **viene applicato** dopo molteplici tentativi falliti di OTP  
- [ ] Sì — il Rate Limiting esiste ma **è possibile** bypassarlo tramite rotazione degli IP o manipolazione degli header  
- [ ] No — non viene applicato alcun Rate Limiting e il Brute Force automatizzato dei codici **è possibile**  

### L'applicazione mantiene uno stato di sessione sicuro durante la transizione MFA?
- [ ] Sì — i token di sessione con privilegi elevati vengono rilasciati **solo dopo** il completamento con successo della MFA  
- [ ] No — un cookie di sessione completamente funzionale viene rilasciato **prima** che la MFA sia completata, consentendo l'accesso ad alcune funzionalità  
- [ ] No — l'applicazione utilizza un identificatore statico o una transizione di sessione prevedibile che **può** essere soggetta a Hijacking  

### I fattori MFA alternativi (SMS, Email, Codici di Backup) sono vulnerabili a Exploit?
- [ ] No — tutti i fattori utilizzano valori sicuri, non prevedibili e limitati nel tempo  
- [ ] Sì — i codici di backup sono prevedibili o **possono** essere enumerati  
- [ ] Sì — la funzionalità "Reinvia Codice" può essere abusata per eseguire flooding di SMS/Email o rivelare dettagli di contatto parziali  

---