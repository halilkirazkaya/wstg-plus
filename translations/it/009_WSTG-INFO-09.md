## WSTG-INFO-09 — Fingerprint Web Application

Il fingerprinting di un'applicazione web è il processo di identificazione del framework specifico, del Content Management System (CMS) o dello stack tecnologico sottostante e delle relative versioni. Questa fase di ricognizione è fondamentale poiché consente a un utente malintenzionato di mappare i componenti identificati rispetto a vulnerabilità note (CVE) ed exploit pubblici, riducendo significativamente la superficie di attacco. Le informazioni vengono solitamente raccolte dagli header delle risposte HTTP, da percorsi di file univoci, nomi di cookie, metadati del codice sorgente HTML e hash crittografici di asset statici. Determinando accuratamente lo stack tecnologico, un attaccante può passare da una scansione automatizzata generica a uno sfruttamento altamente mirato delle debolezze specifiche della versione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Informativa |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Gli header delle risposte HTTP rivelano lo stack tecnologico o i numeri di versione?
- [ ] No — gli header sensibili vengono rimossi o contengono valori generici  
- [ ] Sì — gli header predefiniti come `X-Powered-By`, `Server` o `X-AspNet-Version` **sono** presenti  
- [ ] Sì — gli header rivelano versioni specifiche e obsolete del server web o del framework dell'applicazione  

### Il framework dell'applicazione o il CMS è identificabile attraverso percorsi di file univoci o convenzioni di denominazione?
- [ ] No — i percorsi dei file sono offuscati e **non** rivelano la tecnologia sottostante  
- [ ] Sì — le strutture di directory comuni (es. `/wp-content/`, `/_next/`, `/node_modules/`) **sono** visibili  
- [ ] Sì — file univoci come `robots.txt`, `humans.txt` o `sitemap.xml` contengono metadati specifici della tecnologia  

### Sono esposti numeri di versione specifici all'interno del codice sorgente HTML o negli asset statici?
- [ ] No — nessuna informazione sulla versione è presente nei commenti o nei parametri degli asset  
- [ ] Sì — sono presenti commenti HTML o meta tag (es. `<meta name="generator" content="...">`)  
- [ ] Sì — le stringhe di versione sono aggiunte ai nomi dei file CSS/JS (es. `jquery.js?v=1.12.4`) e **non** possono essere disabilitate  

### Le pagine di errore predefinite o le interfacce di gestione rivelano l'ambiente software?
- [ ] No — l'applicazione utilizza pagine di errore personalizzate e generiche per tutti i codici di stato  
- [ ] Sì — le pagine di errore predefinite per il server web o il framework **sono** abilitate e rivelano dati sulla versione  
- [ ] Sì — i portali di login amministrativi (es. `/wp-admin`, `/phpmyadmin`) **sono** accessibili e identificabili  

### I cookie rivelano il framework di gestione della sessione?
- [ ] No — i cookie di sessione utilizzano nomi generici o personalizzati  
- [ ] Sì — vengono utilizzati nomi di cookie specifici della tecnologia (es. `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`)  

---