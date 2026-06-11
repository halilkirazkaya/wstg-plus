## WSTG-INPV-09 — Testing for XPath Injection

L'XPath Injection si verifica quando un'applicazione utilizza informazioni fornite dall'utente per costruire una query XPath per dati XML, consentendo a un attaccante di interferire con la logica della query. Iniettando una sintassi specifica come apici singoli, parentesi quadre e operatori logici, un attaccante può navigare nella struttura del documento XML, bypassare l'autenticazione o esfiltrare nodi di dati sensibili. Questa vulnerabilità si trova comunemente nei web service, nelle interfacce di gestione della configurazione e nei sistemi legacy che si affidano a data store basati su XML anziché ai tradizionali database SQL. Dal punto di vista di un attaccante, questa viene spesso sfruttata utilizzando tecniche boolean-based o error-based per mappare sistematicamente l'albero XML e recuperare valori nascosti dal backend.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Strumenti:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Gli input dell'utente utilizzati per costruire query XPath sono correttamente sanificati o parametrizzati?
- [ ] Sì — gli input **non** sono utilizzati in query XPath o sono rigorosamente parametrizzati  
- [ ] Sì — **è applicata** una validazione/codifica dell'input robusta e il bypass **non è possibile**  
- [ ] Sì — **è applicata** una certa validazione ma il bypass **è possibile** utilizzando caratteri codificati  
- [ ] No — l'input dell'utente è concatenato direttamente nelle query XPath *(Critico)*  

### L'applicazione rivela dettagli strutturali XML/XPath tramite messaggi di errore?
- [ ] No — vengono mostrate pagine di errore generiche e **non** vengono trapelati dettagli XML  
- [ ] Sì — i messaggi di errore rivelano la presenza di elaborazione XML ma **nessun** dettaglio sul percorso  
- [ ] Sì — messaggi di errore dettagliati rivelano la sintassi XPath, i nomi dei nodi o i percorsi dei file  

### È possibile bypassare la logica di autenticazione o autorizzazione utilizzando la logica XPath?
- [ ] No — la logica di autenticazione **non** si affida a XML/XPath o è implementata in modo sicuro  
- [ ] Sì — i controlli di login o dei permessi possono essere bypassati utilizzando payload basati sulla logica come `' or 1=1 or '`  

### La struttura del documento XML o i dati possono essere enumerati tramite blind injection?
- [ ] No — l'applicazione **non** è suscettibile a inferenze boolean-based o time-based  
- [ ] Sì — i nomi dei nodi e i dati **possono** essere esfiltrati utilizzando l'inferenza boolean-based  
- [ ] Sì — **è possibile** il traversal completo dell'albero XML e l'estrazione dei dati  

---