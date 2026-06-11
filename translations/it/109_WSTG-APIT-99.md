## WSTG-APIT-99 — Testing GraphQL

GraphQL è un linguaggio di interrogazione per API che consente ai client di richiedere specifiche strutture di dati, ma le misconfiguration spesso portano a rischi di sicurezza significativi, inclusi information disclosure ed esaurimento delle risorse. Le vulnerabilità derivano tipicamente dall'abilitazione dell'introspection, dalla mancanza di limiti di profondità/complessità delle query e dalla broken object-level authorization (BOLA) all'interno delle funzioni resolver. Gli attaccanti sfruttano questi endpoint utilizzando l'introspection per mappare l'intero schema, sfruttando frammenti circolari per innescare un Denial of Service (DoS), o aggirando l'autorizzazione accedendo a campi non autorizzati tramite query nidificate. Il testing si concentra sugli endpoint `/graphql`, `/v1/graphql`, o `/graphiql` per garantire che l'implementazione applichi controlli di accesso rigorosi e la gestione delle risorse.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica* |

> *La gravità diventa Critica se la broken object-level authorization (BOLA) consente l'accesso non autorizzato a PII o mutation amministrative.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Strumenti:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### Il sistema di introspection di GraphQL è abilitato?
- [ ] No — l'introspection è **disabilitata** e lo schema non può essere mappato  
- [ ] Sì — l'introspection è **abilitata** ma richiede autenticazione amministrativa  
- [ ] Sì — l'introspection è **abilitata** e accessibile pubblicamente *(Alta)*  

### I limiti di risorse (profondità e complessità) sono applicati alle query?
- [ ] Sì — limiti rigorosi di profondità e complessità delle query sono **applicati** e imposti  
- [ ] Sì — i limiti sono **applicati** ma possono essere aggirati utilizzando alias o frammenti  
- [ ] No — non sono **applicati** limiti, consentendo query ricorsive e DoS  

### È presente Broken Object-Level Authorization (BOLA) nei resolver?
- [ ] No — l'autorizzazione è **applicata** a livello di campo e di oggetto per ogni query  
- [ ] Sì — l'autorizzazione è **applicata** ad alcuni campi ma gli oggetti sensibili sono esposti  
- [ ] Sì — l'autorizzazione **non è applicata**, consentendo l'accesso a qualsiasi oggetto tramite la manipolazione degli ID  

### Le mutation di GraphQL sono adeguatamente protette contro l'uso non autorizzato?
- [ ] No — le mutation **non sono** presenti nello schema  
- [ ] Sì — le mutation richiedono un'autenticazione valida e rigorosi controlli di autorizzazione  
- [ ] Sì — le mutation sono **possibili** per utenti non autenticati o mancano di autorizzazione  

### I messaggi di errore espongono dettagli sensibili sull'implementazione o sullo schema?
- [ ] No — i messaggi di errore sono generici e **non** rivelano la logica interna  
- [ ] Sì — i messaggi di errore rivelano tipi di database sottostanti o suggerimenti tramite "Did you mean...?"  
- [ ] Sì — stack trace completi e dati di configurazione sensibili sono **rivelati** nelle risposte  

---