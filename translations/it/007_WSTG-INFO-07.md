## WSTG-INFO-07 — Mappare i percorsi di esecuzione dell'applicazione

La mappatura dei percorsi di esecuzione comporta l'identificazione sistematica di tutti gli endpoint raggiungibili, dei flussi funzionali e dei rami decisionali all'interno di un'applicazione web. Questo processo è fondamentale per garantire che nessuna funzionalità "oscura" (dark)—come codice legacy, interfacce di debug o rotte API non documentate—rimanga nascosta ai controlli di sicurezza, poiché queste spesso mancano di controlli di sicurezza moderni. Gli attaccanti utilizzano una combinazione di spidering automatizzato ed esplorazione manuale per visualizzare la struttura dell'applicazione, scoprendo spesso vulnerabilità come accessi amministrativi non autorizzati o falle nella logica di business (business logic flaws) durante il processo. Correlando gli input a specifiche risposte lato server, i tester possono individuare logiche di elaborazione sensibili che richiedono test approfonditi e mirati per garantire una copertura robusta.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Informativo |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### L'applicazione è stata completamente indicizzata utilizzando il crawling automatizzato e manuale?
- [ ] Sì — la mappatura completa di tutte le risorse visibili e collegate **è stata completata**  
- [ ] Sì — il crawling automatizzato **è stato completato** ma l'esplorazione manuale è in sospeso  
- [ ] No — l'applicazione **non** è stata indicizzata  

### Gli endpoint nascosti o non documentati sono stati identificati tramite l'analisi JS o il brute-forcing delle directory?
- [ ] No — nessun percorso non documentato scoperto tramite l'analisi side-channel  
- [ ] Sì — percorsi nascosti scoperti ma **non** è possibile accedervi senza autorizzazione  
- [ ] Sì — gli endpoint non documentati **sono** accessibili e forniscono funzionalità sensibili *(Critico / Alto)*  

### I percorsi di esecuzione possono essere alterati tramite parametri o header non standard?
- [ ] No — parametri come `debug`, `test` o `admin` **non** influenzano il flusso di esecuzione  
- [ ] Sì — i parametri modificano l'output ma **non** eludono i controlli di sicurezza  
- [ ] Sì — i parametri che alterano il percorso **vengono applicati** e consentono di eludere la logica prevista  

### Il flusso della logica di business multi-step (es. autenticazione a più fattori, checkout) è stato mappato completamente?
- [ ] Sì — tutti gli stati e le transizioni nei processi multi-step **sono stati identificati**  
- [ ] No — le transizioni complesse della macchina a stati **non** possono essere mappate completamente  

### I file di documentazione API (Swagger/OpenAPI) o le mappe lato client (Source Maps) sono esposti?
- [ ] No — nessuna mappa architetturale o file di documentazione è esposto pubblicamente  
- [ ] Sì — la documentazione è presente ma **è protetta** da autenticazione  
- [ ] Sì — Swagger UI, OpenAPI JSON o JS Source Maps **sono abilitati** e accessibili pubblicamente  

---