## WSTG-ATHN-10 — Verifica di Meccanismi di Autenticazione più Deboli in Canali Alternativi

La verifica di meccanismi di autenticazione più deboli in canali alternativi comporta l'identificazione e l'analisi di percorsi secondari per l'accesso agli account—come API mobili, workflow di recupero password, interfacce di help desk o sottodomini legacy—che potrebbero non applicare lo stesso rigore di sicurezza del portale web principale. Questi canali alternativi spesso mancano di una robusta Multi-Factor Authentication (MFA), di un Rate Limiting rigoroso o di requisiti complessi per le password, creando un "anello debole" che compromette l'intero sistema di autenticazione. Gli attaccanti prendono di mira specificamente questi punti di ingresso trascurati per eseguire Credential Stuffing o bypassare la MFA sfruttando le discrepanze nel modo in cui le diverse interfacce gestiscono la verifica dell'identità. Garantire la parità tra tutte le superfici di autenticazione è essenziale per prevenire l'accesso non autorizzato e mantenere l'integrità della sessione e dei dati dell'utente.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Sono presenti e identificati canali di autenticazione alternativi?
- [ ] No — esiste solo un singolo canale di autenticazione  
- [ ] Sì — identificati canali multipli (es. API di app mobile, client desktop, portale legacy, servizi SOAP)  

### I canali alternativi applicano una parità di sicurezza rispetto al canale principale?
- [ ] Sì — tutti i canali impongono requisiti di complessità della password e MFA **identici**  
- [ ] Sì — i canali impongono la complessità della password, ma la MFA **non è richiesta** o **può** essere bypassata  
- [ ] No — i canali alternativi hanno requisiti di autenticazione **significativamente più deboli**  

### Il Rate Limiting e l'Account Lockout sono coerenti su tutti i canali?
- [ ] Sì — il Rate Limiting e il lockout sono **applicati coerentemente** su tutti gli endpoint  
- [ ] Sì — i controlli sono presenti ma il bypass **è possibile** su canali specifici (es. API mobile)  
- [ ] No — il Rate Limiting o il lockout **non sono applicati** ai percorsi di autenticazione secondari  

### La MFA può essere bypassata passando a un canale alternativo?
- [ ] No — la MFA è obbligatoria e **non può** essere bypassata, indipendentemente dal punto di ingresso  
- [ ] Sì — la MFA è richiesta sul portale web ma **non applicata** sulle API mobili o legacy  

### I flussi di recupero password o "password dimenticata" sono più deboli rispetto al login standard?
- [ ] No — i flussi di recupero utilizzano una verifica strong out-of-band e non espongono informazioni  
- [ ] Sì — i flussi di recupero utilizzano domande di sicurezza deboli che **possono** essere facilmente indovinate o ricercate  
- [ ] Sì — i flussi di recupero sono vulnerabili a Enumeration o **non richiedono** lo stesso livello di verifica di un login standard  

---