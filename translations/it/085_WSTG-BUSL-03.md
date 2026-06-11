## WSTG-BUSL-03 — Test dei controlli di integrità

Il test dei controlli di integrità si concentra sulla verifica che l'applicazione garantisca che i dati rimangano inalterati e affidabili durante il passaggio attraverso vari processi aziendali o tra il client e il server. Gli aggressori mirano a questa logica manipolando parametri critici — come i prezzi dei prodotti, le quantità, i privilegi utente o gli stati delle transazioni — all'interno di richieste HTTP, campi nascosti o cookie. Se l'applicazione manca di verifica lato server o di robusti controlli di integrità come Hashing o HMAC, un avversario può manipolare questi valori per commettere frodi, effettuare un'escalation dei propri privilegi o aggirare i vincoli aziendali previsti. Questo test è fondamentale per qualsiasi applicazione che gestisca transazioni finanziarie, transizioni di stato sensibili o flussi di lavoro multi-fase in cui l'output di una fase funge da input affidabile per la successiva.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se il fallimento dell'integrità consente furti finanziari, escalation di privilegi non autorizzata o la modifica di record persistenti.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### I parametri aziendali sensibili sono esposti alla manipolazione lato client?
- [ ] No — i valori sensibili sono gestiti esclusivamente lato server  
- [ ] Sì — valori come prezzo, quantità o flag di stato **sono** esposti ma crittografati  
- [ ] Sì — i valori **sono** esposti in chiaro all'interno di campi nascosti, parametri URL o cookie  

### Viene applicato un meccanismo di integrità (es. HMAC, Checksum) ai parametri controllati dal client?
- [ ] Sì — un controllo di integrità robusto **viene applicato** e verificato ad ogni richiesta  
- [ ] Sì — viene applicato un controllo di integrità ma utilizza un algoritmo debole/prevedibile o un segreto hardcoded  
- [ ] No — non viene applicato alcun controllo di integrità ai parametri sensibili  

### Il controllo di integrità può essere aggirato o ricalcolato da un aggressore?
- [ ] No — la verifica della firma è rigorosamente applicata e l'aggiramento **non è possibile**  
- [ ] Sì — la verifica della firma può essere aggirata omettendo il parametro della firma  
- [ ] Sì — la firma **può** essere ricalcolata perché la chiave segreta o la logica sono esposte/deboli  

### L'applicazione esegue la ri-validazione dei dati nel backend rispetto a una fonte affidabile?
- [ ] Sì — il server ri-valida tutti i valori rispetto al database e l'aggiramento **non è possibile**  
- [ ] Sì — il server esegue una validazione parziale ma l'aggiramento **è possibile** attraverso casi limite specifici  
- [ ] No — il server si fida dei dati forniti dal client senza verifica nel backend *(Critico)*  

### È possibile manipolare lo stato di un processo multi-fase?
- [ ] No — lo stato della sessione è gestito lato server e non può essere manipolato  
- [ ] Sì — le informazioni di stato vengono passate tramite il client e la manipolazione **è possibile** per saltare passaggi o ripetere azioni  

---