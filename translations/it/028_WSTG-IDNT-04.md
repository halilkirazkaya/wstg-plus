## WSTG-IDNT-04 — Testing for Account Enumeration and Guessable User Account

L'Account Enumeration si verifica quando un'applicazione rivela l'esistenza o l'inesistenza di un account utente attraverso variazioni nelle risposte HTTP, messaggi di errore o differenze temporali misurabili. Gli attaccanti sfruttano questo comportamento inviando sistematicamente liste di nomi utente per identificare account validi, che vengono successivamente presi di mira per attacchi di Credential Stuffing, Brute Force o Social Engineering. Questa vulnerabilità si manifesta comunemente nei portali di login, nelle funzionalità di recupero password, nei moduli di registrazione e negli endpoint API che gestiscono identificatori specifici dell'utente. Dal punto di vista di un attaccante, un'enumerazione riuscita riduce significativamente la superficie di attacco trasformando un tentativo di Brute Force alla cieca in uno sfruttamento mirato di identità note e valide.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### I messaggi di errore di autenticazione sono coerenti in tutti gli scenari?
- [ ] Sì — vengono utilizzati messaggi di errore generici sia per nomi utente non validi che per password non valide  
- [ ] Sì — i messaggi sono simili, ma **sono presenti** lievi variazioni nella punteggiatura o nel formato (casing)  
- [ ] No — i messaggi di errore indicano esplicitamente "Utente non trovato" o "Password errata" *(Vulnerabile)*  

### Il flusso di reset della password o di "password dimenticata" rivela l'esistenza dell'account?
- [ ] No — l'applicazione restituisce un messaggio generico indipendentemente dal fatto che l'email/nome utente esista  
- [ ] Sì — i controlli sono presenti, ma il bypass **è possibile** tramite la lunghezza della risposta o i codici di stato  
- [ ] Sì — l'applicazione conferma esplicitamente se è stata inviata un'email di reset o se l'account **non esiste**  

### Il processo di registrazione è protetto contro l'User Enumeration?
- [ ] No — la registrazione non rivela informazioni o utilizza CAPTCHA/Rate Limiting per prevenire controlli massivi  
- [ ] Sì — i controlli sono presenti, ma il bypass **è possibile** tramite differenze temporali (timing) o nelle risposte API  
- [ ] Sì — l'applicazione notifica immediatamente l'utente se il nome utente o l'email **è già in uso**  

### Le differenze temporali sono misurabili confrontando nomi utente validi e non validi?
- [ ] No — i tempi di risposta sono coerenti indipendentemente dalla validità dell'account grazie a confronti a tempo costante (constant-time comparisons)  
- [ ] Sì — esistono differenze temporali ma sono troppo piccole per essere misurate in modo affidabile sulla rete  
- [ ] Sì — discrepanze temporali misurabili **sono presenti** (ad esempio, a causa dell'esecuzione dell'hashing della password solo per gli utenti validi)  

### L'applicazione utilizza convenzioni di denominazione prevedibili o indovinabili per gli account?
- [ ] No — gli identificativi degli account sono casuali (UUID) o definiti dall'utente e complessi  
- [ ] Sì — gli identificativi degli account seguono un pattern (es. `emp001`, `emp002`) e l'enumerazione **è possibile**  
- [ ] Sì — gli identificativi sono interi sequenziali, rendendo **possibile** l'enumerazione completa del database  

---