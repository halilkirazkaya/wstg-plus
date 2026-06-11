## WSTG-BUSL-05 — Test dei limiti sul numero di volte in cui una funzione può essere utilizzata

Questo test valuta se un'applicazione impone vincoli di business logic sulla frequenza e sul numero totale di volte in cui una specifica azione o funzione può essere eseguita da un singolo utente, sessione o indirizzo IP. La mancata implementazione di questi limiti consente agli attaccanti di abusare di funzioni ad alto valore—come l'invio di SMS OTP, l'attivazione di email di password reset, l'applicazione di codici sconto o l'esecuzione di pesanti data exports—portando a perdite finanziarie, esaurimento delle risorse o danni reputazionali. Queste vulnerabilità si trovano tipicamente negli endpoint transazionali o nelle funzionalità di comunicazione e vengono sfruttate tramite automated scripts che ripetono l'azione ben oltre le soglie di business previste. Dal punto di vista di un attaccante, questo è un obiettivo primario per SMS pumping, mail bombing o workflow di brute-forcing che mancano di espliciti rate-limiting o controlli basati sul conteggio (count-based throttles).


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se la funzione coinvolge transazioni finanziarie, costi SMS o impatta sulla disponibilità a livello di sistema.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### L'applicazione definisce e impone limiti sulle funzioni di business sensibili?
- [ ] Sì — i limiti sono rigorosamente imposti e il bypass **non è possibile**  
- [ ] Sì — i limiti sono imposti ma sono troppo elevati per prevenire l'abuso  
- [ ] No — nessun limite è applicato all'esecuzione della funzione  

### I rate limits e i contatori di utilizzo sono imposti lato server?
- [ ] Sì — l'imposizione è rigorosamente lato server e **non può** essere manipolata  
- [ ] No — l'imposizione si affida alla logica lato client (es. JavaScript o campi nascosti)  
- [ ] No — non esiste alcuna imposizione a nessun livello  

### I limiti di utilizzo possono essere bypassati tramite la manipolazione di header o sessioni?
- [ ] No — il bypass tramite `X-Forwarded-For`, `Origin` o la rotazione della sessione **non è possibile**  
- [ ] Sì — i limiti **possono** essere bypassati tramite lo spoofing degli header IP o la cancellazione dei cookie  
- [ ] Sì — i limiti **possono** essere bypassati creando account o sessioni multiple  

### Il superamento del limite attiva una risposta difensiva appropriata?
- [ ] Sì — l'applicazione restituisce `429 Too Many Requests` e registra l'evento *(Più sicuro)*  
- [ ] Sì — l'applicazione restituisce un errore ma **non** registra il potenziale abuso  
- [ ] No — l'applicazione continua a elaborare le richieste ma ignora l'output  
- [ ] No — l'applicazione non fornisce alcuna risposta o va in crash sotto carico  

### L'impatto dell'abuso della funzione è significativo per il business?
- [ ] No — l'abuso della funzione ha un costo o un impatto sulle risorse trascurabile  
- [ ] Sì — l'abuso **può** portare a un consumo minore di risorse o al fastidio dell'utente  
- [ ] Sì — l'abuso **può** portare a costi finanziari significativi (es. tariffe SMS) o al denial of service *(Critico)*  

---