## WSTG-BUSL-06 — Test per l'Aggiramento dei Flussi di Lavoro (Work Flows)

L'aggiramento dei flussi di lavoro (Workflow circumvention) comporta la manipolazione della logica di un'applicazione per eludere le sequenze previste o i prerequisiti all'interno di processi multi-fase. Gli attaccanti identificano endpoint sensibili—come conferme di pagamento, approvazioni amministrative o schermate di completamento della MFA—e tentano di accedervi direttamente, saltando i passaggi intermedi obbligatori. Questa vulnerabilità si verifica quando la logica lato server non riesce a mantenere una macchina a stati robusta, basandosi invece sull'assunto che l'utente segua il percorso guidato dall'interfaccia utente (UI). Un'esplosione riuscita può portare a gravi fallimenti della logica di business (Business Logic), tra cui transazioni non autorizzate, escalation dei privilegi (Privilege Escalation) e l'aggiramento dei meccanismi di verifica dell'identità.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se l'aggiramento consente transazioni finanziarie non autorizzate, escalation dei privilegi o accesso amministrativo.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### L'applicazione contiene processi di business multi-fase?
- [ ] No — l'applicazione consiste solo di azioni a richiesta singola  
- [ ] Sì — **sono** presenti processi multi-fase (es. carrelli della spesa, moduli wizard, registrazione)  

### La sequenza dei passaggi è applicata rigorosamente dal server?
- [ ] Sì — il tracciamento dello stato lato server garantisce che i passaggi **non possano** essere saltati  
- [ ] Sì — i controlli lato client suggeriscono una sequenza, ma il server **non** convalida l'avanzamento  
- [ ] No — l'applicazione **non** impone alcun ordine specifico di operazioni  

### Un attaccante può raggiungere la fase finale di un processo richiedendo direttamente l'URL o l'endpoint finale?
- [ ] No — l'accesso diretto ai passaggi finali **non è possibile** senza il completamento preventivo  
- [ ] Sì — i passaggi finali (es. `/api/v1/checkout/complete`) **possono** essere raggiunti tramite richiesta diretta  

### L'applicazione verifica che i dati prerequisiti dei passaggi precedenti siano presenti e validi?
- [ ] Sì — il server convalida tutti i dati dei passaggi precedenti prima di procedere  
- [ ] No — il server **è** vulnerabile al "salto" di passaggi in cui i dati sono attesi ma non verificati  

### I controlli di sicurezza come la MFA o la verifica dell'identità possono essere aggirati manipolando il workflow?
- [ ] No — i controlli critici **non possono** essere elusi tramite la manipolazione del flusso  
- [ ] Sì — l'aggiramento della MFA o dei controlli di identità **è possibile** saltando al passaggio successivo alla verifica  

---