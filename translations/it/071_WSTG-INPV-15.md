## WSTG-INPV-15 — HTTP Splitting Smuggling

Le vulnerabilità di HTTP Splitting e Smuggling derivano da discrepanze nel modo in cui i proxy frontend e i server backend interpretano ed elaborano i confini delle richieste HTTP, in particolare per quanto riguarda gli header `Content-Length` e `Transfer-Encoding`. Creando richieste ambigue, un attaccante può effettuare lo "smuggling" di una richiesta nascosta verso il backend o iniettare sequenze CRLF per suddividere una risposta (splitting), portando a cache poisoning, request hijacking o al bypass dei controlli di sicurezza. Questi difetti si manifestano tipicamente in ambienti complessi che utilizzano reverse proxy, load balancer o CDN che presentano logiche di parsing incoerenti. Dal punto di vista di un attaccante, ciò consente il reindirizzamento del traffico degli utenti, il furto di session token sensibili e l'esecuzione di azioni non autorizzate nel contesto delle sessioni di altri utenti.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Strumenti:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### L'ambiente è suscettibile al request smuggling tramite discrepanze CL.TE o TE.CL?
- [ ] No — i server frontend e backend gestiscono i confini delle richieste in modo coerente  
- [ ] Sì — esistono discrepanze ma lo sfruttamento **non è possibile** a causa delle mitigazioni dell'infrastruttura  
- [ ] Sì — lo smuggling CL.TE o TE.CL **è possibile**, consentendo a richieste nascoste di raggiungere il backend *(Alta)*  
- [ ] Sì — il TE.TE (doppia codifica/offuscamento) **è possibile** per bypassare i filtri del frontend *(Critica)*  

### È possibile ottenere l'HTTP Response Splitting tramite injection di CRLF negli header?
- [ ] No — l'applicazione sanitizza correttamente le sequenze CRLF in tutti gli input degli header  
- [ ] Sì — le sequenze CRLF sono riflesse negli header ma il response splitting **non è possibile**  
- [ ] Sì — l'injection di CRLF **è possibile**, consentendo l'header injection o il cache poisoning  

### I controlli di sicurezza (WAF/ACL) vengono bypassati utilizzando richieste "smuggled"?
- [ ] No — i controlli di sicurezza si applicano sia alle richieste esterne che a quelle "smuggled"  
- [ ] Sì — le richieste "smuggled" **possono** bypassare le regole WAF del frontend o le ACL basate su IP  

### È possibile dirottare le sessioni di altri utenti o reindirizzare il loro traffico?
- [ ] No — i flussi di richiesta/risposta sono isolati e non possono essere incrociati  
- [ ] Sì — il request smuggling **è possibile** e consente di catturare le richieste di altri utenti *(Critica)*  
- [ ] Sì — il response splitting **è possibile** e consente il cache poisoning lato browser o XSS  

---