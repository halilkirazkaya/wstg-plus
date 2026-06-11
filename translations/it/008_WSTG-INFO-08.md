## WSTG-INFO-08 — Fingerprinting del Framework dell'Applicazione Web

Il fingerprinting di un framework per applicazioni web consiste nell'identificare lo stack tecnologico specifico e le versioni del software utilizzate per sviluppare e gestire l'applicazione. Gli attaccanti analizzano gli header di risposta HTTP, i cookie specifici del framework, le strutture dei file prevedibili e gli artefatti JavaScript univoci per determinare se il target stia eseguendo piattaforme come React, Angular, Django o Spring. Questa fase di ricognizione (reconnaissance) è fondamentale poiché consente a un tester di restringere la superficie di attacco alle vulnerabilità note (CVE) e ai punti deboli di configurazione comuni specifici per quel framework. Comprendendo l'architettura sottostante, un attaccante può passare da test generici a strategie di exploitation altamente mirate.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Informativa |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Gli header di risposta HTTP espongono il nome o la versione del framework?
- [ ] No — gli header come `X-Powered-By` o `Server` **non** sono presenti o sono stati sanificati  
- [ ] Sì — il nome del framework è presente ma la versione **è nascosta**  
- [ ] Sì — sia il nome che la versione del framework **sono esposti**  

### I cookie o le strutture delle directory specifici del framework sono identificabili?
- [ ] No — i comuni artefatti del framework (es. percorsi `.js`, nomi dei cookie) sono offuscati o rinominati  
- [ ] Sì — i cookie specifici del framework (es. `JSESSIONID`, `PHPSESSID`, `csrftoken`) **sono** identificabili  
- [ ] Sì — le strutture delle directory (es. `/wp-content/`, `_next/static/`) **sono** visibili e confermano il framework  

### I messaggi di errore o i commenti nel codice sorgente rivelano dettagli sul framework?
- [ ] No — la gestione degli errori è generica e i commenti sono stati rimossi  
- [ ] Sì — gli stack trace specifici del framework **sono abilitati** e visibili nelle risposte  
- [ ] Sì — il codice sorgente HTML contiene commenti o tag metadata che identificano il framework  

### Esistono vulnerabilità note associate alla versione del framework identificata?
- [ ] No — il framework identificato è aggiornato e **non presenta** vulnerabilità pubbliche note  
- [ ] Sì — la versione del framework è obsoleta e **possono essere** identificate CVE specifiche  

---