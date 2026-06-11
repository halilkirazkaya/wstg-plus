## WSTG-SESS-01 — Testing for Session Management Schema

Il testing dello schema di gestione della sessione si concentra sull'analisi di come l'applicazione gestisce il ciclo di vita e la struttura degli identificativi di sessione per garantire che siano robusti contro la predizione e l'hijacking. Gli attaccanti acquisiscono molteplici session token per eseguire analisi statistiche, alla ricerca di pattern, bassa entropia o incrementi prevedibili che permettano di falsificare sessioni valide per altri utenti. Le debolezze nello schema si manifestano spesso come vulnerabilità di session fixation o attraverso l'uso di valori non sufficientemente casuali, tipicamente presenti nei cookie HTTP, nei parametri URL o nelle implementazioni di header personalizzati. Compromettere lo schema di sessione è un attacco ad alto impatto che garantisce a un avversario l'accesso completo allo stato autenticato di una vittima senza necessità delle sue credenziali primarie.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Strumenti:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### L'identificativo di sessione è sufficientemente complesso e casuale?
- [ ] Sì — il token supera i test di casualità statistica (alta entropia) e il bypass **non è possibile**  
- [ ] Sì — il token è lungo ma contiene pattern prevedibili o segmenti statici  
- [ ] No — il token è sequenziale, basato su timestamp prevedibili o ha un'entropia **criticante bassa** *(Critico)*  

### Dove viene trasmesso e memorizzato l'identificativo di sessione?
- [ ] L'identificativo è memorizzato in un cookie `Secure` e `HttpOnly` *(Più sicuro)*  
- [ ] L'identificativo è memorizzato in un cookie ma mancano i flag `Secure` o `HttpOnly`  
- [ ] L'identificativo è trasmesso **solo** tramite parametri URL o richieste `GET` *(Alto Rischio)*  

### L'applicazione emette un nuovo identificativo di sessione dopo un'autenticazione riuscita?
- [ ] Sì — un nuovo ID di sessione univoco **viene generato** immediatamente dopo il login  
- [ ] No — l'ID di sessione rimane lo stesso prima e dopo il login *(Session Fixation possibile)*  

### L'identificativo di sessione è legato a uno specifico indirizzo IP o User-Agent per prevenirne il riutilizzo?
- [ ] Sì — la sessione viene invalidata se il fingerprint del client cambia significativamente  
- [ ] No — la sessione **può** essere riutilizzata su diversi indirizzi IP o dispositivi senza verifica aggiuntiva  

### Gli identificativi di sessione vengono correttamente invalidati lato server dopo il logout o il timeout?
- [ ] Sì — lo stato della sessione lato server viene **completamente distrutto** al logout  
- [ ] No — la sessione rimane valida sul server anche se il cookie lato client viene eliminato  
- [ ] No — la sessione **non scade mai** o ha un periodo di timeout eccessivamente lungo  

---