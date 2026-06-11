## WSTG-SESS-07 — Test del Session Timeout

Il test del session timeout valuta se un'applicazione termini efficacemente la sessione di un utente dopo un periodo predefinito di inattività o una durata totale. Questo controllo è fondamentale per mitigare il rischio di Session Hijacking, in particolare su workstation condivise o in scenari in cui un attaccante cattura un session identifier tramite intercettazione di rete o XSS. I pentester analizzano sia gli idle timeout, che si attivano dopo un periodo di inattività, sia gli absolute timeout, che limitano la durata totale di una sessione indipendentemente dall'attività. Dal punto di vista di un attaccante, sessioni indefinite o eccessivamente lunghe forniscono una finestra di opportunità estesa per mantenere l'accesso non autorizzato e bypassare la necessità di ri-autenticazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### L'applicazione implementa un idle session timeout?
- [ ] Sì — la sessione scade dopo un breve periodo (es. 15-30 minuti) di inattività  
- [ ] Sì — la sessione scade ma la durata del timeout è **eccessivamente lunga** (es. > 24 ore)  
- [ ] No — la sessione **rimane attiva** a tempo indeterminato durante l'inattività  

### Il session timeout è applicato lato server (server-side)?
- [ ] Sì — il server rifiuta le richieste con token scaduti indipendentemente dallo stato lato client  
- [ ] No — il timeout è applicato **solo** tramite JavaScript lato client o meta-refresh  

### L'applicazione implementa un absolute session timeout?
- [ ] Sì — la sessione viene terminata dopo una durata massima fissa, indipendentemente dall'attività  
- [ ] No — la sessione **può** essere estesa indefinitamente finché vi è un'attività continua  

### L'identificatore di sessione viene invalidato sul server al momento del timeout?
- [ ] Sì — il session token **non può** essere riutilizzato una volta raggiunta la soglia di timeout  
- [ ] No — il session token **è ancora valido** sul server anche dopo che il client è stato reindirizzato alla pagina di login  

### La sessione può essere mantenuta attiva a tempo indeterminato utilizzando richieste heartbeat automatizzate?
- [ ] No — l'absolute timeout o controlli secondari **impediscono** l'estensione infinita della sessione  
- [ ] Sì — richieste periodiche in background (es. AJAX) **possono** mantenere la sessione attiva indefinitamente  

---