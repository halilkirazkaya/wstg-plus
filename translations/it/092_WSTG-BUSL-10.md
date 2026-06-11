## WSTG-BUSL-10 — Test delle Funzionalità di Pagamento

Il test delle funzionalità di pagamento consiste nell'identificare difetti della logica di business (business logic flaws) che permettono a un attaccante di bypassare i payment gateway, manipolare i totali degli ordini o falsificare (spoof) i segnali di transazione riuscita. Queste vulnerabilità risiedono tipicamente nella comunicazione tra l'applicazione, il browser del client e i processori di pagamento di terze parti come Stripe, PayPal o sistemi di contabilità interna. Un attaccante potrebbe tentare di modificare i parametri del prezzo in transito, inviare quantità negative per ridurre il costo totale o eseguire il replay di callback di transazioni riuscite per evadere gli ordini senza un pagamento effettivo. Uno sfruttamento (exploit) riuscito porta a perdite finanziarie dirette, discrepanze nell'inventario e alla potenziale compromissione dell'integrità dell'e-commerce dell'applicazione.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### L'importo della transazione viene validato lato server prima dell'elaborazione?
- [ ] Sì — l'importo è rigorosamente validato rispetto al database dei prodotti lato server *(Più sicuro)*  
- [ ] Sì — l'importo è validato ma è possibile un bypass della logica tramite parameter pollution o currency switching  
- [ ] No — l'importo della transazione **può** essere modificato nella richiesta lato client e viene accettato dal backend  

### La quantità degli articoli può essere manipolata per ottenere un totale pari a zero o negativo?
- [ ] No — le quantità negative o pari a zero vengono rifiutate dalla logica di validazione dell'applicazione  
- [ ] Sì — le quantità negative sono accettate nel carrello ma il totale viene corretto al checkout  
- [ ] Sì — le quantità negative **possono** essere utilizzate per ridurre il totale complessivo dell'ordine o ottenere un credito *(Critica)*  

### L'applicazione gestisce in modo sicuro le callback o i webhook dei gateway di pagamento?
- [ ] Sì — le callback richiedono una firma crittografica valida che **non può** essere falsificata (spoofed)  
- [ ] Sì — le firme vengono controllate ma il segreto è debole o la verifica **può** essere bypassata  
- [ ] No — l'applicazione si affida a callback non autenticate o non verificate per confermare lo stato del pagamento  

### L'applicazione è vulnerabile a race conditions durante la fase di checkout o di pagamento?
- [ ] No — l'integrità transazionale e i meccanismi di blocco (locking) del database sono **abilitati**  
- [ ] Sì — le race conditions **sono possibili** ma non influiscono sul prezzo finale o sull'evasione dell'ordine  
- [ ] Sì — le richieste concorrenti **possono** portare a più evasioni per un singolo pagamento o al "double-spending" di carte regalo  

### I codici sconto o i punti fedeltà sono soggetti a manipolazione o riutilizzo?
- [ ] No — i codici sono validati per un utilizzo singolo e i vincoli logici sono **applicati**  
- [ ] Sì — i codici possono essere riutilizzati o applicati ad articoli non idonei, ma l'impatto è limitato  
- [ ] Sì — gli attaccanti **possono** applicare più codici in conflitto o bypassare la scadenza e i limiti di utilizzo  

---