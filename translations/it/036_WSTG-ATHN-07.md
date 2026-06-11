## WSTG-ATHN-07 — Testing for Weak Password Policy

La verifica di criteri per le password deboli (Weak Password Policy) comporta la valutazione dei requisiti di complessità, lunghezza ed entropia imposti da un'applicazione durante la creazione dell'account e l'aggiornamento delle credenziali. Policy insufficienti compromettono direttamente la riservatezza degli account utente, rendendoli suscettibili ad attacchi di tipo Dictionary Attack automatizzati, tentativi di Brute Force e Credential Stuffing. Queste vulnerabilità risiedono tipicamente negli endpoint di registrazione, nei workflow di reset della password e nei pannelli di gestione amministrativa degli utenti. Dal punto di vista di un attaccante, una policy permissiva riduce significativamente il costo computazionale e il tempo necessario per compromettere con successo le credenziali utilizzando wordlist comuni o tecniche di cracking basate su pattern.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Bassa* |

> *La gravità diventa Alta se l'applicazione manca di meccanismi di Account Lockout o Rate Limiting per prevenire il guessing automatizzato.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Strumenti:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### È imposta dall'applicazione una lunghezza minima della password?
- [ ] Sì — la lunghezza minima è di 12+ caratteri *(Più sicuro)*  
- [ ] Sì — la lunghezza minima è compresa tra 8 e 11 caratteri  
- [ ] Sì — la lunghezza minima è **inferiore** a 8 caratteri  
- [ ] No — non viene imposta alcuna lunghezza minima  

### Sono imposti requisiti di complessità (maiuscole, minuscole, cifre, simboli)?
- [ ] Sì — più classi di caratteri sono obbligatorie e il bypass **non è possibile**  
- [ ] Sì — le classi di caratteri sono suggerite ma **non** imposte  
- [ ] No — non viene applicato alcun requisito di complessità  

### L'applicazione blocca password comuni/deboli o password contenenti dati specifici dell'utente?
- [ ] Sì — password deboli note (es. "password123") e password basate sull'username **non possono** essere utilizzate  
- [ ] Sì — le password comuni sono bloccate, ma i dati specifici dell'utente (es. username, email) **possono** essere utilizzati  
- [ ] No — viene accettata qualsiasi stringa che soddisfi i requisiti di lunghezza/complessità  

### I requisiti di complessità o lunghezza possono essere aggirati tramite modifiche lato client?
- [ ] No — le policy sono imposte rigorosamente lato **server-side**  
- [ ] Sì — le policy sono imposte tramite JavaScript e **possono** essere aggirate intercettando la richiesta  

### La password policy è comunicata chiaramente all'utente senza esporre dettagli di implementazione?
- [ ] Sì — la policy è visibile durante l'inserimento e fornisce messaggi di errore generici  
- [ ] Sì — la policy è visibile ma i messaggi di errore rivelano regex specifiche o lacune logiche  
- [ ] No — la policy **non** è visibile, costringendo a una scoperta per tentativi (trial-and-error)  

---