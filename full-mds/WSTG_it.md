## WSTG-INFO-01 — Condurre la Ricerca e il Riconoscimento tramite Motori di Ricerca per Perdite di Informazioni

La scoperta tramite motori di ricerca (Search engine discovery) consiste nell'utilizzare motori di ricerca pubblici, pagine in cache e servizi di indicizzazione per identificare informazioni che l'organizzazione target ha esposto involontariamente. Gli attaccanti utilizzano operatori di ricerca avanzati (Google Dorks) e servizi di indicizzazione di terze parti per scoprire file sensibili, directory, portali di login, messaggi di errore e metadati che non dovrebbero essere accessibili pubblicamente. Questa fase di riconoscimento (reconnaissance) è critica perché non richiede alcuna interazione diretta con il target, rendendola virtualmente non rilevabile pur rivelando potenzialmente punti di ingresso di alto valore come pannelli di amministrazione esposti, file di configurazione, dump di database e documentazione interna.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Strumenti:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### I file o le directory sensibili sono indicizzati dai motori di ricerca?
- [ ] No — nessun contenuto sensibile trovato nei risultati dei motori di ricerca  
- [ ] Sì — i file di configurazione, i backup o i documenti interni **sono** indicizzati *(Critico)*  

### Il Google Dorking rivela interfacce amministrative o di login esposte?
- [ ] No — i pannelli di amministrazione e le pagine di login **non sono** indicizzati  
- [ ] Sì — i pannelli di amministrazione **sono** indicizzati ma richiedono un'autenticazione a più fattori forte  
- [ ] Sì — i pannelli di amministrazione **sono** indicizzati e accessibili pubblicamente **senza** controlli sufficienti  

### Le versioni in cache delle pagine espongono dati sensibili che nel frattempo sono stati rimossi?
- [ ] No — nessun dato sensibile trovato negli snapshot in cache  
- [ ] Sì — le pagine in cache contengono credenziali, indirizzi IP interni o informazioni sensibili  

### Il file `robots.txt` sta involontariamente rivelando percorsi sensibili?
- [ ] No — `robots.txt` **non** rivela strutture di directory sensibili  
- [ ] Sì — `robots.txt` elenca percorsi sensibili che facilitano il riconoscimento dell'attaccante (reconnaissance)  
- [ ] Non esiste alcun file `robots.txt`  

### I servizi di terze parti (Shodan, Censys, Wayback Machine) rivelano esposizioni storiche o attuali?
- [ ] No — nessun risultato significativo dai servizi di indicizzazione di terze parti  
- [ ] Sì — gli snapshot storici o le scansioni dei servizi rivelano metadati sensibili o banner dei servizi

---

## WSTG-INFO-02 — Fingerprinting del Web Server

Il fingerprinting del web server è il processo di identificazione del tipo di software specifico, della versione e del sistema operativo sottostante di un web server target. Gli attaccanti eseguono questa attività di ricognizione per identificare vulnerabilità note, come CVE non patchate o debolezze di configurazione, specifiche per determinate versioni di Apache, Nginx, IIS o altre tecnologie server. Ciò si ottiene tipicamente analizzando gli header di risposta HTTP (es. `Server`, `X-Powered-By`), esaminando comportamenti unici del protocollo o osservando informazioni trapelate attraverso pagine di errore e file predefiniti. Un fingerprinting efficace consente a un attaccante di personalizzare la propria strategia di Exploit, passando da una scansione generica ad attacchi ad alta probabilità contro difetti architetturali noti.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Informativo / Basso* |

> *La gravità diventa Alta se il fingerprinting rivela una versione del server obsoleta o a fine del ciclo di vita (EOL) con vulnerabilità critiche note.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Tool:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Sono presenti stringhe identificative negli header di risposta HTTP?
- [ ] No — gli header `Server` e `X-Powered-By` **non sono presenti** o contengono valori generici  
- [ ] Sì — gli header rivelano il software del server ma **non** i numeri di versione specifici  
- [ ] Sì — gli header rivelano il software specifico del server e i numeri di versione **esatti**  

### Le pagine di errore predefinite o le risposte generate dal sistema rivelano dettagli del server?
- [ ] No — vengono utilizzate pagine di errore personalizzate che **non** divulgano informazioni sul server  
- [ ] Sì — le pagine di errore predefinite rivelano il nome del software del server e/o la versione  
- [ ] Sì — le pagine di errore rivelano dettagli del server, la versione e il **sistema operativo sottostante**  

### La versione del server è divulgata tramite file predefiniti o strutture di directory?
- [ ] No — i file di installazione predefiniti, i manuali e gli script di test sono stati **rimossi**  
- [ ] Sì — i file predefiniti (es. `info.php`, `manual/`, `test.html`) sono **presenti** e divulgano dati sulla versione  

### È possibile dedurre accuratamente il tipo di server tramite l'analisi di comportamenti univoci?
- [ ] No — le risposte del server sono normalizzate e il fingerprinting basato sul comportamento **non è possibile**  
- [ ] Sì — comportamenti univoci (es. ordinamento degli header, codici di errore specifici o particolarità dello stack TCP/IP) **consentono** l'identificazione del server  

### La versione del server identificata è attualmente supportata e patchata?
- [ ] Sì — la versione identificata è attuale e **supportata** dal fornitore  
- [ ] No — la versione identificata è **obsoleta** o a **fine del ciclo di vita (EOL)** con vulnerabilità note

---

## WSTG-INFO-03 — Revisione dei Metafile del Webserver per Fughe di Informazioni (Information Leakage)

I metafile del server web come `robots.txt`, `sitemap.xml` e `security.txt` hanno lo scopo di guidare i crawler e fornire metadati amministrativi, ma spesso rivelano inavvertitamente strutture di directory sensibili o endpoint nascosti. Gli attaccanti analizzano questi file per enumerare la superficie di attacco dell'applicazione, identificando le direttive "Disallow" che spesso puntano a pannelli amministrativi non collegati, directory di backup o ambienti di sviluppo. Oltre alle istruzioni per i motori di ricerca, i file nella directory `.well-known/` o file come `humans.txt` possono rivelare stack tecnologici, informazioni sugli sviluppatori o contatti di sicurezza dell'organizzazione. Una revisione sistematica di questi file permette a un tester di ottenere approfondimenti strutturali e tecnici senza eseguire una scoperta tramite Brute Force aggressivo.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Informativa* |

> *La severità diventa Media se i metafile rivelano interfacce amministrative non autenticate o backup di configurazioni sensibili.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Il file `robots.txt` esiste ed espone directory sensibili?
- [ ] No — `robots.txt` **non** esiste  
- [ ] Sì — `robots.txt` esiste ma contiene solo percorsi pubblici standard  
- [ ] Sì — `robots.txt` rivela percorsi di directory sensibili o interfacce amministrative  
- [ ] Sì — `robots.txt` rivela credenziali o versioni specifiche del software nei commenti  

### È presente un file `sitemap.xml` che rivela la struttura nascosta dell'applicazione?
- [ ] No — `sitemap.xml` **non** esiste  
- [ ] Sì — `sitemap.xml` è presente ma elenca solo URL pubblici  
- [ ] Sì — `sitemap.xml` elenca endpoint non collegati o risorse esclusivamente interne a cui **è possibile** accedere  

### I metafile di sicurezza e organizzativi espongono dettagli tecnici?
- [ ] No — nessun file di metadati trovato nella root o in `.well-known/`  
- [ ] Sì — `security.txt` o `humans.txt` sono presenti ma contengono informazioni standard  
- [ ] Sì — i metafile rivelano nomi host interni, identità degli sviluppatori o dettagli dello stack tecnologico che **possono** agevolare il Social Engineering o ulteriori attacchi  

### Sono presenti metafile legacy o specifici del framework?
- [ ] No — nessun file specifico del framework (es. `info.php`, `composer.json`, `package.json`) trovato  
- [ ] Sì — file come `README.md`, `CHANGELOG.txt` o `.DS_Store` sono presenti ma bonificati  
- [ ] Sì — sono presenti file specifici del framework o della versione che **permettono** un Fingerprinting tecnologico preciso

---

## WSTG-INFO-04 — Enumerazione delle Applicazioni sul Web Server

L'enumerazione delle applicazioni è il processo di identificazione di tutte le applicazioni e i servizi web ospitati su un singolo web server o indirizzo IP. Poiché i moderni web server ospitano spesso molteplici host virtuali, ambienti di staging legacy o interfacce amministrative su porte e percorsi non standard, la mancata identificazione di questi asset nascosti lascia una parte significativa della superficie di attacco non testata. Gli attaccanti utilizzano tecniche come i DNS zone transfers, il brute-forcing degli host virtuali e i reverse IP lookups per scoprire questi punti di ingresso dimenticati o oscurati, che sono spesso meno sicuri dell'applicazione di produzione principale.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Informativa / Bassa |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Strumenti:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Sono associati più indirizzi IP o alias DNS all'infrastruttura di destinazione?
- [ ] No — esistono solo un singolo IP e un record A  
- [ ] Sì — sono stati identificati più indirizzi IP o alias, ampliando il perimetro  

### Il web server ospita più Host Virtuali (vhosts) sullo stesso IP?
- [ ] No — solo il dominio principale è servito dal server  
- [ ] Sì — sono stati identificati host virtuali aggiuntivi, ma l'accesso **non è possibile** (es. 403 Forbidden)  
- [ ] Sì — sono stati identificati e risultano accessibili host virtuali nascosti, di sviluppo o di staging  

### Le applicazioni sono ospitate su porte non standard?
- [ ] No — sono aperte solo le porte standard (80/443)  
- [ ] Sì — le porte non standard sono aperte ma **non** ospitano applicazioni web  
- [ ] Sì — sono state identificate applicazioni web aggiuntive su porte non standard (es. 8080, 8443, 9000)  

### Sono mappate applicazioni distinte su percorsi URI specifici?
- [ ] No — una singola applicazione serve l'intera directory radice (root)  
- [ ] Sì — sono state scoperte più applicazioni tramite l'enumerazione delle directory (es. `/api`, `/portal`, `/backup`)  

### I servizi di ricognizione di terze parti rivelano applicazioni storiche o secondarie?
- [ ] No — nessun reperto significativo da Shodan, Censys o Wayback Machine  
- [ ] Sì — snapshot storici o scansioni dei servizi rivelano applicazioni attualmente attive ma non collegate direttamente

---

## WSTG-INFO-05 — Revisione del Contenuto della Pagina Web per Information Leakage

La revisione del contenuto della pagina web comporta l'ispezione manuale e automatizzata delle risposte dell'applicazione, inclusi i file HTML, JavaScript e CSS, per identificare informazioni sensibili involontariamente esposte agli utenti. Gli attaccanti esaminano il codice sorgente client-side alla ricerca di commenti degli sviluppatori, campi di input nascosti, percorsi interni del server, chiavi API (hardcoded API keys) e informazioni di debugging che possono favorire successive fasi di exploitation. Questa fuga di informazioni (leakage) si verifica spesso quando gli sviluppatori dimenticano di rimuovere metadati o codice di debug prima del passaggio in produzione, rivelando potenzialmente difetti logici o dettagli dell'infrastruttura backend. Dal punto di vista di un attaccante, questo rappresenta un metodo a basso sforzo per mappare la struttura interna dell'applicazione e scoprire punti di ingresso trascurati senza attivare avvisi di sicurezza o interagire con la logica backend del server.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media* |

> *La severità diventa Alta se vengono scoperte credenziali hardcoded sensibili, chiavi private o variabili d'ambiente interne.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Strumenti:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Sono presenti commenti degli sviluppatori contenenti informazioni sensibili nel codice sorgente?
- [ ] No — nessun commento trovato o presenti solo commenti generici e non sensibili  
- [ ] Sì — i commenti esistono ma **non** contengono logica o dati sensibili  
- [ ] Sì — i commenti **rivelano** percorsi interni, query SQL o istruzioni amministrative  
- [ ] Sì — i commenti **rivelano** credenziali o note sensibili degli sviluppatori *(Alta)*  

### I campi di input HTML nascosti contengono dati sensibili o informazioni di stato?
- [ ] No — nessun campo nascosto trovato o contengono solo token non sensibili  
- [ ] Sì — i campi nascosti sono usati per gestire lo stato ma sono **cifrati** o **firmati**  
- [ ] Sì — i campi nascosti contengono password in **testo in chiaro (plaintext)**, ruoli utente o ID interni  

### Sono presenti segreti hardcoded, chiavi API o indirizzi IP interni nei file JavaScript?
- [ ] No — nessuna stringa sensibile o segreto trovato nei file JS  
- [ ] Sì — i file JS contengono chiavi API pubbliche con restrizioni **adeguate**  
- [ ] Sì — i file JS contengono chiavi API **non protette**, IP interni o endpoint privati  
- [ ] Sì — i file JS contengono credenziali amministrative **hardcoded** o chiavi private *(Critica)*  

### L'applicazione rivela versioni del framework o percorsi di file interni nel sorgente?
- [ ] No — le informazioni sul framework e i percorsi dei file sono **minimizzati (minified)** o **offuscati**  
- [ ] Sì — i nomi e le versioni del framework sono **visibili** ma aggiornati (patched)  
- [ ] Sì — percorsi di file interni o percorsi assoluti del server sono **esposti** nel codice sorgente  

### Il codice sorgente è incline alla perdita di dati a causa della mancanza di minimizzazione o offuscamento?
- [ ] No — il codice di produzione è **completamente minimizzato** e privo di commenti  
- [ ] Sì — il codice è minimizzato ma i commenti **rimangono** nel sorgente  
- [ ] Sì — le source maps (file `.map`) sono **accessibili pubblicamente**, consentendo la ricostruzione completa del codice sorgente

---

## WSTG-INFO-06 — Identificazione dei punti di ingresso dell'applicazione

L'identificazione dei punti di ingresso dell'applicazione comporta la mappatura dell'intera superficie di attacco enumerando tutti i modi possibili in cui un utente o un sistema esterno può interagire con l'applicazione. Questo processo include l'individuazione di tutti gli URL, endpoint API, parametri (GET, POST, basati su cookie), header HTTP e protocolli specializzati come WebSockets o gRPC. Per un penetration tester, questa fase è fondamentale perché le vulnerabilità si trovano spesso in "shadow" API non documentate, endpoint legacy o strutture di parametri complesse che sono state trascurate durante lo sviluppo. Un'enumerazione approfondita assicura che nessun componente dell'applicazione rimanga una black box non testata, impedendo così agli aggressori di trovare percorsi non monitorati verso il sistema.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Informativo |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Tutti i parametri GET e POST sono stati enumerati all'interno dell'applicazione?
- [ ] Sì — l'enumerazione completa tramite crawling automatico e manuale **è stata completata**  
- [ ] Sì — sono stati identificati solo i parametri comuni tramite crawling standard  
- [ ] No — i parametri sono in gran parte **non identificati** o non testati  

### I parametri non documentati o nascosti (ad es. debug, admin) sono stati ricercati tramite fuzzing?
- [ ] No — l'individuazione dei parametri nascosti **non è necessaria** per questo perimetro specifico  
- [ ] Sì — il fuzzing dei parametri (ad es. utilizzando `Arjun`) **è stato eseguito** senza alcun risultato  
- [ ] Sì — i parametri nascosti **sono stati scoperti** e mappati per ulteriori test  
- [ ] No — l'individuazione dei parametri nascosti **non è stata** eseguita  

### Sono stati identificati tutti i metodi HTTP supportati (PUT, DELETE, PATCH, ecc.) per ogni endpoint?
- [ ] Sì — l'individuazione dei metodi **è applicata** a tutti gli endpoint individuati  
- [ ] Sì — l'individuazione dei metodi **è applicata** solo agli endpoint sensibili o di alto interesse  
- [ ] No — sono stati identificati solo i metodi standard GET e POST  

### I punti di ingresso non standard come WebSockets, gRPC o header personalizzati sono stati mappati?
- [ ] No — l'applicazione **non utilizza** protocolli non standard o header personalizzati  
- [ ] Sì — tutti i punti di ingresso specifici del protocollo e gli header personalizzati **sono stati identificati**  
- [ ] No — esistono punti di ingresso non standard ma **non sono stati mappati**  

### La superficie delle API (inclusi gli endpoint con versione come /v1/ o /v2/) è stata completamente mappata?
- [ ] No — l'applicazione **non espone** un'API  
- [ ] Sì — tutte le versioni e gli endpoint delle API **sono stati identificati** e documentati  
- [ ] Sì — è stata identificata solo l'attuale versione di produzione dell'API  
- [ ] No — gli endpoint API **rimangono non mappati** o solo parzialmente individuati

---

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

## WSTG-INFO-10 — Mappatura dell'Architettura dell'Applicazione

La mappatura dell'architettura dell'applicazione consiste nell'identificare le tecnologie sottostanti, i componenti dell'infrastruttura e i percorsi dei flussi di dati che supportano l'applicazione web. Attraverso l'analisi degli header delle risposte HTTP, dei messaggi di errore, dei formati dei cookie e delle estensioni dei file, i tester possono ricostruire uno schema dei web server, degli application server, dei database e delle integrazioni di terze parti in uso. Questa conoscenza è fondamentale per personalizzare gli attacchi successivi, in quanto consente la selezione di platform-specific exploit e l'identificazione di potenziali colli di bottiglia o dispositivi intermedi misconfigurati come load balancer e WAF. Un attaccante sfrutta questa mappa strutturale per passare da una scansione generica a uno sfruttamento mirato dello stack software specifico e dei suoi componenti interconnessi.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Informativa / Bassa* |

> *La gravità diventa Media se vengono divulgati la topologia della rete interna o indirizzi IP privati.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Le tecnologie del web server e dell'application server possono essere identificate con precisione?
- [ ] No — nessuna identificazione **possibile** a causa di hardening o offuscamento efficaci  
- [ ] Sì — il tipo di tecnologia è noto ma le versioni specifiche **non possono** essere determinate  
- [ ] Sì — la tecnologia e le versioni specifiche **sono** completamente divulgate tramite header o firme dei file *(Informativa)*  

### I dispositivi intermedi (WAF, Load Balancer, Proxy) sono rilevabili nel percorso di comunicazione?
- [ ] No — non viene rilevato alcun intermediario o sono completamente trasparenti  
- [ ] Sì — l'esistenza è sospettata tramite tempistiche o comportamento, ma l'identità **non è** confermata  
- [ ] Sì — prodotti specifici (es. Cloudflare, Nginx, F5) **sono** identificati tramite header univoci o comportamento  

### L'applicazione espone informazioni sulla propria struttura delle directory interne o sulla topologia di rete?
- [ ] No — i percorsi interni e gli IP **non sono** divulgati  
- [ ] Sì — i percorsi interni sono trapelati nei messaggi di errore o negli stack trace, ma gli IP interni **non lo sono**  
- [ ] Sì — sia le strutture delle directory interne che gli indirizzi IP privati **sono** divulgati  

### Il tipo e la versione del database di backend possono essere dedotti dal comportamento dell'applicazione?
- [ ] No — i dettagli del database **non possono** essere determinati  
- [ ] Sì — il tipo di database (es. PostgreSQL, MSSQL) viene dedotto tramite messaggi di errore o comportamento  
- [ ] Sì — il tipo e la versione del database **sono** esplicitamente divulgati nel corpo della risposta o negli header  

### L'applicazione è ospitata su fornitori di servizi cloud identificabili o CDN di terze parti specifiche?
- [ ] No — l'infrastruttura di hosting **non è** identificabile  
- [ ] Sì — il provider cloud (AWS, Azure, GCP) è identificato ma i servizi specifici **non lo sono**  
- [ ] Sì — servizi cloud specifici (es. bucket S3, Lambda, Azure Functions) **sono** mappati e raggiungibili

---

## WSTG-CONF-01 — Test della configurazione dell'infrastruttura di rete

Il test della configurazione dell'infrastruttura di rete comporta l'identificazione di debolezze nei componenti server e di rete sottostanti che supportano l'applicazione web. Gli attaccanti prendono di mira servizi malconfigurati, protocolli obsoleti e porte aperte non necessarie per ottenere un punto d'appoggio, muoversi lateralmente o eseguire attività di reconnaissance. Le evidenze comuni includono versioni TLS insicure, banner di servizio eccessivamente dettagliati (verbose) e interfacce amministrative esposte che dovrebbero essere limitate alle reti interne. Sfruttando queste falle a livello di infrastruttura, un avversario può compromettere la riservatezza dei dati in transito o ottenere l'accesso non autorizzato al sistema operativo host.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Stato del Test** | Non eseguito |
| **Gravità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Tool:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Sono presenti porte aperte non necessarie o servizi esposti sull'host dell'applicazione?
- [ ] No — solo le porte richieste (es. 443) sono **accessibili**  
- [ ] Sì — sono esposti servizi aggiuntivi, ma seguono una configurazione **sicura**  
- [ ] Sì — servizi non essenziali (es. FTP, Telnet, SMB) sono **esposti** pubblicamente  

### I banner o gli header dei servizi espongono informazioni sensibili sulla versione?
- [ ] No — i banner sono rimossi o generici  
- [ ] Sì — i banner rivelano il software del server (es. Apache/nginx) ma **non** i numeri di versione  
- [ ] Sì — i banner descrittivi (verbose) rivelano versioni software specifiche, facilitando l'**exploitation**  

### La configurazione TLS segue le moderne best practice di sicurezza?
- [ ] Sì — solo TLS 1.2/1.3 sono **abilitati** con cipher suites forti  
- [ ] Sì — i protocolli legacy (TLS 1.0/1.1) sono **abilitati**  
- [ ] No — cipher deboli o protocolli insicuri (SSLv2/v3) sono **abilitati**  

### Le interfacce amministrative o di gestione sono limitate a reti autorizzate?
- [ ] Sì — le interfacce (es. SSH, RDP, pannelli di controllo) **non sono accessibili** dalla rete internet pubblica  
- [ ] No — le interfacce sono pubblicamente **accessibili** ma richiedono l'autenticazione a più fattori (MFA)  
- [ ] No — le interfacce sono pubblicamente **accessibili** con autenticazione a fattore singolo o credenziali di default

---

## WSTG-CONF-02 — Test Application Platform Configuration

Il test della configurazione della piattaforma applicativa prevede l'audit del web server sottostante, dell'application server e delle impostazioni del framework per garantire che siano protetti contro exploit comuni. Gli attaccanti cercano attivamente credenziali di default, vulnerabilità della piattaforma non patchate e interfacce amministrative esposte per ottenere accessi non autorizzati o eseguire codice. Le configurazioni errate spesso comportano la divulgazione di variabili d'ambiente sensibili, percorsi di sistema interni o la presenza di applicazioni di esempio non necessarie che espandono la superficie di attacco. Da una prospettiva professionale, una piattaforma configurata in modo errato funge da punto di ingresso ad alta probabilità per ottenere l'accesso iniziale all'ambiente target.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se le interfacce amministrative sono accessibili con credenziali di default o se gli script di esempio consentono la Remote Code Execution.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Le credenziali o gli account di default sono attivi sulla piattaforma?
- [ ] No — tutti gli account di default sono **disabilitati** o le password sono state modificate  
- [ ] Sì — esistono account di default ma **non sono accessibili** dalla rete esterna  
- [ ] Sì — gli account di default sono **attivi** e accessibili tramite internet *(Critico)*  

### La piattaforma divulga informazioni sensibili sulla versione tramite header o pagine di errore?
- [ ] No — le stringhe di versione sono **nascoste** o generiche  
- [ ] Sì — informazioni dettagliate sulla versione **vengono divulgate** negli HTTP headers (es., `Server`, `X-Powered-By`)  
- [ ] Sì — stack traces completi e percorsi di sistema interni **vengono esposti** in messaggi di errore dettagliati  

### Le interfacce amministrative o di gestione sono correttamente limitate?
- [ ] No — le interfacce amministrative **non sono esposte**  
- [ ] Sì — le interfacce esistono ma sono **limitate** tramite IP allowlisting o Multi-Factor Authentication  
- [ ] Sì — le interfacce (es., `/admin`, `/manager`, `/console`) sono **accessibili pubblicamente** con autenticazione a singolo fattore  

### Sono presenti file di esempio, script o documentazione sul server di produzione?
- [ ] No — tutti i file non essenziali sono stati **rimossi**  
- [ ] Sì — file di esempio o documentazione **sono presenti** ma non espongono informazioni sensibili  
- [ ] Sì — script di esempio (es., `info.php`, `examples/`) **sono presenti** e forniscono un vettore di attacco diretto  

### Il server è configurato per supportare metodi HTTP insicuri?
- [ ] No — solo `GET` e `POST` sono **abilitati**  
- [ ] Sì — metodi potenzialmente rischiosi come `PUT`, `DELETE` o `TRACE` sono **abilitati** ma limitati  
- [ ] Sì — i metodi rischiosi sono **abilitati** e consentono la modifica non autorizzata di file o il furto di credenziali

---

## WSTG-CONF-03 — Verifica della Gestione delle Estensioni dei File per Informazioni Sensibili

L'attività di testing sulla gestione delle estensioni dei file consiste nell'identificare se il server web o l'application server riveli informazioni sensibili distribuendo file che dovrebbero essere limitati o eseguiti. Gli attaccanti cercano frequentemente file di backup, file di configurazione e frammenti di codice sorgente (ad es. `.bak`, `.old`, `.env`, `.inc`) che potrebbero essere rimasti nella web root e serviti come testo in chiaro a causa di una mancanza di mappatura specifica dei handler. Questa vulnerabilità è rilevante poiché può portare all'esposizione di credenziali di database, chiavi API e logica di business interna, facilitando uno sfruttamento più profondo dell'infrastruttura. Queste esposizioni si verificano solitamente in directory pubbliche, cartelle di caricamento (upload) o tramite impostazioni dei tipi MIME configurate in modo errato sul server.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se sono accessibili file di configurazione contenenti credenziali o l'intero codice sorgente.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Le estensioni dei file di backup o temporanei sensibili (ad es. `.bak`, `.old`, `.swp`, `~`) sono accessibili?
- [ ] No — il server restituisce 403 Forbidden o 404 Not Found per le comuni estensioni di backup  
- [ ] Sì — il server permette il download dei file di backup, ma questi **non** contengono dati sensibili  
- [ ] Sì — il server permette il download di file di backup contenenti dati sensibili o codice sorgente *(Alta)*  

### Il server espone file di configurazione di sistema o di ambiente (ad es. `.env`, `.config`, `.yml`, `.ini`)?
- [ ] No — i file di configurazione riservati **non** possono essere consultati e restituiscono 403/404  
- [ ] Sì — i file di configurazione sensibili **sono** accessibili e rivelano segreti interni o credenziali  

### È possibile recuperare il codice sorgente aggiungendo estensioni alternative (ad es. `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] No — il codice dell'applicazione **non** viene visualizzato come testo in chiaro indipendentemente dalla manipolazione dell'estensione  
- [ ] Sì — il codice sorgente viene rivelato perché il server **non** dispone di un handler per l'estensione aggiunta  

### Le directory amministrative o di metadati (ad es. `.git/`, `.svn/`, `.DS_Store`) sono protette dall'accesso pubblico?
- [ ] No — queste directory/file **non** esistono nella web root  
- [ ] Sì — l'accesso **non è possibile** grazie a regole di rewrite lato server o permessi  
- [ ] Sì — le directory dei metadati **sono** accessibili e consentono la ricostruzione completa del codice sorgente  

### In che modo il server gestisce le richieste per file con estensioni multiple (ad es. `file.php.jpg`)?
- [ ] No — il server assegna correttamente la priorità alla sicurezza dell'estensione interna o blocca la richiesta  
- [ ] Sì — il server elabora il file come se avesse la prima estensione, ma il bypass **non è possibile** per l'esecuzione  
- [ ] Sì — il server ignora l'estensione finale, rendendo **possibile** l'esecuzione di codice o la divulgazione del sorgente

---

## WSTG-CONF-04 — Analisi di vecchi backup e file non referenziati per informazioni sensibili

L'analisi di vecchi backup e file non referenziati consiste nell'identificare file dimenticati, temporanei o nascosti su un server web il cui accesso non è destinato al pubblico. Questi file includono spesso backup del codice sorgente (`.zip`, `.bak`), file di swap di editor di testo (`.swp`, `~`) o metadati del controllo di versione (`.git`, `.svn`) che possono causare la perdita di informazioni sensibili. Gli aggressori utilizzano wordlist automatizzate e strumenti di discovery per eseguire il Brute Force sulle convenzioni di denominazione comuni, con l'obiettivo di esfiltrare credenziali di database, chiavi API hardcoded o logiche che facilitano ulteriori exploit. Questa vulnerabilità deriva solitamente dalla manutenzione manuale del server o da pipeline CI/CD difettose che non riescono a sanificare la web root di produzione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se vengono scoperti file di configurazione, archivi del codice sorgente o credenziali.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Il directory listing è abilitato sul server web?
- [ ] No — il directory listing è **disabilitato** e restituisce un errore 403 Forbidden o un errore personalizzato  
- [ ] Sì — il directory listing è **abilitato** su directory non sensibili  
- [ ] Sì — il directory listing è **abilitato** su directory contenenti codice sorgente o file sensibili *(Critico)*  

### I file di backup con estensioni comuni (es. .bak, .old, .save) sono individuabili?
- [ ] No — le estensioni di backup comuni **non sono state trovate** o sono bloccate dalla configurazione del server  
- [ ] Sì — i file di backup esistono ma **non** contengono informazioni sensibili  
- [ ] Sì — i file di backup della configurazione o del codice sorgente **sono** accessibili *(Alta)*  

### Le directory del controllo di versione (es. .git, .svn) o i metadati sono esposti?
- [ ] No — i metadati del controllo di versione **non sono presenti** o sono correttamente bloccati  
- [ ] Sì — i metadati esistono ma l'intero repository **non può** essere ricostruito  
- [ ] Sì — l'intero repository del codice sorgente **può** essere esfiltrato tramite i metadati esposti  

### Sono presenti archivi compressi (es. .zip, .tar.gz, .rar) dell'applicazione nella web root?
- [ ] No — nessun archivio sensibile è stato individuato tramite Brute Force o enumerazione  
- [ ] Sì — sono stati trovati archivi, ma sono protetti da password o contengono asset pubblici  
- [ ] Sì — archivi non crittografati contenenti il codice sorgente o i dati dell'applicazione **sono** accessibili pubblicamente  

### L'applicazione espone file temporanei creati da editor di testo o IDE?
- [ ] No — i file temporanei come `.swp`, `~` o `.DS_Store` **non sono presenti**  
- [ ] Sì — sono **presenti** file temporanei non sensibili  
- [ ] Sì — i file temporanei rivelano frammenti di codice sorgente o percorsi interni

---

## WSTG-CONF-05 — Enumerazione delle Interfacce di Amministrazione dell'Infrastruttura e dell'Applicazione

Le interfacce amministrative rappresentano il piano di controllo della gestione di un'applicazione e della sua infrastruttura sottostante, garantendo spesso l'accesso privilegiato alle configurazioni di sistema, ai dati degli utenti e alle operazioni lato server. Gli attaccanti danno la priorità all'enumerazione di queste interfacce attraverso il Brute Force delle directory, l'analisi del file `robots.txt` o l'osservazione di pattern URL prevedibili per trovare punti di ingresso che potrebbero mancare di un'autenticazione robusta o basarsi su credenziali predefinite. La scoperta di un pannello di amministrazione esposto aumenta significativamente il rischio di compromissione totale del sistema, modifica non autorizzata dei dati o interruzione del servizio. Queste interfacce si trovano frequentemente su porte non standard o sottodomini, rendendole bersagli primari per scanner automatizzati e ricognizione manuale.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Medio / Alto* |

> *La gravità diventa Alta se l'interfaccia consente modifiche alla configurazione a livello di sistema, gestione degli utenti o esfiltrazione di dati senza autenticazione a più fattori.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Le interfacce amministrative sono individuabili tramite directory Fuzzing o ricognizione?
- [ ] No — nessuna interfaccia amministrativa è esposta o individuabile  
- [ ] Sì — le interfacce sono individuabili ma l'accesso **è limitato** a intervalli IP interni  
- [ ] Sì — le interfacce **sono** individuabili e accessibili pubblicamente  

### L'autenticazione è obbligatoria sui portali amministrativi individuati?
- [ ] Sì — l'autenticazione a più fattori (MFA) **è obbligatoria**  
- [ ] Sì — l'autenticazione a fattore singolo **è obbligatoria**  
- [ ] No — non è richiesta alcuna autenticazione per accedere all'interfaccia *(Critico)*  

### I percorsi amministrativi sono nascosti o non standard per prevenirne l'individuazione?
- [ ] Sì — i percorsi utilizzano una denominazione non standard, casuale o non prevedibile  
- [ ] No — i percorsi utilizzano convenzioni di denominazione comuni (es. `/admin`, `/manager`, `/console`) e **possono** essere facilmente indovinati  

### L'interfaccia fornisce l'accesso a funzionalità sensibili del sistema o dell'applicazione?
- [ ] No — l'interfaccia è una dashboard di stato in "sola lettura" con impatto minimo  
- [ ] Sì — l'interfaccia **può** modificare la configurazione dell'applicazione o i dati degli utenti  
- [ ] Sì — l'interfaccia **può** eseguire comandi a livello di sistema o gestire il database

---

## WSTG-CONF-06 — Test dei Metodi HTTP

Il test dei metodi HTTP consiste nell'identificare quali verbi sono supportati dal server web e dall'applicazione per garantire che venga esposta solo la funzionalità necessaria. Oltre ai protocolli standard `GET` e `POST`, i server potrebbero inavvertitamente abilitare metodi pericolosi come `PUT`, `DELETE` o `TRACE`, che possono consentire agli aggressori di caricare file dannosi, eliminare contenuti esistenti o esfiltrare cookie di sessione tramite Cross-Site Tracing (XST). I Pentester esaminano gli header di risposta come `Allow` o `Public` e tentano di aggirare le restrizioni utilizzando tecniche di method overriding o verb tampering per accedere a funzionalità non autorizzate. Questo test è fondamentale per il consolidamento (hardening) della superficie di attacco e per prevenire errori di configurazione che potrebbero portare alla compromissione del server o alla manipolazione non autorizzata dei dati.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media* |

> *La gravità diventa Alta se `PUT` o `DELETE` consentono la manipolazione non autorizzata di file o se `TRACE` facilita l'esfiltrazione dei token di sessione.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### I metodi HTTP pericolosi come `PUT` o `DELETE` sono disabilitati in tutta l'applicazione?
- [ ] Sì — solo i metodi sicuri (GET, POST, HEAD) **sono abilitati**  
- [ ] No — `PUT` o `DELETE` **sono abilitati** ma richiedono un'autenticazione valida  
- [ ] No — `PUT` o `DELETE` **sono abilitati** e accessibili senza autenticazione *(Critico)*  

### Il metodo `TRACE` è disabilitato per prevenire il Cross-Site Tracing (XST)?
- [ ] Sì — il metodo `TRACE` **è disabilitato** o restituisce un 405 Method Not Allowed  
- [ ] No — il metodo `TRACE` **è abilitato** ma non riflette header sensibili  
- [ ] No — il metodo `TRACE` **è abilitato** e riflette gli header `Cookie` o `Authorization` *(Media)*  

### L'HTTP Method Overriding (es. `X-HTTP-Method-Override`) è supportato dal server?
- [ ] No — il server **non** elabora gli header di method override  
- [ ] Sì — il server elabora gli header di override ma i controlli di accesso **sono applicati**  
- [ ] Sì — il server elabora gli header di override ed è possibile l'aggiramento (bypass) del controllo degli accessi  

### Il server risponde in modo sicuro a metodi HTTP arbitrari o malformati?
- [ ] Sì — il server restituisce un 405 Method Not Allowed o 501 Not Implemented  
- [ ] No — il server restituisce un 200 OK o 500 Error per metodi arbitrari come `JEFF` o `TEST`  
- [ ] No — l'uso di metodi arbitrari consente di aggirare i filtri di autenticazione o autorizzazione  

### Il metodo `OPTIONS` è limitato o configurato per ridurre al minimo la divulgazione di informazioni?
- [ ] Sì — `OPTIONS` è disabilitato o restituisce un header `Allow` minimo  
- [ ] No — `OPTIONS` rivela una vasta gamma di metodi abilitati, inclusi i verbi DAV o di debug

---

## WSTG-CONF-07 — Test HTTP Strict Transport Security

L'HTTP Strict Transport Security (HSTS) è un meccanismo di sicurezza che consente a un server web di informare i browser che l'accesso deve avvenire esclusivamente tramite HTTPS, prevenendo attacchi di protocol downgrade e il cookie hijacking. Imponendo una connessione crittografata, l'HSTS mitiga gli attacchi Man-in-the-Middle (MitM) come lo SSLStrip, in cui un attaccante intercetta una richiesta HTTP iniziale non sicura per impedire il passaggio a TLS. Dal punto di vista di un attaccante, l'assenza di questo header o una durata breve del `max-age` fornisce una finestra di opportunità per intercettare session token sensibili o credenziali durante l'handshake iniziale in chiaro. Questo test si concentra sulla verifica della presenza, della durata e dell'ambito dell'header `Strict-Transport-Security` su tutti gli endpoint sensibili dell'applicazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media* |

> *La gravità diventa Media se l'applicazione gestisce PII, session token o credenziali, poiché la mancanza di HSTS aumenta la percentuale di successo degli attacchi MitM.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### L'header `Strict-Transport-Security` è presente nelle risposte HTTPS?
- [ ] Sì — l'header **è presente** su tutti gli endpoint sensibili  
- [ ] Sì — l'header **è presente** ma solo sulla landing page  
- [ ] No — l'header **è assente** nell'intera applicazione  

### La direttiva `max-age` è impostata su una durata sufficiente?
- [ ] Sì — il `max-age` è impostato a un anno o più (es. `31536000`)  
- [ ] Sì — il `max-age` è impostato ma è **troppo breve** (es. meno di 6 mesi)  
- [ ] No — la direttiva `max-age` **è assente** o impostata a `0` *(Critico)*  

### La policy HSTS copre tutti i sottodomini?
- [ ] Sì — la direttiva `includeSubDomains` **è applicata**  
- [ ] No — la direttiva `includeSubDomains` **è assente**, lasciando i sottodomini vulnerabili al downgrade  
- [ ] No — la funzionalità non è applicabile in quanto non esistono sottodomini  

### L'applicazione è registrata per l'HSTS Preloading?
- [ ] Sì — la direttiva `preload` **è presente** e il dominio è nella lista di preload HSTS  
- [ ] Sì — la direttiva `preload` **è presente** ma il dominio **non** è ancora nella lista di preload  
- [ ] No — la direttiva `preload` **è assente**, consentendo una finestra per attacchi MitM alla prima visita  

### L'header HSTS viene inviato erroneamente su HTTP in chiaro?
- [ ] No — l'header viene inviato **solo** su connessioni HTTPS sicure  
- [ ] Sì — l'header **viene inviato** su HTTP, il che viene ignorato dai browser e potrebbe esporre dettagli di configurazione

---

## WSTG-CONF-08 — Test RIA Cross Domain Policy

Le policy cross-domain delle Rich Internet Application (RIA) coinvolgono file come `crossdomain.xml` e `clientaccesspolicy.xml`, che definiscono il modo in cui i client web—nello specifico Adobe Flash e Microsoft Silverlight—gestiscono le richieste cross-origin. Questi file sono critici per la sicurezza poiché sovrascrivono esplicitamente la Same-Origin Policy (SOP), determinando quali domini esterni siano autorizzati a interagire con l'applicazione e a leggerne le risposte. Configurazioni eccessivamente permissive, come l'uso di wildcard nell'attributo `domain`, permettono agli attaccanti di ospitare oggetti RIA malevoli su siti di terze parti in grado di esfiltrare dati sensibili o eseguire azioni per conto di utenti autenticati. I pentester tipicamente individuano questi file nella root web per valutare il livello di fiducia concesso alle origini di terze parti e identificare potenziali fughe di dati cross-origin.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se l'applicazione gestisce dati utente sensibili o identificativi di sessione che possono essere esfiltrati tramite una policy permissiva.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### I file delle policy cross-domain (`crossdomain.xml` o `clientaccesspolicy.xml`) sono presenti nella root web?
- [ ] No — i file **non** possono essere trovati nella directory root  
- [ ] Sì — `crossdomain.xml` (Flash) **è** presente  
- [ ] Sì — `clientaccesspolicy.xml` (Silverlight) **è** presente  
- [ ] Sì — entrambi i file delle policy RIA **sono** presenti  

### L'attributo di dominio `allow-access-from` è limitato a origini fidate?
- [ ] No — i file esistono ma non contengono voci `allow-access-from`  
- [ ] Sì — domini specifici e fidati sono esplicitamente in whitelist e il bypass **non è possibile**  
- [ ] Sì — i domini sono in whitelist ma includono terze parti non fidate o sottodomini  
- [ ] Sì — viene utilizzata una wildcard `*`, consentendo l'accesso da **qualsiasi** origine *(Critico)*  

### La policy permette comunicazioni non sicure via HTTP per risorse HTTPS?
- [ ] No — `secure="true"` è impostato (o è il valore predefinito), impedendo alle origini HTTP di accedere ai dati HTTPS  
- [ ] Sì — `secure="false"` è impostato esplicitamente, permettendo ad attaccanti MITM di esfiltrare dati sicuri  

### Gli header HTTP sono eccessivamente permissivi nella configurazione cross-domain?
- [ ] No — `allow-http-request-headers-from` è limitato o **non abilitato**  
- [ ] Sì — header specifici sono consentiti per domini fidati  
- [ ] Sì — viene utilizzata la wildcard `*` per gli header, permettendo agli attaccanti di inviare header personalizzati da qualsiasi dominio  

### La configurazione `site-control` (master-policy) è restrittiva?
- [ ] No — `permitted-cross-domain-policies` è impostato su `none` o `master-only`  
- [ ] Sì — la policy permette ad altri file sul server di definire le proprie regole (`all`)

---

## WSTG-CONF-09 — Test File Permission

Il testing dei permessi dei file comporta l'audit dei livelli di controllo degli accessi assegnati ai file e alle directory sul server web per garantire che le risorse sensibili non siano eccessivamente esposte a utenti o processi non autorizzati. Permessi configurati in modo errato possono consentire agli attaccanti di leggere file di configurazione sensibili contenenti credenziali di database, visualizzare codice sorgente proprietario o modificare script lato server per ottenere la Remote Code Execution. Questa vulnerabilità si verifica tipicamente durante la fase di deployment quando i flag predefiniti "world-readable" o "world-writable" vengono lasciati su directory critiche dell'applicazione, come percorsi di configurazione, cartelle di log o repository di caricamento. Dal punto di vista di un attaccante, la scoperta di un file non adeguatamente protetto funge spesso da catalizzatore primario per la Privilege Escalation o la compromissione totale del sistema.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se i file di configurazione sensibili (ad es., `.env`, `web.config`) sono leggibili o se è possibile l'accesso in scrittura alla web root.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Tool:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### I file di configurazione sensibili dell'applicazione sono protetti da accessi non autorizzati?
- [ ] Sì — i file di configurazione (ad es., `.env`, `config.php`) **non sono accessibili** tramite richieste web  
- [ ] Sì — i file sono presenti ma l'accesso è **limitato** ai soli utenti locali autorizzati  
- [ ] No — i file di configurazione sensibili **sono accessibili** e divulgano credenziali o segreti  

### Un attaccante può modificare file o directory all'interno della web root?
- [ ] No — tutte le directory della web root sono in **sola lettura** per l'utente del server web  
- [ ] Sì — esistono directory scrivibili ma l'esecuzione di script è **disabilitata**  
- [ ] Sì — esistono directory world-writable e **consentono** il caricamento e l'esecuzione di script *(Critico)*  

### I metadati del controllo di versione o i file di backup sono esposti a causa di permessi permissivi?
- [ ] No — `.git`, `.svn` e i file di backup (ad es., `.bak`, `~`) **non sono presenti** o sono protetti  
- [ ] Sì — le directory dei metadati esistono ma il directory listing è **disabilitato**  
- [ ] Sì — i metadati sensibili o i backup sono **completamente accessibili**, consentendo il recupero del codice sorgente  

### L'utente del server web opera secondo il principio del minimo privilegio (least privilege)?
- [ ] Sì — il processo del server web viene eseguito come utente **dedicato a basso privilegio**  
- [ ] No — il processo del server web viene eseguito con **privilegi non necessari** (ad es., `root` o `Administrator`)  

### I file di log sono protetti da lettura o modifica non autorizzata?
- [ ] Sì — i file di log sono **riservati** agli utenti amministrativi  
- [ ] Sì — i file di log sono leggibili ma **non** contengono token di sessione o PII  
- [ ] No — i file di log sono **pubblicamente leggibili** e divulgano dati sensibili sulle transazioni o informazioni sugli utenti

---

## WSTG-CONF-10 — Subdomain Takeover

Il Subdomain Takeover si verifica quando una voce DNS (tipicamente un record CNAME, ma occasionalmente record A o MX) punta a una risorsa dismessa o inesistente su un cloud provider di terze parti o un servizio di hosting. Gli attaccanti identificano questi record DNS "dangling" (sospesi) cercando messaggi di errore specifici da parte di provider come AWS, GitHub o Azure che indicano che una risorsa non è più rivendicata. Registrando il nome della risorsa abbandonata sulla piattaforma del provider, l'attaccante ottiene il pieno controllo sul sottodominio, permettendogli di ospitare contenuti malevoli, esfiltrare cookie di sessione con scope sul dominio base e bypassare le protezioni Content Security Policy (CSP) o CORS. Questa vulnerabilità è particolarmente pericolosa perché sfrutta la fiducia associata alla struttura del dominio legittimo dell'organizzazione per facilitare phishing ad alto impatto e furto di dati.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta / Critica* |

> *La severità diventa Critica se il sottodominio può essere utilizzato per il dirottamento (hijacking) dei cookie di sessione dal dominio base o per bypassare i redirect SSO/OAuth.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Strumenti:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### L'applicazione utilizza hosting di terze parti o servizi cloud tramite sottodomini?
- [ ] No — tutti i sottodomini puntano a uno spazio IP controllato dall'organizzazione  
- [ ] Sì — i sottodomini puntano a provider di terze parti (S3, GitHub Pages, Heroku, ecc.)  

### Esistono record DNS "dangling" che puntano a risorse inesistenti?
- [ ] No — tutti i record DNS identificati risolvono in risorse attive e controllate  
- [ ] Sì — esistono record CNAME o A per risorse che **non esistono più** presso il provider  
- [ ] Sì — i record DNS risolvono in NXDOMAIN o in pagine di errore specifiche del provider *(es. "NoSuchBucket")*  

### La risorsa "dangling" identificata può essere rivendicata con successo?
- [ ] No — il provider dispone di protezioni contro la rivendicazione di nomi abbandonati o è richiesta la verifica dell'account  
- [ ] Sì — il nome della risorsa **può** essere registrato sulla piattaforma di terze parti da qualsiasi utente  

### Il dominio base utilizza cookie con scope "Domain" che potrebbero essere compromessi?
- [ ] No — i cookie hanno uno scope limitato a sottodomini specifici o utilizzano i flag `Host-Only`  
- [ ] Sì — i cookie sensibili (session ID, JWT) hanno come scope il dominio padre e **possono** essere esfiltrati tramite un sottodominio compromesso  

### Il sottodominio può essere utilizzato per bypassare gli header di sicurezza o i flussi di autenticazione?
- [ ] No — nessuna dipendenza di sicurezza sui sottodomini identificati  
- [ ] Sì — il sottodominio è inserito in whitelist nelle direttive CSP `script-src` o `connect-src`  
- [ ] Sì — il sottodominio è un URI di reindirizzamento valido per configurazioni OAuth o SSO

---

## WSTG-CONF-11 — Test Cloud Storage

Il testing delle errate configurazioni dello storage cloud prevede l'identificazione e l'audit dei servizi di object storage accessibili pubblicamente come Amazon S3, Azure Blobs o Google Cloud Storage. Queste risorse contengono spesso dati sensibili inclusi backup, codice sorgente, PII o file di configurazione che risultano esposti a causa di Access Control Lists (ACLs) o policy di Identity and Access Management (IAM) eccessivamente permissive. Gli attaccanti enumerano i nomi dei bucket attraverso la reconnaissance di file JavaScript, sorgenti HTML e tecniche di brute-force per trovare punti di ingresso per la data exfiltration o la modifica non autorizzata dei file. Lo sfruttamento può portare a un impatto significativo, inclusi data breach completi o la possibilità di distribuire file malevoli da un dominio attendibile.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica* |

> *La gravità diventa Critica se PII, credenziali o backup di produzione sono accessibili, o se è abilitato l'accesso in scrittura non autenticato.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Gli endpoint dello storage cloud (bucket/container) sono stati identificati ed enumerati?
- [ ] No — non vengono utilizzati o non sono individuabili endpoint di storage cloud  
- [ ] Sì — gli endpoint di storage sono identificati tramite analisi del codice o monitoraggio del traffico  
- [ ] Sì — gli endpoint di storage sono individuati tramite brute-force sulla convenzione di denominazione  

### Gli utenti non autenticati possono elencare i contenuti dello storage cloud?
- [ ] No — l'elenco dei bucket (listing) è **disabilitato** e restituisce 403 Forbidden o 404 Not Found  
- [ ] Sì — l'elenco è **abilitato** ma non sono presenti file sensibili  
- [ ] Sì — l'elenco è **abilitato** e i nomi/metadati dei file sensibili sono visibili  

### È possibile scaricare file sensibili dai bucket identificati?
- [ ] No — i file sono protetti da policy IAM o è richiesta un'autenticazione valida  
- [ ] Sì — i file sono accessibili ma richiedono un URL firmato (signed URL) che **non può** essere facilmente indovinato  
- [ ] Sì — i file sensibili (backup, `.env`, PII) sono **accessibili pubblicamente** per il download *(Critica)*  

### Il caricamento o la modifica di file senza autenticazione sono consentiti?
- [ ] No — i caricamenti o le modifiche non autenticati **non sono possibili**  
- [ ] Sì — è richiesto il caricamento autenticato ma il bypass **è possibile** tramite condizioni di policy deboli  
- [ ] Sì — la scrittura, sovrascrittura o eliminazione non autenticata di file è **possibile** *(Critica)*  

### Le policy Cross-Origin Resource Sharing (CORS) sull'endpoint di storage sono restrittive?
- [ ] Sì — il CORS è **disabilitato** o limitato a specifici origin attendibili  
- [ ] No — il CORS è **abilitato** con un origin wildcard `*`, consentendo l'accesso non autorizzato basato sul web

---

## WSTG-CONF-12 — Content Security Policy (CSP)

La Content Security Policy (CSP) è un meccanismo critico di difesa in profondità (defense-in-depth) implementato tramite gli header di risposta HTTP per mitigare rischi come Cross-Site Scripting (XSS), clickjacking e attacchi di data injection. Definendo quali risorse dinamiche possono essere caricate, una CSP ben configurata limita la capacità di un utente malintenzionato di eseguire script non autorizzati o di esfiltrare dati sensibili verso domini esterni. I pentester valutano la policy alla ricerca di direttive eccessivamente permissive, dell'uso di parole chiave non sicure come `'unsafe-inline'` e dell'affidamento a CDN non sicure che potrebbero facilitare i bypass. Una CSP debole o mancante non crea direttamente una vulnerabilità, ma aumenta significativamente l'impatto di attacchi injection andati a buon fine, non fornendo un secondo livello di protezione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Strumenti:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### L'header Content Security Policy (CSP) è presente nelle risposte dell'applicazione?
- [ ] Sì — l'header `Content-Security-Policy` è **presente** ed **applicato**  
- [ ] Sì — `Content-Security-Policy-Report-Only` è **presente** per test  
- [ ] No — nessun header CSP è **presente** nelle risposte  

### Le direttive `script-src` o `default-src` sono correttamente limitate?
- [ ] Sì — le direttive utilizzano whitelist rigorose, nonce o hash e il bypass **non è possibile**  
- [ ] Sì — le direttive sono **correttamente configurate** ma la whitelist include CDN con bypass noti  
- [ ] No — le direttive utilizzano wildcard (`*`) o schemi `data:`, rendendo **possibile** il bypass  

### La policy consente l'esecuzione di script inline non sicuri o l'uso di eval?
- [ ] No — `'unsafe-inline'` e `'unsafe-eval'` **non sono presenti**  
- [ ] Sì — `'unsafe-inline'` è **presente** ma protetto da un `nonce` o `hash`  
- [ ] Sì — `'unsafe-inline'` o `'unsafe-eval'` sono **abilitati** senza protezioni aggiuntive *(Critico)*  

### L'applicazione è protetta contro il clickjacking tramite la direttiva `frame-ancestors`?
- [ ] Sì — `frame-ancestors` è impostato su `'none'` o `'self'`  
- [ ] Sì — `frame-ancestors` è **abilitato** ma consente specifici domini di terze parti fidati  
- [ ] No — `frame-ancestors` è **assente**, si affida al legacy `X-Frame-Options` o a nessuna protezione  

### Esiste un meccanismo per segnalare le violazioni della CSP?
- [ ] Sì — `report-uri` o `report-to` è **configurato** e attivo  
- [ ] No — il reporting delle violazioni è **disabilitato** o **non configurato**

---

## WSTG-CONF-13 — Path Confusion

La Path Confusion deriva da discrepanze nel modo in cui diversi componenti web, come reverse proxy, load balancer e server applicativi di backend, analizzano e interpretano i percorsi URL. Gli attaccanti sfruttano queste incoerenze iniettando caratteri specifici come punti e virgola, slash codificati o segmenti punto (dot-segments) per ingannare i filtri di sicurezza e accedere a endpoint limitati o risorse statiche. Questa vulnerabilità può portare a gravi falle di sicurezza, tra cui il bypass dell'autenticazione, l'accesso non autorizzato ai dati e il Web Cache Poisoning, poiché il front-end potrebbe applicare regole di sicurezza a un percorso che il back-end interpreta in modo diverso. Lo sfruttamento (Exploit) con successo si verifica tipicamente al confine architetturale dove la logica di routing delle richieste e la normalizzazione divergono all'interno dello stack tecnologico.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se la path confusion comporta un bypass dell'autenticazione o del controllo degli accessi per endpoint amministrativi sensibili.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### I diversi livelli architetturali interpretano i delimitatori di percorso (es. `;`, `#`, `?`) in modo coerente?
- [ ] Sì — tutti i livelli normalizzano e interpretano i delimitatori di percorso in modo identico  
- [ ] No — esistono incoerenze ma **non possono** essere utilizzate per accedere a risorse limitate  
- [ ] No — **è possibile** che le discrepanze consentano a un attaccante di bypassare i filtri front-end e raggiungere la logica di back-end  

### I controlli di accesso possono essere bypassati utilizzando sequenze di path traversal o caratteri codificati?
- [ ] No — la normalizzazione viene applicata in modo coerente prima dei controlli di sicurezza  
- [ ] Sì — il bypass è **possibile** tramite sequenze di segmenti punto (es. `/admin/..;/`)  
- [ ] Sì — il bypass è **possibile** tramite caratteri codificati in URL (es. `%2f`, `%2e%2e%2f`)  

### L'applicazione è suscettibile al Web Cache Poisoning tramite path confusion?
- [ ] No — la logica di caching non è influenzata dall'ambiguità del percorso o da informazioni di percorso extra  
- [ ] Sì — contenuti malevoli **possono** essere memorizzati in cache per percorsi legittimi a causa di discrepanze di mappatura  
- [ ] Sì — informazioni sensibili **possono** essere memorizzate in cache in directory pubbliche tramite tecniche di `RCD` (Relative Path Overwrite)  

### Il server gestisce le "informazioni di percorso extra" (PathInfo) in modo sicuro?
- [ ] No — la funzionalità è **disabilitata** o non esiste  
- [ ] Sì — `PathInfo` viene gestito e **non** interferisce con i filtri di sicurezza  
- [ ] No — `PathInfo` consente a un attaccante di aggiungere dati che confondono la logica di routing dell'applicazione

---

## WSTG-CONF-14 — Test delle configurazioni errate di altre intestazioni di sicurezza HTTP

Il test per le configurazioni errate delle intestazioni di sicurezza HTTP comporta la verifica della presenza e della corretta implementazione delle intestazioni difensive che forniscono una protezione essenziale contro i comuni vettori di attacco web. Sebbene queste intestazioni non risolvano le vulnerabilità del codice sottostante, la loro assenza aumenta significativamente il rischio di attacchi Man-in-the-Middle (MITM), dello sfruttamento del MIME-sniffing e della perdita involontaria di dati cross-origin tramite referrer. Dal punto di vista di un attaccante, la mancanza di intestazioni di sicurezza rappresenta un indicatore ad alta visibilità di un ambiente scarsamente protetto (hardened), facilitando spesso catene di exploit più complesse come il session hijacking o l'esfiltrazione di informazioni. Queste configurazioni sono tipicamente valutate a livello di server e dovrebbero essere applicate in modo coerente a tutti i tipi di risposta, inclusi gli endpoint API e le risorse statiche.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audit della presenza delle intestazioni di sicurezza
| Intestazione | Presente | Mancante | Configurazione Raccomandata |
|--------------|:--------:|:--------:|-----------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` o `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Specifica per funzionalità, es., `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### In che modo le intestazioni di sicurezza vengono applicate nell'ambito dell'applicazione?
- [ ] Sì — le intestazioni sono applicate globalmente tramite un load balancer centrale o un reverse proxy e **non possono** essere sovrascritte  
- [ ] Sì — le intestazioni sono applicate alle risposte dei documenti principali ma sono **mancanti** nelle risposte API o negli asset statici  
- [ ] No — le intestazioni sono applicate in modo incoerente o sono **mancanti** in tutto il dominio dell'applicazione  

### L'intestazione Strict-Transport-Security (HSTS) è configurata correttamente?
- [ ] Sì — `max-age` è impostato su una durata lunga (es. 2 anni) e `includeSubDomains` è **abilitato**  
- [ ] Sì — `max-age` è impostato ma `includeSubDomains` è **disabilitato**, lasciando vulnerabili i sottodomini  
- [ ] No — l'intestazione HSTS **non è presente** o `max-age` è impostato su 0, consentendo attacchi di protocol downgrade  

### La Referrer-Policy impedisce la perdita di parametri URL sensibili?
- [ ] Sì — la policy è impostata su `no-referrer` o `same-origin` *(Più sicura)*  
- [ ] Sì — la policy è impostata su `strict-origin-when-cross-origin`  
- [ ] No — la policy **non è impostata** o è impostata su `unsafe-url`, con la potenziale perdita di token o PII sensibili nelle intestazioni referer  

### L'intestazione X-Content-Type-Options impedisce il MIME-sniffing?
- [ ] Sì — la direttiva `nosniff` è **presente** e correttamente implementata  
- [ ] No — l'intestazione è **mancante**, consentendo potenzialmente a un browser di eseguire file non eseguibili (es. immagini) come script

---

## WSTG-IDNT-01 — Test delle Definizioni dei Ruoli

Il test delle definizioni dei ruoli prevede l'identificazione e la documentazione dei vari ruoli utente e dei livelli di autorizzazione definiti all'interno dell'applicazione per garantire che siano logicamente separati e aderiscano al principio del minimo privilegio (**least privilege**). Un attaccante analizza l'applicazione per mappare i ruoli ad alto privilegio rispetto ai ruoli utente standard, cercando eventuali ambiguità nei confini dei permessi o funzioni amministrative non documentate. L'identificazione di questi ruoli è il primo passo per scoprire percorsi di **privilege escalation**, poiché le incongruenze nelle definizioni dei ruoli portano spesso all'accesso non autorizzato a dati sensibili o funzionalità amministrative. Questa fase si verifica tipicamente durante la ricognizione iniziale e la mappatura della **business logic**, concentrandosi sui profili utente, sulle dashboard amministrative e sulle strutture dei permessi delle **API**.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### I ruoli sono chiaramente definiti e documentati nell'applicazione o nella sua documentazione?
- [ ] Sì — i ruoli sono completamente documentati e seguono il principio del **least privilege**  
- [ ] Sì — i ruoli sono documentati ma contengono **permessi sovrapposti**  
- [ ] No — non esiste alcuna documentazione o definizione formale dei ruoli  

### Esiste una chiara separazione tra le funzioni amministrative e quelle degli utenti standard?
- [ ] Sì — le funzioni amministrative sono **completamente isolate** dalle viste degli utenti standard  
- [ ] Sì — la separazione è presente ma gli endpoint amministrativi sono **prevedibili o indovinabili**  
- [ ] No — le funzioni amministrative e standard sono **mischiate** all'interno delle stesse interfacce  

### L'applicazione utilizza un meccanismo centralizzato per il Role-Based Access Control (RBAC)?
- [ ] Sì — il **RBAC** o **ABAC** centralizzato è **implementato** e applicato in modo coerente  
- [ ] Sì — esistono controlli decentralizzati ma sono **applicati in modo incoerente** tra i vari moduli  
- [ ] No — la logica di controllo degli accessi è **hardcoded** nelle singole pagine o componenti  

### Sono presenti ruoli non documentati o nascosti nel sistema?
- [ ] No — tutti i ruoli scoperti corrispondono alle funzionalità documentate  
- [ ] Sì — sono presenti ruoli nascosti o "shadow" (ad es., `super_admin`, `support`, `test`)  

### Un utente con bassi privilegi può enumerare i permessi o i ruoli di altri utenti?
- [ ] No — gli utenti **non possono** visualizzare o enumerare i ruoli/permessi di altre entità  
- [ ] Sì — gli utenti **possono** enumerare i ruoli tramite pagine di profilo, metadati o risposte **API** *(Media)*

---

## WSTG-IDNT-02 — Test del Processo di Registrazione Utente

Il test del processo di registrazione utente consiste nel valutare il workflow attraverso il quale vengono create nuove identità per garantire che non possa essere abusato per accessi non autorizzati o sfruttamento automatizzato. Le vulnerabilità in questo processo si manifestano spesso come mancanza di verifica dell'identità (email/SMS), suscettibilità alla creazione massiva di account tramite bot, o Information Disclosure attraverso messaggi di errore descrittivi che rivelano nomi utente esistenti. Gli attaccanti sfruttano queste falle per eseguire User Enumeration, condurre campagne di spam su larga scala o manipolare i parametri di registrazione per ottenere privilegi elevati durante la fase di creazione dell'account. Questo test è fondamentale per proteggere l'integrità dell'applicazione e impedire agli aggressori di popolare il sistema con account fraudolenti o malevoli.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### È richiesta la verifica dell'identità prima che un nuovo account diventi attivo?
- [ ] Sì — la registrazione richiede un token univoco a scadenza temporale inviato via email o SMS  
- [ ] Sì — la verifica è richiesta ma i token sono **deboli** o **prevedibili**  
- [ ] No — gli account vengono attivati **immediatamente** senza alcuna fase di verifica  

### Il processo di registrazione previene la User Enumeration?
- [ ] Sì — l'applicazione restituisce risposte **identiche** sia per email/username nuovi che per quelli esistenti  
- [ ] No — i messaggi di errore (es. "Email già in uso") **permettono** a un attaccante di mappare gli utenti registrati  
- [ ] No — differenze temporali nelle risposte del server (**timing differences**) rivelano se un utente esiste  

### Sono implementati controlli anti-automazione per prevenire la registrazione massiva?
- [ ] Sì — meccanismi CAPTCHA o proof-of-work sono **presenti** ed **efficaci**  
- [ ] Sì — i controlli esistono ma il bypass **è possibile** tramite endpoint API o manipolazione degli header  
- [ ] No — nessun Rate Limiting o CAPTCHA è **applicato** all'endpoint di registrazione  

### I parametri di registrazione possono essere manipolati per scalare i privilegi?
- [ ] No — i ruoli utente sono assegnati **esclusivamente** dalla logica server-side  
- [ ] Sì — parametri come `role`, `admin`, o `group_id` **possono** essere modificati nella richiesta per ottenere un accesso superiore  

### La password policy è applicata durante la fase di registrazione?
- [ ] Sì — requisiti di password complessa sono **imposti** lato server  
- [ ] No — password deboli (es. "password123") sono **accettate** dal sistema

---

## WSTG-IDNT-03 — Test del Processo di Provisioning degli Account

Il processo di provisioning degli account disciplina il modo in cui le nuove identità vengono create, verificate e assegnate a specifici privilegi all'interno di un ambiente applicativo. Gli attaccanti mirano a questo workflow per eseguire registrazioni di massa automatizzate finalizzate allo spam o al Denial-of-Service (DoS), eludere i passaggi di verifica dell'identità per creare account fraudolenti o manipolare i parametri per concedersi permessi elevati durante la fase di registrazione (signup). Le vulnerabilità si manifestano spesso nei moduli di registrazione self-service, nei sistemi basati su invito o nelle console di provisioning amministrativo, dove i difetti della Business Logic consentono la creazione di account non autorizzati o l'Escalation dei Privilegi (Privilege Escalation). Dal punto di vista di un attaccante, la compromissione del flusso di provisioning fornisce un punto di appoggio legittimo che può essere utilizzato come base per exploitation più profonde, esfiltrazione di dati o attacchi di Ingegneria Sociale (Social Engineering).


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La Gravità diventa Critica se un attaccante può effettuare il provisioning di account amministrativi o eludere le restrizioni globali di registrazione.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### La registrazione autonoma (self-registration) è strettamente controllata o limitata agli utenti autorizzati?
- [ ] Sì — la registrazione è **disabilitata** o richiede l'approvazione amministrativa manuale  
- [ ] Sì — la registrazione è aperta ma limitata a domini email **autorizzati** o a token di invito  
- [ ] No — la registrazione è **aperta** a qualsiasi utente senza restrizioni di dominio o token  

### È richiesto un meccanismo robusto di verifica dell'identità (ad es. email/SMS) prima dell'attivazione dell'account?
- [ ] Sì — la verifica è **richiesta** e **non può** essere elusa  
- [ ] Sì — la verifica è **richiesta** ma **può** essere elusa tramite la manipolazione dei parametri (Parameter Tampering) o token prevedibili  
- [ ] No — gli account sono **attivi immediatamente** dopo l'invio senza alcuna verifica  

### L'utente può manipolare i parametri relativi ai ruoli o ai permessi durante la richiesta di registrazione?
- [ ] No — i ruoli sono **assegnati lato server (server-side)** e non esistono parametri lato client  
- [ ] Sì — i parametri del ruolo esistono ma la manipolazione **non è possibile** a causa della validazione lato server  
- [ ] Sì — i parametri di ruolo/permesso (ad es. `role=admin`, `is_staff=true`) **possono** essere modificati per ottenere un accesso privilegiato *(Critico)*  

### Sono implementati controlli di Rate Limiting o anti-automazione per prevenire la creazione di massa di account?
- [ ] Sì — il Rate Limiting e i CAPTCHA sono **attivi** ed efficaci  
- [ ] Sì — i controlli esistono ma l'elusione **è possibile** tramite lo spoofing degli header o servizi di risoluzione CAPTCHA  
- [ ] No — non viene **applicato** alcun Rate Limiting o controllo anti-automazione  

### Il processo di provisioning espone informazioni sugli account esistenti (User Enumeration)?
- [ ] No — i messaggi di risposta sono **identici** per utenti esistenti e non esistenti  
- [ ] Sì — risposte differenti o variazioni temporali (timing variations) **consentono** l'enumerazione degli utenti esistenti

---

## WSTG-IDNT-04 — Testing for Account Enumeration and Guessable User Account

L'Account Enumeration si verifica quando un'applicazione rivela l'esistenza o l'inesistenza di un account utente attraverso variazioni nelle risposte HTTP, messaggi di errore o differenze temporali misurabili. Gli attaccanti sfruttano questo comportamento inviando sistematicamente liste di nomi utente per identificare account validi, che vengono successivamente presi di mira per attacchi di Credential Stuffing, Brute Force o Social Engineering. Questa vulnerabilità si manifesta comunemente nei portali di login, nelle funzionalità di recupero password, nei moduli di registrazione e negli endpoint API che gestiscono identificatori specifici dell'utente. Dal punto di vista di un attaccante, un'enumerazione riuscita riduce significativamente la superficie di attacco trasformando un tentativo di Brute Force alla cieca in uno sfruttamento mirato di identità note e valide.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### I messaggi di errore di autenticazione sono coerenti in tutti gli scenari?
- [ ] Sì — vengono utilizzati messaggi di errore generici sia per nomi utente non validi che per password non valide  
- [ ] Sì — i messaggi sono simili, ma **sono presenti** lievi variazioni nella punteggiatura o nel formato (casing)  
- [ ] No — i messaggi di errore indicano esplicitamente "Utente non trovato" o "Password errata" *(Vulnerabile)*  

### Il flusso di reset della password o di "password dimenticata" rivela l'esistenza dell'account?
- [ ] No — l'applicazione restituisce un messaggio generico indipendentemente dal fatto che l'email/nome utente esista  
- [ ] Sì — i controlli sono presenti, ma il bypass **è possibile** tramite la lunghezza della risposta o i codici di stato  
- [ ] Sì — l'applicazione conferma esplicitamente se è stata inviata un'email di reset o se l'account **non esiste**  

### Il processo di registrazione è protetto contro l'User Enumeration?
- [ ] No — la registrazione non rivela informazioni o utilizza CAPTCHA/Rate Limiting per prevenire controlli massivi  
- [ ] Sì — i controlli sono presenti, ma il bypass **è possibile** tramite differenze temporali (timing) o nelle risposte API  
- [ ] Sì — l'applicazione notifica immediatamente l'utente se il nome utente o l'email **è già in uso**  

### Le differenze temporali sono misurabili confrontando nomi utente validi e non validi?
- [ ] No — i tempi di risposta sono coerenti indipendentemente dalla validità dell'account grazie a confronti a tempo costante (constant-time comparisons)  
- [ ] Sì — esistono differenze temporali ma sono troppo piccole per essere misurate in modo affidabile sulla rete  
- [ ] Sì — discrepanze temporali misurabili **sono presenti** (ad esempio, a causa dell'esecuzione dell'hashing della password solo per gli utenti validi)  

### L'applicazione utilizza convenzioni di denominazione prevedibili o indovinabili per gli account?
- [ ] No — gli identificativi degli account sono casuali (UUID) o definiti dall'utente e complessi  
- [ ] Sì — gli identificativi degli account seguono un pattern (es. `emp001`, `emp002`) e l'enumerazione **è possibile**  
- [ ] Sì — gli identificativi sono interi sequenziali, rendendo **possibile** l'enumerazione completa del database

---

## WSTG-IDNT-05 — Testing for Weak or Unenforced Username Policy

La verifica di policy degli username deboli o non applicate si concentra sulle regole che governano la creazione degli identificativi degli account e sulla loro suscettibilità all'enumeration o allo spoofing. Policy deboli spesso permettono sequenze prevedibili, parole comuni di dizionario o identificativi che rispecchiano gli ID interni dei dipendenti, il che riduce significativamente lo spazio di ricerca per attacchi di Brute Force e Credential Stuffing. Dal punto di vista di un attaccante, l'identificazione di questi pattern è il primo passo per la scoperta di account su larga scala, spesso ottenuta analizzando i messaggi di errore di registrazione, il comportamento del reset della password o i metadati nei profili pubblici. Garantire una policy robusta impedisce agli attaccanti di mappare sistematicamente la base utenti e facilita la difesa contro phishing mirato e tentativi automatizzati di Authentication Bypass.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Gli username si basano su pattern altamente prevedibili o sequenziali?
- [ ] No — gli username sono stringhe casuali, definite dall'utente o ad alta entropia  
- [ ] Sì — gli username seguono un formato prevedibile (es. `user1001`, `user1002`) ma l'autenticazione è robusta  
- [ ] Sì — gli username sono **rigorosamente sequenziali** o seguono un formato aziendale noto, facilitando l'enumeration  

### L'applicazione impone requisiti minimi di lunghezza e complessità per gli username?
- [ ] Sì — policy rigorose sulla lunghezza e sul set di caratteri **sono applicate** durante la registrazione  
- [ ] Sì — esistono delle policy ma permettono username estremamente brevi (es. 1-2 caratteri) o eccessivamente semplici  
- [ ] No — **nessuna policy** viene applicata riguardo alla lunghezza dello username o ai tipi di caratteri  

### È possibile enumerare username validi attraverso le risposte dell'applicazione?
- [ ] No — l'applicazione restituisce messaggi di errore generici e mostra un timing coerente per tutti i tentativi  
- [ ] Sì — l'applicazione restituisce messaggi di errore distinti (es. "Username già in uso") ma il Rate Limiting **viene applicato**  
- [ ] Sì — l'applicazione **rivela (leaks)** username validi tramite errori di registrazione, reset della password o differenze di timing  

### La policy impedisce la registrazione di username amministrativi comuni o riservati?
- [ ] Sì — l'applicazione blocca nomi comuni (es. `admin`, `root`, `support`) e termini riservati di sistema  
- [ ] No — gli utenti **possono** registrare account con nomi sensibili o amministrativi  

### La policy degli username è coerente in tutti i punti di ingresso (API, Mobile, Web)?
- [ ] Sì — le policy **sono applicate** in modo coerente in tutte le interfacce e versioni  
- [ ] No — gli endpoint legacy o versioni specifiche delle API **non impongono** gli stessi vincoli sugli username

---

## WSTG-ATHN-01 — Test per le credenziali trasmesse su un canale crittografato

Il test per le credenziali trasmesse su un canale crittografato garantisce che i dati di autenticazione sensibili, come nomi utente, password e session tokens, siano protetti dall'intercettazione durante il transito. Gli attaccanti posizionati sulla rete — ad esempio tramite attacchi Man-in-the-Middle (MitM) su Wi-Fi pubblici o reti interne compromesse — possono utilizzare strumenti di packet sniffing per catturare credenziali in chiaro se TLS/SSL non è applicato correttamente. Questa vulnerabilità si verifica tipicamente negli endpoint di login, nei moduli di reset della password e negli header di autenticazione delle API dove l'applicazione non impone l'uso di HTTPS o effettua reindirizzamenti errati da HTTP. Lo sfruttamento riuscito garantisce all'attaccante l'accesso completo agli account utente, portando alla compromissione totale dei dati dell'utente e al potenziale movimento laterale (Lateral Movement) all'interno dell'applicazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Strumenti:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### L'HTTPS è imposto per l'intero processo di autenticazione?
- [ ] Sì — HSTS è **abilitato** e tutto il traffico è forzato rigorosamente su HTTPS  
- [ ] Sì — avviene il reindirizzamento a HTTPS, ma HSTS **non è abilitato**  
- [ ] No — le credenziali **possono** essere inviate su HTTP non crittografato  

### Le pagine di login e i moduli sono forniti tramite un canale crittografato?
- [ ] Sì — la pagina contenente il modulo di login è distribuita tramite HTTPS  
- [ ] No — la pagina di login è servita su HTTP, consentendo il form-action hijacking o lo sniffing delle credenziali  

### Il flag `Secure` è applicato a tutti i cookie di sessione sensibili?
- [ ] Sì — il flag `Secure` è **applicato** a tutti i cookie di autenticazione e di sessione  
- [ ] Sì — il flag `Secure` è **applicato** solo ad alcuni cookie  
- [ ] No — il flag `Secure` **non è applicato**, consentendo l'esfiltrazione dei cookie tramite richieste non crittografate  

### Il server supporta versioni TLS deboli o cipher suite insicure?
- [ ] No — sono **abilitati** solo TLS moderni (1.2+) e cifrari forti  
- [ ] Sì — sono **supportate** versioni legacy (TLS 1.0/1.1) o cifrari deboli (RC4, DES, CBC)  

### L'applicazione impedisce la trasmissione delle credenziali verso domini di terze parti su canali non crittografati?
- [ ] Sì — nessun dato sensibile viene inviato a domini esterni o tutte le chiamate esterne sono forzate su HTTPS  
- [ ] No — header di autenticazione o credenziali vengono inviati a endpoint di terze parti (ad es. analytics o SSO) tramite HTTP

---

## WSTG-ATHN-02 — Testing for Default Credentials

Il test per le credenziali predefinite (Default Credentials) consiste nell'identificare interfacce amministrative, servizi o componenti applicativi che utilizzano ancora nomi utente e password impostati in fabbrica o forniti dal produttore. Questa vulnerabilità rappresenta un obiettivo ad alta priorità per gli attaccanti poiché spesso fornisce un percorso immediato verso la compromissione totale del sistema, l'accesso amministrativo o la Remote Code Execution con un impegno minimo. Si verifica tipicamente in software standard (off-the-shelf), piattaforme CMS, strumenti di gestione di database e hardware integrato come dispositivi IoT o appliance di rete. L'Exploit viene solitamente eseguito confrontando le versioni del software identificate con database pubblici di password predefinite o utilizzando strumenti automatizzati di Credential Stuffing durante le fasi iniziali di ricognizione e sfruttamento.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Critica / Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Strumenti:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Le interfacce amministrative o di gestione sono esposte su Internet o su segmenti di rete non attendibili?
- [ ] No — nessuna interfaccia di gestione è accessibile  
- [ ] Sì — le interfacce sono presenti ma limitate da controlli a livello di rete (es. VPN, IP Allowlist)  
- [ ] Sì — le interfacce **sono** accessibili pubblicamente e richiedono autenticazione  

### Le credenziali predefinite fornite dal fornitore sono state modificate per tutti i componenti identificati?
- [ ] Sì — tutte le credenziali predefinite **sono state modificate** in tutti i componenti *(Massima sicurezza)*  
- [ ] Sì — le credenziali **sono state modificate** per l'applicazione principale, ma i componenti secondari (es. CMS, DB) mantengono quelle predefinite  
- [ ] No — le credenziali predefinite **sono attive** su una o più interfacce identificabili *(Critico)*  

### L'applicazione impone il cambio della password al primo accesso per i nuovi account o per gli account amministrativi?
- [ ] Sì — il cambio della password **è obbligatorio** prima di poter eseguire qualsiasi azione amministrativa  
- [ ] No — l'applicazione **consente** l'uso delle credenziali predefinite a tempo indeterminato  

### I meccanismi di blocco (lockout) o i controlli di Rate Limiting sono attivi per prevenire il test automatizzato delle credenziali predefinite?
- [ ] Sì — viene applicato un Rate Limiting aggressivo o il blocco dell'account (lockout)  
- [ ] Sì — i controlli sono presenti ma il bypass **è possibile** tramite manipolazione degli header o rotazione degli IP  
- [ ] No — non è abilitato alcun meccanismo di Rate Limiting o di blocco  

### I plugin di terze parti, i middleware o gli strumenti amministrativi (es. phpMyAdmin, Tomcat Manager) utilizzano credenziali univoche?
- [ ] No — non esistono componenti di terze parti nell'ambiente  
- [ ] Sì — tutti i componenti di terze parti utilizzano credenziali univoche e non predefinite  
- [ ] No — almeno un componente di terze parti **è accessibile** tramite credenziali predefinite note

---

## WSTG-ATHN-03 — Testing for Weak Lock Out Mechanism

Un meccanismo di blocco dell'account (account lockout mechanism) è un controllo critico di difesa in profondità progettato per mitigare attacchi di Brute Force e Credential Stuffing disabilitando un account dopo un numero specifico di tentativi di autenticazione falliti. Senza una robusta politica di blocco, gli attaccanti possono utilizzare strumenti automatizzati per indovinare sistematicamente le password attraverso più account (Password Spraying) o colpire un singolo account di alto valore fino al successo. Questa vulnerabilità si manifesta tipicamente sui portali di login, moduli di reset della password ed endpoint API che mancano di Rate Limiting o protezione a livello di account. Dal punto di vista di un attaccante, un meccanismo di blocco debole o assente consente tentativi di password infiniti, mentre uno eccessivamente aggressivo può essere sfruttato per causare un Denial of Service (DoS) distribuito contro utenti legittimi bloccando intenzionalmente i loro account.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se l'applicazione gestisce PII sensibili, dati finanziari o se gli account amministrativi sono suscettibili al Brute Force senza alcun controllo secondario.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Strumenti:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Viene applicato un meccanismo di blocco dell'account dopo un numero prestabilito di tentativi falliti?
- [ ] Sì — l'account viene bloccato dopo una soglia definita e ragionevole (es. 5-10 tentativi)  
- [ ] Sì — l'account viene bloccato ma solo dopo un numero eccessivamente elevato di tentativi  
- [ ] No — l'account **non può** essere bloccato e consente il Brute Force indefinito  

### Il meccanismo di blocco può essere eluso utilizzando tecniche comuni di bypass?
- [ ] No — il blocco è applicato lato server e **non** è influenzato da modifiche lato client  
- [ ] Sì — è **possibile** eludere il blocco ruotando gli indirizzi IP o utilizzando un pool di proxy  
- [ ] Sì — è **possibile** eludere il blocco manipolando gli header HTTP come `X-Forwarded-For` o `User-Agent`  
- [ ] Sì — è **possibile** eludere il blocco cancellando i cookie o avviando nuove sessioni  

### Il meccanismo di blocco fornisce un percorso per il ripristino dell'account o una scadenza?
- [ ] Sì — l'account viene sbloccato automaticamente dopo una durata stabilita o tramite verifica via email  
- [ ] Sì — l'account richiede l'intervento di un amministratore per lo sblocco  
- [ ] No — il blocco è permanente senza un chiaro percorso di ripristino, aumentando il rischio di DoS  

### L'applicazione distingue tra un nome utente valido e uno non valido durante il blocco?
- [ ] No — la risposta dell'applicazione è identica sia per gli utenti esistenti che per quelli non esistenti  
- [ ] Sì — l'applicazione rivela se un account è bloccato, consentendo l'**enumerazione dei nomi utente** (API o Username Enumeration)  

### Il meccanismo di blocco è suscettibile a un attacco Denial of Service (DoS)?
- [ ] No — controlli come CAPTCHA o il Rate Limiting basato su IP prevengono il blocco di massa degli account  
- [ ] Sì — un attaccante **può** bloccare sistematicamente tutti gli utenti conosciuti fornendo password errate

---

## WSTG-ATHN-04 — Test per l'aggiramento dello schema di autenticazione (Bypassing Authentication Schema)

L'aggiramento dello schema di autenticazione (Bypassing Authentication Schema) comporta l'identificazione e lo sfruttamento di falle nella logica applicativa che permettono a un attaccante di ottenere l'accesso a risorse protette senza fornire credenziali valide. Questa vulnerabilità si verifica tipicamente quando l'applicazione si affida a controlli lato client, non riesce a imporre l'autorizzazione lato server su ogni richiesta, o lascia esposte in produzione "backdoor" amministrative ed endpoint di debug. Gli attaccanti sfruttano queste debolezze eseguendo forced browsing verso URL sensibili, manipolando parametri HTTP come `authenticated=true`, o effettuando lo spoofing degli header per indurre l'applicazione a presumere che una sessione sia già stata stabilita. Uno sfruttamento (Exploit) riuscito comporta una compromissione totale del perimetro di autenticazione, portando potenzialmente all'esfiltrazione non autorizzata di dati o alla completa compromissione del sistema.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Test Status** | Non Eseguito |
| **Severity** | Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Strumenti:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### È possibile accedere direttamente a pagine interne sensibili senza una sessione attiva?
- [ ] No — l'accesso diretto comporta il reindirizzamento al login o una risposta 403/404  
- [ ] Sì — alcune pagine sensibili sono accessibili ma forniscono dati limitati o non sensibili  
- [ ] Sì — le pagine amministrative sensibili o specifiche dell'utente **sono** completamente accessibili senza autenticazione *(Critica)*  

### L'applicazione si affida a flag o parametri lato client per determinare lo stato di autenticazione?
- [ ] No — lo stato di autenticazione è gestito rigorosamente tramite lo stato della sessione lato server  
- [ ] Sì — esistono flag lato client ma la loro modifica **non** garantisce l'accesso alle aree protette  
- [ ] Sì — la modifica dei parametri (ad es., `is_authenticated=true`, `user_role=admin`) **permette** il bypass dell'autenticazione  

### L'autenticazione può essere bypassata tramite lo spoofing o la manipolazione di specifici header HTTP?
- [ ] No — gli header come `X-Forwarded-For`, `Referer` o header personalizzati non hanno alcun impatto sull'autenticazione  
- [ ] Sì — la manipolazione degli header (ad es., `X-Custom-IP-Authorization`, `X-Remote-User`) **è possibile** e garantisce l'accesso non autorizzato  

### Esistono percorsi alternativi o artefatti di sviluppo che aggirano il normale flusso di login?
- [ ] No — tutte le risorse protette sono presidiate da un controllo di autenticazione centralizzato e pronto per la produzione  
- [ ] Sì — endpoint legacy, rotte di debug (ad es., `/debug/login`) o versioni API dimenticate **sono** accessibili senza credenziali  

### L'applicazione omette di riautenticare o verificare le sessioni per azioni ad alto privilegio?
- [ ] No — le azioni ad alto privilegio o sensibili richiedono un token di sessione fresco o valido  
- [ ] Sì — una volta trovato un bypass su un endpoint, esso **può** essere utilizzato per eseguire azioni amministrative in tutta l'applicazione

---

## WSTG-ATHN-05 — Testing for Vulnerable Remember Password

La funzionalità "Remember Me" o di login persistente è progettata per mantenere lo stato autenticato di un utente attraverso i riavvii del browser, memorizzando un token o una credenziale sul lato client. Se questa funzionalità è implementata in modo errato — ad esempio memorizzando password in chiaro (plaintext), hash deboli o token a bassa entropia nei cookie — espone l'utente al furto dell'account (account takeover) tramite Cross-Site Scripting (XSS), accesso fisico o intercettazione di rete. I pentester valutano l'entropia di questi token di persistenza, i flag di sicurezza applicati al meccanismo di memorizzazione e il ciclo di vita della sessione per garantire che la comodità dell'accesso persistente non aggiri i controlli di sicurezza critici come la Multi-Factor Authentication (MFA) o l'invalidazione della sessione. L'Exploit spesso comporta la raccolta di questi token a lunga durata per ottenere un accesso non autorizzato agli account senza richiedere le credenziali originali.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se le credenziali in chiaro (plaintext) o gli hash senza salt (unsalted) vengono memorizzati nel cookie persistente, o se il token permette di bypassare la Multi-Factor Authentication (MFA).

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Strumenti:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### L'applicazione implementa una funzionalità "Remember Me" o "Resta collegato"?
- [ ] No — la funzionalità **non** è presente nell'applicazione  
- [ ] Sì — la funzionalità è presente e utilizza token crittograficamente forti e non prevedibili  
- [ ] Sì — la funzionalità è presente ma utilizza token prevedibili o a bassa entropia *(Media)*  
- [ ] Sì — la funzionalità è presente e memorizza credenziali in chiaro (plaintext) o password codificate in base64 *(Alta)*  

### I flag di sicurezza sono applicati correttamente al cookie persistente?
- [ ] Sì — i flag `HttpOnly`, `Secure` e `SameSite` sono **applicati**  
- [ ] Sì — alcuni flag sono applicati ma `HttpOnly` è **mancante**  
- [ ] Sì — alcuni flag sono applicati ma `Secure` è **mancante** su HTTPS  
- [ ] No — nessun flag di sicurezza è **applicato** al cookie persistente  

### Il token "Remember Me" rimane valido dopo un logout manuale?
- [ ] No — il token viene invalidato lato server immediatamente dopo il logout  
- [ ] Sì — il token rimane valido ma richiede una password per azioni sensibili  
- [ ] Sì — il token rimane valido e consente l'accesso completo all'account **senza** nuova autenticazione *(Media)*  

### La sessione persistente viene terminata in seguito a un cambio password?
- [ ] Sì — tutti i token persistenti vengono revocati quando l'utente cambia la propria password  
- [ ] No — il token "Remember Me" rimane valido dopo che un reset o un cambio password **è stato eseguito** *(Alta)*  

### Il token persistente aggira la Multi-Factor Authentication (MFA) nelle visite successive?
- [ ] No — la MFA è richiesta anche quando è presente un token "Remember Me"  
- [ ] Sì — la MFA viene aggirata, ma il token è legato a uno specifico fingerprint del dispositivo  
- [ ] Sì — la MFA viene **completamente aggirata** utilizzando solo il cookie persistente da qualsiasi dispositivo *(Alta)*

---

## WSTG-ATHN-06 — Test per la Debolezza della Cache del Browser

La debolezza della cache del browser si verifica quando informazioni sensibili vengono memorizzate nella cache locale del browser e possono essere recuperate da un utente non autorizzato con accesso alla stessa macchina fisica. Questa mancanza di adeguati header di controllo della cache consente a dati potenzialmente sensibili, come informazioni personali, dettagli dell'account o identificativi di sessione, di persistere dopo che l'utente ha effettuato il logout o ha chiuso la sessione. Gli attaccanti o gli utenti successivi su terminali condivisi o pubblici possono sfruttare questa vulnerabilità navigando nella cronologia del browser, utilizzando il pulsante "Indietro" o ispezionando i file della cache locale per esfiltrare le risposte memorizzate. Garantire l'implementazione di direttive rigorose come `Cache-Control: no-store` e `Pragma: no-cache` è fondamentale per qualsiasi applicazione che gestisca contenuti autenticati, al fine di assicurare che i dati sensibili non vengano mai scritti su disco.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media* |

> *La severità diventa Media se l'applicazione è consultata frequentemente da chioschi condivisi/pubblici o contiene dati PII/PHI altamente sensibili.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Strumenti:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Sono presenti header cache-control appropriati sulle pagine autenticate o sensibili?
- [ ] Sì — `Cache-Control: no-store, no-cache` e `Pragma: no-cache` sono **presenti** e **configurati correttamente**  
- [ ] Sì — alcuni header sono presenti ma `no-store` è **mancante**, consentendo il caching su disco  
- [ ] No — non è **applicato** alcun header relativo alla cache, affidandosi al comportamento predefinito del browser  

### L'applicazione utilizza la direttiva `private` per i dati specifici dell'utente?
- [ ] Sì — `Cache-Control: private` è **abilitato** per tutti i contenuti personalizzati per impedire il caching da parte dei proxy  
- [ ] No — il contenuto è contrassegnato come `public` o manca della direttiva `private`, rendendolo **archiviabile in cache** dai proxy intermedi  

### Le informazioni sensibili sono accessibili tramite il pulsante "Indietro" del browser dopo un logout riuscito?
- [ ] No — il browser richiede la ri-autenticazione o la pagina **non viene caricata** dalla cache locale  
- [ ] Sì — le informazioni sensibili sono **ancora visibili** tramite il pulsante Indietro dopo che la sessione è stata terminata  

### I file non-HTML sensibili (es. PDF, esportazioni CSV, risposte API JSON) sono protetti dal caching?
- [ ] No — non vengono generati o gestiti file non-HTML sensibili dall'applicazione  
- [ ] Sì — header rigorosi di controllo della cache sono **applicati** a tutti i download di file sensibili e agli endpoint API  
- [ ] No — i download sensibili o le risposte API vengono **memorizzati** nella cache locale del browser o in directory temporanee  

### L'applicazione utilizza `Expires: 0` o una data passata per impedire l'uso di dati obsoleti?
- [ ] Sì — l'header `Expires` è impostato su `0` o su una data passata, **garantendo** che il browser tratti il contenuto come immediatamente obsoleto  
- [ ] No — l'header `Expires` è **mancante** o impostato su una data futura per le pagine sensibili

---

## WSTG-ATHN-07 — Testing for Weak Password Policy

La verifica di criteri per le password deboli (Weak Password Policy) comporta la valutazione dei requisiti di complessità, lunghezza ed entropia imposti da un'applicazione durante la creazione dell'account e l'aggiornamento delle credenziali. Policy insufficienti compromettono direttamente la riservatezza degli account utente, rendendoli suscettibili ad attacchi di tipo Dictionary Attack automatizzati, tentativi di Brute Force e Credential Stuffing. Queste vulnerabilità risiedono tipicamente negli endpoint di registrazione, nei workflow di reset della password e nei pannelli di gestione amministrativa degli utenti. Dal punto di vista di un attaccante, una policy permissiva riduce significativamente il costo computazionale e il tempo necessario per compromettere con successo le credenziali utilizzando wordlist comuni o tecniche di cracking basate su pattern.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Bassa* |

> *La gravità diventa Alta se l'applicazione manca di meccanismi di Account Lockout o Rate Limiting per prevenire il guessing automatizzato.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Strumenti:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### È imposta dall'applicazione una lunghezza minima della password?
- [ ] Sì — la lunghezza minima è di 12+ caratteri *(Più sicuro)*  
- [ ] Sì — la lunghezza minima è compresa tra 8 e 11 caratteri  
- [ ] Sì — la lunghezza minima è **inferiore** a 8 caratteri  
- [ ] No — non viene imposta alcuna lunghezza minima  

### Sono imposti requisiti di complessità (maiuscole, minuscole, cifre, simboli)?
- [ ] Sì — più classi di caratteri sono obbligatorie e il bypass **non è possibile**  
- [ ] Sì — le classi di caratteri sono suggerite ma **non** imposte  
- [ ] No — non viene applicato alcun requisito di complessità  

### L'applicazione blocca password comuni/deboli o password contenenti dati specifici dell'utente?
- [ ] Sì — password deboli note (es. "password123") e password basate sull'username **non possono** essere utilizzate  
- [ ] Sì — le password comuni sono bloccate, ma i dati specifici dell'utente (es. username, email) **possono** essere utilizzati  
- [ ] No — viene accettata qualsiasi stringa che soddisfi i requisiti di lunghezza/complessità  

### I requisiti di complessità o lunghezza possono essere aggirati tramite modifiche lato client?
- [ ] No — le policy sono imposte rigorosamente lato **server-side**  
- [ ] Sì — le policy sono imposte tramite JavaScript e **possono** essere aggirate intercettando la richiesta  

### La password policy è comunicata chiaramente all'utente senza esporre dettagli di implementazione?
- [ ] Sì — la policy è visibile durante l'inserimento e fornisce messaggi di errore generici  
- [ ] Sì — la policy è visibile ma i messaggi di errore rivelano regex specifiche o lacune logiche  
- [ ] No — la policy **non** è visibile, costringendo a una scoperta per tentativi (trial-and-error)

---

## WSTG-ATHN-08 — Verifica di risposte deboli alle domande di sicurezza

La verifica delle risposte deboli alle domande di sicurezza comporta la valutazione dei meccanismi di autenticazione basata sulla conoscenza (Knowledge-Based Authentication - KBA) utilizzati per verificare l'identità di un utente, tipicamente durante il recupero della password o i flussi di autenticazione a più fattori. Questo controllo è rilevante poiché le domande di sicurezza spesso si basano su informazioni statiche e non segrete che possono essere facilmente scoperte tramite Open-Source Intelligence (OSINT) o Social Engineering, portando a una sottrazione non autorizzata dell'account (Account Takeover). Queste vulnerabilità si verificano solitamente nei moduli "Password dimenticata" o "Sblocco account" in cui gli utenti forniscono risposte a domande preimpostate. Dal punto di vista di un attaccante, questo è un bersaglio primario per attacchi Brute Force automatizzati o ricerche mirate, poiché molti utenti forniscono risposte prevedibili o pubblicamente verificabili a domande comuni come "Qual è il nome da nubile di tua madre?" o "Quale scuola superiore hai frequentato?".


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se la domanda di sicurezza è l'unico requisito per un reset completo della password e l'account takeover senza ulteriore verifica.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Strumenti:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### L'applicazione implementa domande di sicurezza per la verifica dell'identità o il recupero della password?
- [ ] No — le domande di sicurezza **non sono utilizzate** per l'autenticazione o il recupero  
- [ ] Sì — le domande di sicurezza **sono utilizzate** come fattore secondario opzionale  
- [ ] Sì — le domande di sicurezza **sono utilizzate** come meccanismo di recupero primario/unico *(Alto Rischio)*  

### Le domande di sicurezza sono selezionate da un elenco fisso o sono definite dall'utente?
- [ ] No — le domande sono interamente definite dall'utente e richiedono un'elevata entropia  
- [ ] Sì — le domande sono fisse ma includono opzioni non individuabili tramite OSINT  
- [ ] Sì — le domande provengono da un elenco fisso di argomenti comuni individuabili tramite OSINT *(Vulnerabile)*  

### Vengono applicati il Rate Limiting o il blocco dell'account ai tentativi di risposta alle domande di sicurezza?
- [ ] Sì — vengono applicati un rigoroso Rate Limiting e il blocco dell'account per prevenire il Brute Force  
- [ ] Sì — il Rate Limiting **è applicato** ma l'aggiramento **è possibile** tramite rotazione degli IP o manipolazione degli header  
- [ ] No — **nessun Rate Limiting** è imposto sui tentativi di risposta  

### L'applicazione impone requisiti di complessità o validazione sul contenuto della risposta?
- [ ] Sì — la validazione impedisce l'uso di parole comuni e garantisce la complessità/univocità della risposta  
- [ ] Sì — è presente una validazione di base ma **può** essere aggirata con wordlist comuni o attacchi a dizionario  
- [ ] No — **non viene eseguita alcuna validazione**; sono ammesse risposte semplici o a carattere singolo  

### La sfida della domanda di sicurezza può essere aggirata attraverso falle logiche?
- [ ] No — il flusso di recupero richiede rigorosamente una risposta valida per procedere  
- [ ] Sì — il flusso di recupero **può** essere aggirato manipolando i codici di risposta HTTP o i parametri di sessione  
- [ ] Sì — la risposta è riflessa nel codice sorgente o nei campi nascosti della pagina

---

## WSTG-ATHN-09 — Testing for Weak Password Change or Reset Functionalities

I meccanismi di modifica e reset della password sono componenti critiche della gestione dell'autenticazione che, se implementate in modo errato, consentono agli attaccanti di compromettere gli account utente. Queste vulnerabilità solitamente coinvolgono token di reset prevedibili, mancanza di account lockouts durante i tentativi di Brute Force, invio non sicuro dei token o la possibilità di modificare le password senza verificare la password corrente. Gli attaccanti sfruttano queste falle intercettando o indovinando i link di recupero, eseguendo Host Header Injection per reindirizzare le email di reset, o utilizzando la Session Fixation per mantenere l'accesso dopo un cambio password. Garantire che questi processi richiedano una verifica robusta e utilizzino la casualità crittografica è essenziale per mantenere l'integrità degli account e prevenire l'accesso non autorizzato.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### È richiesta la password corrente per impostare una nuova password durante una modifica standard?
- [ ] Sì — la password corrente **è richiesta** e verificata  
- [ ] Sì — la password corrente **è richiesta** ma può essere bypassata tramite Parameter Pollution o manipolazione  
- [ ] No — la password corrente **non è** richiesta, consentendo l'account takeover tramite Session Hijacking *(Alta)*  

### I token di reset della password sono crittograficamente sicuri e non prevedibili?
- [ ] Sì — i token sono ad alta entropia, casuali e **non prevedibili**  
- [ ] Sì — i token vengono generati ma mostrano bassa entropia o pattern prevedibili (es. basati su timestamp)  
- [ ] No — i token sono **prevedibili** o basati su dati utente statici come email/ID codificati in Base64 *(Critica)*  

### Il link di reset della password può essere reindirizzato tramite Host Header Injection?
- [ ] No — l'applicazione ignora o convalida l'header `Host` per la generazione dei link  
- [ ] Sì — l'applicazione utilizza l'header `Host` o `X-Forwarded-Host` per costruire i link, ma la validazione **è presente**  
- [ ] Sì — i link **possono** essere reindirizzati verso un dominio controllato dall'attaccante tramite la manipolazione degli header *(Alta)*  

### Il token di reset ha un ciclo di vita limitato e un'invalidazione immediata?
- [ ] Sì — i token scadono rapidamente e vengono invalidati **immediatamente** dopo l'uso o la modifica della password  
- [ ] Sì — i token scadono dopo una lunga durata (es. 24+ ore) o consentono **utilizzi multipli**  
- [ ] No — i token **non scadono mai** o rimangono validi dopo che la password è stata modificata con successo  

### Sono presenti Rate Limiting o blocchi (lockouts) sugli endpoint di reset e modifica della password?
- [ ] Sì — Rate Limiting rigoroso e CAPTCHA **sono applicati** per prevenire l'enumerazione e il Brute Force  
- [ ] Sì — esistono controlli limitati ma il bypass **è possibile** tramite rotazione degli IP o header spoofing  
- [ ] No — nessun Rate Limiting **è applicato**, consentendo l'enumerazione massiva degli account o il Brute Force dei token

---

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

## WSTG-ATHN-11 — Testing Multi-Factor Authentication (MFA)

Il testing della Multi-Factor Authentication (MFA) valuta la robustezza del secondo livello di sicurezza progettato per prevenire l'accesso non autorizzato anche quando le credenziali primarie sono compromesse. Gli attaccanti tentano frequentemente di bypassare la MFA identificando endpoint in cui non è applicata in modo coerente, come versioni legacy delle API, backend mobile o flussi di ripristino password. L'Exploit spesso comporta la manipolazione delle risposte del server, il Brute Force di codici a breve durata (OTP) o lo sfruttamento di race conditions e falle nella gestione delle sessioni che consentono a un utente di passare a uno stato autenticato senza fornire il secondo fattore.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### La MFA è applicata in modo coerente su tutti i portali di autenticazione?
- [ ] Sì — la MFA è richiesta per tutti i tentativi di login via web, mobile e basati su API  
- [ ] Sì — ma la MFA **non è applicata** su endpoint specifici (es. `/api/v1/login` legacy o portali specifici per il mobile)  
- [ ] No — la MFA **non è implementata** nell'applicazione *(Critica)*  

### È possibile bypassare la fase di verifica MFA tramite navigazione diretta dell'URL o manipolazione della risposta?
- [ ] No — la navigazione diretta o il Parameter Tampering **non possono** bypassare la challenge  
- [ ] Sì — la navigazione diretta verso URL di dashboard interne **è possibile** senza completare la challenge MFA  
- [ ] Sì — la manipolazione della risposta del server (es. cambiare un `401 Unauthorized` in `200 OK`) **è possibile** per ottenere l'accesso  

### Il processo di verifica del codice MFA è protetto contro attacchi di Brute Force?
- [ ] Sì — un Rate Limiting rigoroso o il blocco dell'account **viene applicato** dopo molteplici tentativi falliti di OTP  
- [ ] Sì — il Rate Limiting esiste ma **è possibile** bypassarlo tramite rotazione degli IP o manipolazione degli header  
- [ ] No — non viene applicato alcun Rate Limiting e il Brute Force automatizzato dei codici **è possibile**  

### L'applicazione mantiene uno stato di sessione sicuro durante la transizione MFA?
- [ ] Sì — i token di sessione con privilegi elevati vengono rilasciati **solo dopo** il completamento con successo della MFA  
- [ ] No — un cookie di sessione completamente funzionale viene rilasciato **prima** che la MFA sia completata, consentendo l'accesso ad alcune funzionalità  
- [ ] No — l'applicazione utilizza un identificatore statico o una transizione di sessione prevedibile che **può** essere soggetta a Hijacking  

### I fattori MFA alternativi (SMS, Email, Codici di Backup) sono vulnerabili a Exploit?
- [ ] No — tutti i fattori utilizzano valori sicuri, non prevedibili e limitati nel tempo  
- [ ] Sì — i codici di backup sono prevedibili o **possono** essere enumerati  
- [ ] Sì — la funzionalità "Reinvia Codice" può essere abusata per eseguire flooding di SMS/Email o rivelare dettagli di contatto parziali

---

## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Le vulnerabilità di Directory Traversal e File Inclusion si verificano quando un'applicazione utilizza input controllabili dall'utente per costruire percorsi verso file o directory senza una sufficiente validazione o sanitizzazione. Gli attaccanti sfruttano queste falle iniettando sequenze come `../` per navigare al di fuori della directory prevista, accedendo potenzialmente a file di sistema sensibili, dati di configurazione o codice sorgente dell'applicazione. In casi più gravi che coinvolgono Local File Inclusion (LFI) o Remote File Inclusion (RFI), un attaccante può ottenere la remote code execution includendo script malevoli o sfruttando il log poisoning e i wrapper PHP. Queste vulnerabilità risiedono tipicamente nei parametri utilizzati per il caricamento dinamico di contenuti, motori di template o endpoint di recupero immagini in cui la logica lato server gestisce i percorsi del file system.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Strumenti:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### I parametri accettano nomi di file o percorsi per l'elaborazione lato server?
- [ ] No — nessun parametro sembra interagire con il file system  
- [ ] Sì — i parametri esistono ma utilizzano una allowlist rigorosa di identificatori di file  
- [ ] Sì — i parametri accettano nomi di file o percorsi diretti  

### L'input viene sanitizzato per prevenire sequenze di directory traversal?
- [ ] Sì — l'input è validato rispetto a una allowlist rigorosa e le sequenze di traversal **non sono possibili**  
- [ ] Sì — l'input viene sanitizzato rimuovendo le sequenze `../`, ma il bypass ricorsivo **è possibile**  
- [ ] No — nessuna sanitizzazione o validazione **viene applicata** all'input relativo ai percorsi  

### È possibile accedere a file esterni alla directory limitata tramite sequenze di traversal?
- [ ] No — l'applicazione o il sistema operativo impediscono l'accesso ai file al di fuori dell'ambito definito  
- [ ] Sì — l'accesso ai file all'interno della web root **è possibile**  
- [ ] Sì — l'accesso a file di sistema sensibili (es. `/etc/passwd`, `C:\Windows\win.ini`) **è possibile** *(Critico)*  

### Il server consente l'inclusione di URL remoti (RFI)?
- [ ] No — la Remote File Inclusion è **disabilitata** a livello di server/applicazione  
- [ ] Sì — i file remoti **possono** essere inclusi ma l'esecuzione **non è possibile**  
- [ ] Sì — i file remoti **possono** essere inclusi ed eseguiti sul server *(Critico)*  

### I filtri possono essere aggirati utilizzando codifiche o caratteri speciali?
- [ ] No — i filtri gestiscono efficacemente varie codifiche e null byte  
- [ ] Sì — il bypass **è possibile** utilizzando URL encoding, double URL encoding o Unicode a 16 bit  
- [ ] Sì — il bypass **è possibile** utilizzando l'iniezione di null byte (`%00`) o wrapper del filesystem (es. `php://filter`)

---

## WSTG-ATHZ-02 — Testing for Bypassing Authorization Schema

L'aggiramento degli schemi di autorizzazione si verifica quando un'applicazione non riesce a imporre correttamente i controlli di accesso, consentendo a un utente malintenzionato di accedere a risorse o funzioni al di fuori dei permessi previsti. Questa vulnerabilità si manifesta tipicamente come Horizontal Privilege Escalation, dove un attaccante accede a dati appartenenti a un altro utente dello stesso livello, o Vertical Privilege Escalation, dove un utente con privilegi limitati ottiene capacità amministrative. Gli attaccanti sfruttano queste falle manipolando i parametri delle richieste, come i resource IDs, modificando i session tokens o utilizzando header HTTP come `X-Original-URL` per eludere le restrizioni basate sul percorso. Garantire un'autorizzazione robusta è fondamentale per mantenere la riservatezza dei dati e prevenire azioni amministrative non autorizzate che potrebbero compromettere l'intero sistema.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Strumenti:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### L'escalation orizzontale dei privilegi è impedita tra utenti dello stesso ruolo?
- [ ] Sì — i controlli di autorizzazione **sono applicati** a ogni richiesta di risorsa e il bypass **non è possibile**  
- [ ] Sì — i controlli di autorizzazione **sono applicati** ma possono essere aggirati tramite IDOR o parameter tampering  
- [ ] No — gli utenti **possono** accedere ai dati appartenenti ad altri utenti semplicemente modificando un resource ID *(Alta)*  

### L'escalation verticale dei privilegi è impedita per gli utenti con privilegi limitati?
- [ ] No — l'applicazione ha un solo livello di privilegio  
- [ ] Sì — le funzioni amministrative **non sono accessibili** agli utenti con privilegi limitati  
- [ ] Sì — le funzioni amministrative sono nascoste nell'interfaccia utente (UI) ma **sono accessibili** tramite URL diretto  
- [ ] No — gli utenti con privilegi limitati **possono** eseguire azioni amministrative manipolando ruoli o permessi *(Critica)*  

### L'autorizzazione può essere aggirata utilizzando l'HTTP verb tampering?
- [ ] No — i controlli di accesso sono imposti indipendentemente dal metodo HTTP utilizzato  
- [ ] Sì — la modifica del metodo (ad es. da `GET` a `POST` o `PUT`) **aggira** i controlli di autorizzazione  

### Gli endpoint amministrativi o riservati sono accessibili senza alcuna autenticazione?
- [ ] No — tutti gli endpoint sensibili richiedono una sessione valida e permessi appropriati  
- [ ] Sì — alcuni endpoint amministrativi sono accessibili se l'attaccante conosce l'URL diretto  
- [ ] Sì — l'accesso non autenticato a funzioni sensibili **è possibile** *(Critica)*  

### Gli header HTTP consentono l'elusione dell'autorizzazione basata sul percorso (path-based)?
- [ ] No — l'applicazione **non è** suscettibile a bypass basati sugli header  
- [ ] Sì — header come `X-Original-URL`, `X-Rewrite-URL` o `X-Forwarded-For` **possono** essere utilizzati per aggirare i controlli di accesso

---

## WSTG-ATHZ-03 — Testing for Privilege Escalation

Il privilege escalation si verifica quando un utente malintenzionato sfrutta delle vulnerabilità per ottenere l'accesso a risorse o funzionalità riservate a utenti con livelli di autorizzazione superiori o identità diverse. Nell'escalation verticale (vertical escalation), un utente standard tenta di accedere a funzioni amministrative, mentre l'escalation orizzontale (horizontal escalation) comporta l'accesso a dati appartenenti a un altro utente con lo stesso livello di privilegi. Questi difetti si manifestano tipicamente in access control lists (ACL) implementate in modo errato, insecure direct object references (IDOR) o nella manipolazione dei parametri all'interno dei token di sessione o di identità. Dal punto di vista di un attaccante, ciò si ottiene spesso manipolando ID numerici, modificando parametri basati sui ruoli nei cookie o nei JWT, o chiamando direttamente endpoint API nascosti che mancano di controlli di autorizzazione lato server.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Strumenti:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### L'applicazione implementa più livelli di privilegio o il multi-tenancy?
- [ ] No — l'applicazione è un sistema a utente singolo o a ruolo singolo  
- [ ] Sì — sono definiti più ruoli o tenant e **richiedono** test di autorizzazione  

### È possibile un privilege escalation orizzontale (accesso allo stesso livello)?
- [ ] No — i controlli di autorizzazione **sono applicati** a tutti gli identificatori di risorse e ai proprietari delle sessioni  
- [ ] Sì — l'accesso ai dati di altri utenti **è possibile** tramite IDOR o manipolazione dei parametri  
- [ ] Sì — il data leakage **è possibile** ma la modifica dei dati di altri utenti **non è possibile**  

### È possibile un privilege escalation verticale (da basso ad alto)?
- [ ] No — gli endpoint amministrativi applicano rigorosamente i controlli di accesso basati sui ruoli (RBAC)  
- [ ] Sì — le funzioni amministrative **possono** essere accessibili da utenti con privilegi bassi tramite l'accesso diretto agli URL  
- [ ] Sì — le funzioni amministrative **possono** essere accessibili manipolando header relativi ai ruoli, cookie o claim JWT  

### I ruoli o i permessi degli utenti possono essere modificati tramite Mass Assignment o Parameter Pollution?
- [ ] No — i campi basati sui ruoli sono rigorosamente in sola lettura e **non possono** essere modificati dagli utenti  
- [ ] Sì — i permessi dell'utente **possono** essere elevati includendo parametri nascosti (ad es. `role=admin`) negli aggiornamenti del profilo o nella registrazione  

### I controlli di autorizzazione sono applicati in modo coerente in tutte le versioni delle API e nei metodi HTTP?
- [ ] Sì — i controlli **sono applicati** in modo coerente in tutte le versioni e i metodi  
- [ ] No — le versioni legacy delle API (ad es. `/v1/`) o specifici metodi HTTP (ad es. `PUT`, `DELETE`, `PATCH`) **bypassano** l'autorizzazione  
- [ ] No — le funzioni amministrative sono nascoste nell'interfaccia utente ma **abilitate** a livello API per tutti gli utenti

---

## WSTG-ATHZ-04 — Testing for Insecure Direct Object References

Le Insecure Direct Object References (IDOR) si verificano quando un'applicazione fornisce l'accesso diretto agli oggetti in base all'input fornito dall'utente senza eseguire un controllo di autorizzazione per verificare i permessi del richiedente. Questa vulnerabilità ha un impatto significativo sulla riservatezza e sull'integrità, poiché consente agli aggressori di visualizzare, modificare o eliminare dati appartenenti ad altri utenti o al sistema. Si manifesta tipicamente nei parametri URL, nei dati del corpo POST o nelle chiavi JSON che si riferiscono a chiavi interne del database, nomi di file o identificatori di account. Dal punto di vista di un aggressore, l'exploitation comporta la manipolazione di questi identificatori — spesso attraverso l'incremento di numeri interi o il fuzzing di UUID — per accedere a risorse al di fuori del proprio ambito autorizzato.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Strumenti:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### L'applicazione utilizza identificatori diretti di oggetti nei parametri della richiesta?
- [ ] No — l'applicazione utilizza riferimenti indiretti o mappe crittografate *(Più sicuro)*  
- [ ] Sì — l'applicazione utilizza identificatori non prevedibili (es. UUID lunghi/hash)  
- [ ] Sì — l'applicazione utilizza identificatori prevedibili (es. interi incrementali o nomi utente)  

### Viene eseguita l'autorizzazione lato server per ogni richiesta che coinvolge un identificatore di oggetto?
- [ ] Sì — i controlli lato server **non possono** essere bypassati per alcun identificatore  
- [ ] Sì — i controlli lato server sono presenti ma il bypass **è possibile** tramite parameter pollution o manipolazione degli header  
- [ ] No — **non viene applicato** alcun controllo di autorizzazione per verificare la proprietà dell'oggetto richiesto  

### Un aggressore può accedere o modificare oggetti appartenenti ad altri utenti (Horizontal IDOR)?
- [ ] No — l'accesso ai dati di altri utenti **non è possibile**  
- [ ] Sì — la visualizzazione dei dati di altri utenti **è possibile** ma la modifica **non è possibile**  
- [ ] Sì — sia la visualizzazione che la modifica dei dati di altri utenti **sono possibili** *(Critico)*  

### È possibile accedere a oggetti di livello amministrativo o di sistema (Vertical IDOR)?
- [ ] No — l'accesso a oggetti con privilegi superiori **non è possibile**  
- [ ] Sì — l'accesso a configurazioni amministrative o file di sistema **è possibile**  

### L'applicazione espone identificatori di oggetti tramite ricerca, metadati o altri endpoint?
- [ ] No — gli identificatori **non sono esposti** involontariamente  
- [ ] Sì — gli identificatori di altri utenti/oggetti **possono** essere enumerati tramite endpoint secondari o profili pubblici

---

## WSTG-ATHZ-05 — Verifica delle vulnerabilità OAuth

Le debolezze di OAuth comprendono una varietà di falle nell'implementazione dei protocolli OAuth 2.0 o OpenID Connect (OIDC), che spesso risultano in un account takeover completo o nell'accesso non autorizzato ai dati. Queste vulnerabilità si manifestano tipicamente durante il flusso di autorizzazione, in particolare per quanto riguarda la validazione del `redirect_uri`, l'entropia e la verifica del parametro `state`, e la gestione sicura dei token di accesso o ID. Gli attaccanti le sfruttano intercettando gli authorization code, reindirizzando i token verso domini controllati dall'attaccante tramite open redirect, o eseguendo Cross-Site Request Forgery (CSRF) per collegare la sessione di una vittima all'account dell'attaccante. Poiché OAuth funge spesso da meccanismo di autenticazione primario, una singola configurazione errata può compromettere l'integrità dell'intero sistema di identità degli utenti.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Strumenti:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Il parametro `state` è implementato e validato per prevenire attacchi CSRF?
- [ ] Sì — `state` è obbligatorio, univoco per sessione e validato rigorosamente sul server  
- [ ] Sì — `state` è presente ma è statico o prevedibile tra diverse sessioni  
- [ ] Sì — `state` è presente ma l'applicazione **non** lo valida al momento del callback  
- [ ] No — il parametro `state` **non** viene utilizzato nella richiesta di autorizzazione *(Critico)*  

### Il `redirect_uri` è validato rigorosamente rispetto a una whitelist?
- [ ] Sì — vengono **accettate** solo corrispondenze esatte rispetto a una whitelist pre-registrata  
- [ ] Sì — la validazione è applicata ma il bypass **è possibile** tramite path traversal o manipolazione del sottodominio  
- [ ] Sì — la validazione è applicata ma il bypass **è possibile** tramite parameter pollution o fragment injection  
- [ ] No — l'applicazione **consente** URL arbitrari nel parametro `redirect_uri` *(Critico)*  

### Il `response_type` può essere manipolato per alterare il flusso?
- [ ] No — l'applicazione accetta solo il flusso previsto (es. `code`) e rifiuta gli altri  
- [ ] Sì — il passaggio da `code` a `token` (Implicit flow) **è possibile** e causa la perdita di token nell'URL  
- [ ] Sì — i flussi ibridi o valori di `response_type` non autorizzati sono **abilitati** ed elaborati  

### I token di accesso o gli authorization code vengono esposti tramite gli header Referer o la cronologia del browser?
- [ ] No — i token/code sono gestiti in modo sicuro e le pagine sensibili utilizzano una `Referrer-Policy` appropriata  
- [ ] Sì — i token/code **vengono** esposti a domini di terze parti tramite l'header `Referer`  
- [ ] Sì — i token/code **sono** visibili nella cronologia del browser a causa di caching non sicuro o richieste GET  

### L'applicazione applica il principio del "Minimo Privilegio" per gli scope OAuth?
- [ ] Sì — l'applicazione richiede solo gli scope minimi necessari per la funzionalità  
- [ ] No — l'applicazione richiede scope eccessivi o amministrativi per impostazione predefinita  
- [ ] No — l'utente **non può** revisionare o escludere scope specifici durante la fase di consenso

---

## WSTG-SESS-01 — Testing for Session Management Schema

Il testing dello schema di gestione della sessione si concentra sull'analisi di come l'applicazione gestisce il ciclo di vita e la struttura degli identificativi di sessione per garantire che siano robusti contro la predizione e l'hijacking. Gli attaccanti acquisiscono molteplici session token per eseguire analisi statistiche, alla ricerca di pattern, bassa entropia o incrementi prevedibili che permettano di falsificare sessioni valide per altri utenti. Le debolezze nello schema si manifestano spesso come vulnerabilità di session fixation o attraverso l'uso di valori non sufficientemente casuali, tipicamente presenti nei cookie HTTP, nei parametri URL o nelle implementazioni di header personalizzati. Compromettere lo schema di sessione è un attacco ad alto impatto che garantisce a un avversario l'accesso completo allo stato autenticato di una vittima senza necessità delle sue credenziali primarie.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Strumenti:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### L'identificativo di sessione è sufficientemente complesso e casuale?
- [ ] Sì — il token supera i test di casualità statistica (alta entropia) e il bypass **non è possibile**  
- [ ] Sì — il token è lungo ma contiene pattern prevedibili o segmenti statici  
- [ ] No — il token è sequenziale, basato su timestamp prevedibili o ha un'entropia **criticante bassa** *(Critico)*  

### Dove viene trasmesso e memorizzato l'identificativo di sessione?
- [ ] L'identificativo è memorizzato in un cookie `Secure` e `HttpOnly` *(Più sicuro)*  
- [ ] L'identificativo è memorizzato in un cookie ma mancano i flag `Secure` o `HttpOnly`  
- [ ] L'identificativo è trasmesso **solo** tramite parametri URL o richieste `GET` *(Alto Rischio)*  

### L'applicazione emette un nuovo identificativo di sessione dopo un'autenticazione riuscita?
- [ ] Sì — un nuovo ID di sessione univoco **viene generato** immediatamente dopo il login  
- [ ] No — l'ID di sessione rimane lo stesso prima e dopo il login *(Session Fixation possibile)*  

### L'identificativo di sessione è legato a uno specifico indirizzo IP o User-Agent per prevenirne il riutilizzo?
- [ ] Sì — la sessione viene invalidata se il fingerprint del client cambia significativamente  
- [ ] No — la sessione **può** essere riutilizzata su diversi indirizzi IP o dispositivi senza verifica aggiuntiva  

### Gli identificativi di sessione vengono correttamente invalidati lato server dopo il logout o il timeout?
- [ ] Sì — lo stato della sessione lato server viene **completamente distrutto** al logout  
- [ ] No — la sessione rimane valida sul server anche se il cookie lato client viene eliminato  
- [ ] No — la sessione **non scade mai** o ha un periodo di timeout eccessivamente lungo

---

## WSTG-SESS-02 — Test degli attributi dei cookie

La gestione delle sessioni si basa spesso sui cookie HTTP per mantenere lo stato, rendendo gli attributi di sicurezza di questi cookie un obiettivo primario per gli attaccanti. Omettendo o configurando in modo errato attributi come `HttpOnly`, `Secure` e `SameSite`, le applicazioni espongono i token di sessione al furto tramite Cross-Site Scripting (XSS), all'intercettazione su canali non crittografati o ad attacchi di Cross-Site Request Forgery (CSRF). I pentester esaminano questi flag per determinare se un attaccante può esfiltrare gli identificatori di sessione dal Document Object Model (DOM) del browser o forzare il browser a inviarli in richieste cross-origin. Una corretta configurazione degli attributi è una misura fondamentale di defense-in-depth per garantire che i token sensibili rimangano limitati a contesti autorizzati e sicuri.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Strumenti:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analisi dell'implementazione dei flag dei cookie
| Attributo | Presente | Mancante |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Il flag `HttpOnly` è applicato ai cookie di sessione sensibili?
- [ ] Sì — applicato a **tutti** i cookie di sessione sensibili *(Massima sicurezza)*  
- [ ] Sì — applicato solo ad alcuni cookie di sessione  
- [ ] No — il flag **non è applicato**, consentendo l'accesso tramite script client-side *(Critico)*  

### Il flag `Secure` è applicato per i cookie trasmessi via HTTPS?
- [ ] Sì — applicato a **tutti** i cookie trasmessi via TLS  
- [ ] No — il flag **non è applicato**, consentendo l'invio dei cookie tramite HTTP in chiaro  

### L'attributo `SameSite` è utilizzato per mitigare i rischi CSRF?
- [ ] Sì — impostato su `Strict` o `Lax` per i cookie di sessione  
- [ ] Sì — impostato su `None` ma con il flag `Secure` presente  
- [ ] No — l'attributo è **mancante** o impostato su `None` **senza** `Secure`  

### Gli attributi `Domain` e `Path` sono limitati all'ambito minimo necessario?
- [ ] Sì — limitati al sottodominio **specifico** e al percorso dell'applicazione  
- [ ] No — l'ambito è troppo **ampio** (es. impostato su un dominio genitore o sulla root `/`), aumentando la superficie di attacco  

### I cookie di sessione sensibili scadono correttamente tramite gli attributi `Expires` o `Max-Age`?
- [ ] Sì — sono impostate date di scadenza appropriate  
- [ ] No — i cookie sono persistenti con una durata eccessivamente **lunga**  
- [ ] No — i cookie sono solo di sessione ma **mancano** dell'imposizione del timeout lato server

---

## WSTG-SESS-03 — Session Fixation

La Session Fixation si verifica quando un'applicazione non riesce a invalidare o ruotare l'identificativo di sessione dopo che un utente si è autenticato con successo, consentendo a un attaccante di forzare un token di sessione noto su una vittima. Se l'applicazione preserva il Session ID di pre-autenticazione dopo il login, un attaccante può fornire a una vittima un link appositamente creato contenente un Session ID fisso e successivamente dirottare (hijack) la sessione autenticata. Questa vulnerabilità si manifesta tipicamente nei form di login o tramite identificativi di sessione accettati attraverso parametri URL e cookie. Dal punto di vista di un attaccante, l'Exploit comporta il "fissaggio" della sessione tramite un link malevolo o una vulnerabilità di Header Injection e l'attesa che la vittima fornisca le proprie credenziali, bypassando efficacemente la necessità di rubare un token attivo.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### L'identificativo di sessione cambia dopo l'autenticazione riuscita?
- [ ] Sì — un nuovo identificativo di sessione **viene emesso** e quello precedente viene invalidato *(Più sicuro)*  
- [ ] Sì — un nuovo identificativo di sessione **viene emesso** ma quello precedente **rimane valido**  
- [ ] No — l'identificativo di sessione **rimane lo stesso** prima e dopo l'autenticazione *(Critico)*  

### L'applicazione accetta identificativi di sessione forniti tramite parametri URL?
- [ ] No — gli ID di sessione sono gestiti **esclusivamente** tramite cookie  
- [ ] Sì — gli ID di sessione sono accettati nell'URL ma vengono **ignorati** o **ruotati** al momento del login  
- [ ] Sì — gli ID di sessione nell'URL vengono **accettati** e **mantenuti** dopo l'autenticazione  

### È possibile forzare nell'applicazione un Session ID definito dall'attaccante?
- [ ] No — l'applicazione **rifiuta** ID di sessione non validi o inesistenti forniti dall'utente  
- [ ] Sì — l'applicazione **accetta** e inizializza una sessione utilizzando un ID arbitrario fornito in un cookie  
- [ ] Sì — l'applicazione **accetta** e inizializza una sessione utilizzando un ID arbitrario fornito in un parametro URL  

### Le sessioni di pre-autenticazione vengono terminate correttamente?
- [ ] Sì — la sessione anonima viene **completamente distrutta** all'evento di login  
- [ ] No — la sessione anonima **non viene distrutta**, consentendo un potenziale harvesting delle sessioni  

### Il cookie di sessione è protetto contro l'injection lato client?
- [ ] Sì — i flag `HttpOnly` e `Secure` sono **applicati** per prevenire la fixation basata su script  
- [ ] No — i flag **non sono applicati**, consentendo l'impostazione degli Session ID tramite Cross-Site Scripting (XSS)

---

## WSTG-SESS-04 — Testing for Exposed Session Variables

Le variabili di sessione esposte si verificano quando identificativi di sessione sensibili o dati relativi allo stato vengono trasmessi tramite canali insicuri come le URL query string, gli header Referer o i log di sistema. Questa esposizione aumenta significativamente il rischio di Session Hijacking, poiché gli identificativi possono essere memorizzati nella cache della cronologia del browser, registrati dai proxy intermedi o trapelati a siti di terze parti tramite l'header Referer. I pentester identificano queste esposizioni monitorando tutte le richieste in uscita e ispezionando i log dell'applicazione o le configurazioni del server per individuare fughe di dati accidentali. Nel mondo reale, gli aggressori sfruttano queste falle raccogliendo Session Token validi da macchine condivise o analizzando i pattern di traffico per eludere l'autenticazione e ottenere l'accesso non autorizzato agli account utente.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se la variabile esposta è un Session Token che consente il subentro immediato nell'account (Account Takeover).

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Strumenti:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### L'identificativo di sessione viene trasmesso nella URL query string?
- [ ] No — gli identificativi di sessione **non** sono presenti nella URL query string  
- [ ] Sì — gli identificativi sono presenti ma la sessione è di breve durata o a basso rischio  
- [ ] Sì — identificativi di sessione univoci **sono** presenti nella URL query string *(Critico)*  

### L'applicazione espone i Session Token tramite l'header Referer?
- [ ] No — la `Referrer-Policy` impedisce la fuga di dati o non esistono link verso terze parti  
- [ ] Sì — i Session Token **vengono** trasmessi a domini di terze parti tramite l'header Referer  

### Le variabili di sessione sono memorizzate nei log lato server o dell'applicazione?
- [ ] No — le variabili di sessione sono mascherate o escluse dai log  
- [ ] Sì — le variabili di sessione **vengono** registrate in chiaro nei log dell'applicazione o del server web  

### L'applicazione utilizza richieste GET per operazioni che modificano lo stato?
- [ ] No — l'applicazione utilizza `POST` o altri metodi non idempotenti per operazioni sensibili  
- [ ] Sì — viene utilizzato `GET`, causando il caching dei dati di sessione nella cronologia del browser e nei proxy intermedi  

### L'header `Cache-Control` è configurato per impedire il caching dei dati relativi alla sessione?
- [ ] Sì — `Cache-Control: no-store` (o simile) **è applicato** alle risposte sensibili  
- [ ] Sì — i controlli sono presenti ma l'elusione **è possibile** tramite casi limite del browser  
- [ ] No — le risposte sensibili contenenti dati di sessione **possono** essere memorizzate nella cache dal browser

---

## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Il Cross-Site Request Forgery (CSRF) è una vulnerabilità in cui un attaccante induce il browser della vittima a eseguire un'azione indesiderata su un sito web differente nel quale la vittima è attualmente autenticata. Questo exploit sfrutta il comportamento del browser di allegare automaticamente le credenziali "ambientali", come i cookie di sessione o gli Authorization headers, alle richieste in uscita. Gli attaccanti solitamente mirano a operazioni che modificano lo stato (state-changing operations), come il cambio della password, l'aggiornamento degli indirizzi email o l'esecuzione di trasferimenti finanziari, ospitando script malevoli o moduli nascosti su un sito di terze parti. Una corretta esecuzione dell'exploit può portare al completo account takeover o alla modifica non autorizzata dei dati senza la conoscenza o il consenso dell'utente.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Media / Alta* |

> *La severità diventa Alta se l'azione vulnerabile consente l'account takeover, la privilege escalation o transazioni finanziarie non autorizzate.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Strumenti:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### I token anti-CSRF sono implementati per le richieste che modificano lo stato?
- [ ] Sì — token unici e crittograficamente forti sono richiesti per tutte le azioni che modificano lo stato  
- [ ] Sì — i token sono presenti ma **non** sono unici per sessione o sono prevedibili  
- [ ] No — i token anti-CSRF **non** sono implementati  

### La validazione lato server del token anti-CSRF è robusta?
- [ ] Sì — il server valida rigorosamente il token e il bypass **non è possibile**  
- [ ] Sì — la validazione viene eseguita ma **può** essere bypassata rimuovendo il parametro del token  
- [ ] Sì — la validazione viene eseguita ma **può** essere bypassata fornendo un token vuoto o fittizio (dummy token)  
- [ ] Sì — la validazione viene eseguita ma il token **non** è legato alla sessione dell'utente  

### L'applicazione si affida a metodi facilmente aggirabili per la protezione CSRF?
- [ ] No — l'applicazione non si affida esclusivamente a header deboli o controlli dell'origine  
- [ ] Sì — la protezione si basa esclusivamente sull'header `Referer` o `Origin` che **può** essere falsificato (spoofed) o rimosso  
- [ ] Sì — la protezione si basa sul controllo dell'header `X-Requested-With` che **può** essere bypassato tramite configurazioni errate di CORS  

### I cookie di sessione sono configurati con l'attributo `SameSite`?
- [ ] Sì — `SameSite` è impostato su `Strict` o `Lax` su tutti i cookie relativi alla sessione  
- [ ] No — l'attributo `SameSite` è **mancante**, lasciando spazio al comportamento predefinito del browser  
- [ ] No — `SameSite` è esplicitamente impostato su `None` senza il flag `Secure` o ulteriori mitigazioni  

### La protezione CSRF può essere bypassata cambiando il metodo della richiesta HTTP?
- [ ] No — la protezione viene applicata indipendentemente dal metodo HTTP utilizzato  
- [ ] Sì — i token sono validati solo sulle richieste `POST`, ma l'azione **può** essere eseguita tramite `GET`  
- [ ] Sì — il cambio del metodo (es. in `PUT` o `DELETE`) bypassa il controllo del token

---

## WSTG-SESS-06 — Testing for Logout Functionality

La funzionalità di logout è un controllo di sicurezza critico progettato per terminare la sessione di un utente e invalidare gli identificatori di sessione associati sia sul lato client che sul lato server. La mancata implementazione corretta del logout consente attacchi di Session Fixation o Hijacking, in quanto un attaccante che ottiene l'accesso a una macchina dopo che un utente ha effettuato il "logout" potrebbe potenzialmente riutilizzare il session token ancora attivo. I pentester valutano questo aspetto verificando se il session cookie viene rimosso dal browser, se lo stato della sessione lato server viene esplicitamente distrutto e se la navigazione tramite il pulsante "Back" del browser consente l'accesso a dati sensibili memorizzati nella cache. Un'implementazione sicura del logout garantisce che, una volta attivata l'azione, nessuna richiesta successiva effettuata con il vecchio session token venga autorizzata dall'applicazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Strumenti:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### È presente e accessibile un meccanismo di logout funzionante?
- [ ] Sì — il pulsante di logout esiste e attiva una richiesta di terminazione  
- [ ] No — il pulsante di logout è **mancante** o **non** attiva un evento di terminazione  

### L'identificativo di sessione viene invalidato lato server?
- [ ] Sì — il server rifiuta tutte le richieste successive che utilizzano il vecchio session token  
- [ ] No — il server **continua ad accettare** il vecchio session token dopo il logout  

### L'applicazione rimuove i cookie di sessione nel browser al momento del logout?
- [ ] Sì — i cookie vengono sovrascritti con una data di scadenza passata o un valore vuoto  
- [ ] Sì — i cookie rimangono ma **non sono più validi** sul server  
- [ ] No — i cookie rimangono nel browser e **mantengono** i loro valori originali  

### È possibile accedere a contenuti autenticati sensibili tramite il pulsante "Back" del browser dopo il logout?
- [ ] No — gli header `Cache-Control: no-store` o simili impediscono la visualizzazione di pagine sensibili post-logout  
- [ ] Sì — le pagine sensibili sono memorizzate nella cache e **possono** essere visualizzate tramite la navigazione dopo che la sessione è stata terminata

---

## WSTG-SESS-07 — Test del Session Timeout

Il test del session timeout valuta se un'applicazione termini efficacemente la sessione di un utente dopo un periodo predefinito di inattività o una durata totale. Questo controllo è fondamentale per mitigare il rischio di Session Hijacking, in particolare su workstation condivise o in scenari in cui un attaccante cattura un session identifier tramite intercettazione di rete o XSS. I pentester analizzano sia gli idle timeout, che si attivano dopo un periodo di inattività, sia gli absolute timeout, che limitano la durata totale di una sessione indipendentemente dall'attività. Dal punto di vista di un attaccante, sessioni indefinite o eccessivamente lunghe forniscono una finestra di opportunità estesa per mantenere l'accesso non autorizzato e bypassare la necessità di ri-autenticazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### L'applicazione implementa un idle session timeout?
- [ ] Sì — la sessione scade dopo un breve periodo (es. 15-30 minuti) di inattività  
- [ ] Sì — la sessione scade ma la durata del timeout è **eccessivamente lunga** (es. > 24 ore)  
- [ ] No — la sessione **rimane attiva** a tempo indeterminato durante l'inattività  

### Il session timeout è applicato lato server (server-side)?
- [ ] Sì — il server rifiuta le richieste con token scaduti indipendentemente dallo stato lato client  
- [ ] No — il timeout è applicato **solo** tramite JavaScript lato client o meta-refresh  

### L'applicazione implementa un absolute session timeout?
- [ ] Sì — la sessione viene terminata dopo una durata massima fissa, indipendentemente dall'attività  
- [ ] No — la sessione **può** essere estesa indefinitamente finché vi è un'attività continua  

### L'identificatore di sessione viene invalidato sul server al momento del timeout?
- [ ] Sì — il session token **non può** essere riutilizzato una volta raggiunta la soglia di timeout  
- [ ] No — il session token **è ancora valido** sul server anche dopo che il client è stato reindirizzato alla pagina di login  

### La sessione può essere mantenuta attiva a tempo indeterminato utilizzando richieste heartbeat automatizzate?
- [ ] No — l'absolute timeout o controlli secondari **impediscono** l'estensione infinita della sessione  
- [ ] Sì — richieste periodiche in background (es. AJAX) **possono** mantenere la sessione attiva indefinitamente

---

## WSTG-SESS-08 — Session Puzzling

Il Session Puzzling, noto anche come Session Variable Overloading, si verifica quando un'applicazione utilizza la stessa variabile di sessione per molteplici scopi in diversi moduli o non riesce a cancellare correttamente gli stati della sessione durante le transizioni del flusso di lavoro. Gli attaccanti sfruttano questo comportamento popolando le variabili di sessione in un contesto — come l'anteprima di un profilo non autenticato o una registrazione multi-step — per poi navigare in un'area protetta dove l'applicazione si fida erroneamente di quelle variabili preesistenti. Ciò può portare a impatti critici tra cui Authentication bypass, Privilege Escalation o accesso non autorizzato a dati sensibili, ingannando l'applicazione e facendole credere che una sessione si trovi in uno stato di privilegio superiore a quello reale. La vulnerabilità è più diffusa in applicazioni complesse e stateful dove la logica di gestione della sessione è applicata in modo incoerente tra i diversi componenti funzionali.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### L'applicazione utilizza gli stessi nomi di variabile di sessione per scopi diversi in moduli separati?
- [ ] No — i nomi delle variabili sono univoci o gli scope sono isolati  
- [ ] Sì — esistono nomi di variabili condivisi ma lo stato viene cancellato tra i moduli  
- [ ] Sì — esistono nomi di variabili condivisi e lo stato **non viene** cancellato  

### Le azioni non autenticate possono influenzare le variabili di sessione utilizzate in contesti autenticati?
- [ ] No — le variabili di sessione vengono inizializzate solo **dopo** un'autenticazione riuscita  
- [ ] Sì — le variabili di sessione possono essere impostate prima del login ma **non influiscono** sullo stato post-login  
- [ ] Sì — le variabili di sessione impostate durante la pre-autenticazione **sono ritenute affidabili** dopo l'autenticazione *(Critico)*  

### L'applicazione reinizializza l'identificatore di sessione e le relative variabili associate in caso di variazione del livello di privilegio?
- [ ] Sì — la sessione viene completamente resettata e **viene emesso** un nuovo ID  
- [ ] Parziale — viene emesso un nuovo ID ma alcune variabili di sessione **persistono**  
- [ ] No — l'ID di sessione e tutte le variabili **rimangono** invariati  

### È possibile ottenere privilegi amministrativi manipolando le variabili di sessione tramite un account non privilegiato?
- [ ] No — le variabili di privilegio **non possono** essere modificate dagli utenti  
- [ ] Sì — le variabili possono essere modificate ma l'applicazione le **valida** rispetto al database di backend  
- [ ] Sì — le variabili possono essere modificate e l'applicazione **si fida** implicitamente dello stato della sessione *(Critico)*  

### Lo stato della sessione viene cancellato immediatamente al completamento di uno specifico flusso di lavoro (es. reset della password, checkout)?
- [ ] Sì — le variabili di sessione per il flusso di lavoro vengono distrutte al completamento  
- [ ] No — le variabili del flusso di lavoro **persistono** e possono essere riutilizzate in altre aree dell'applicazione

---

## WSTG-SESS-09 — Testing for Session Hijacking

Il Session Hijacking si verifica quando un utente malintenzionato cattura, predice o fissa un identificatore di sessione (SID) valido per ottenere l'accesso non autorizzato a una sessione attiva di un utente. Questa vulnerabilità si manifesta tipicamente attraverso una sicurezza insufficiente del transport layer, la mancanza di flag di sicurezza nei cookie o algoritmi di generazione del SID prevedibili che consentono a un attaccante di bypassare completamente l'autenticazione. Dal punto di vista di un attaccante, lo sfruttamento con successo di questa vulnerabilità permette l'impersonificazione completa della vittima senza richiederne le credenziali, spesso ottenuta tramite network sniffing, Cross-Site Scripting (XSS) o attacchi di session fixation.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Strumenti:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Gli identificatori di sessione sono protetti durante il transito?
- [ ] Sì — il flag `Secure` è **abilitato** e HSTS è applicato rigorosamente  
- [ ] Sì — il flag `Secure` è **abilitato** ma HSTS è **disabilitato**  
- [ ] No — il flag `Secure` **non è applicato**, consentendo l'intercettazione del SID su canali non criptati *(Alta)*  

### L'identificatore di sessione è protetto dall'accesso tramite script lato client?
- [ ] Sì — il flag `HttpOnly` è **applicato** a tutti i cookie relativi alla sessione  
- [ ] No — il flag `HttpOnly` **non è applicato**, consentendo l'esfiltrazione del SID tramite XSS *(Critica)*  

### L'applicazione implementa il session binding agli attributi del client?
- [ ] Sì — la sessione è legata agli attributi del client (IP/User-Agent) e il riutilizzo **non è possibile**  
- [ ] Sì — il session binding è presente ma il bypass **è possibile** tramite header spoofing  
- [ ] No — non esiste alcun session binding; il SID **è valido** se utilizzato da qualsiasi sorgente  

### L'identificatore di sessione è sufficientemente casuale da prevenirne la predizione?
- [ ] Sì — il SID utilizza un generatore di numeri pseudo-casuali crittograficamente sicuro (CSPRNG)  
- [ ] No — la lunghezza del SID è insufficiente o segue una sequenza/pattern **prevedibile**  

### Le sessioni simultanee sono gestite per limitare la finestra di attacco?
- [ ] No — l'applicazione impone rigorosamente una singola sessione attiva per utente  
- [ ] Sì — sessioni simultanee multiple sono **abilitate** ma monitorate  
- [ ] Sì — sessioni simultanee illimitate **sono possibili** attraverso diversi dispositivi/posizioni

---

## WSTG-SESS-10 — Testing JSON Web Tokens

I JSON Web Tokens (JWT) sono comunemente utilizzati per la gestione delle sessioni stateless e la propagazione dell'identità, ma la loro sicurezza dipende interamente dalla corretta implementazione degli algoritmi di firma e da una rigorosa verifica dei claim. Gli attaccanti tentano spesso di bypassare l'autenticazione sfruttando le falle dell'algoritmo "none", effettuando il brute-forcing di chiavi segrete HS256 deboli o eseguendo attacchi di algorithm-switching, in cui una chiave pubblica asimmetrica viene utilizzata come segreto HMAC simmetrico. Queste vulnerabilità si manifestano tipicamente nei cookie di sessione o negli header di Authorization, permettendo potenzialmente a un attaccante di elevare i propri privilegi o impersonare qualsiasi utente modificando claim come `role` o `user_id` senza invalidare l'integrità crittografica del token.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Strumenti:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### L'algoritmo "none" è accettato dal server?
- [ ] No — il server **rifiuta** i token che utilizzano `alg: none` nell'header  
- [ ] Sì — il server **accetta** i token con `alg: none` e una firma vuota *(Critico)*  

### Come viene verificata la firma JWT e protetta contro il brute-force?
- [ ] Sì — vengono utilizzate chiavi asimmetriche forti (RS256/ES256) o segreti HMAC ad alta entropia  
- [ ] Sì — viene utilizzato HS256 ma il segreto **può** essere forzato tramite brute-force a causa della bassa entropia o di valori predefiniti  
- [ ] No — la verifica della firma **non è applicata** o **può** essere aggirata tramite algorithm switching (da RS256 a HS256)  

### I claim standard (exp, nbf, iat) sono applicati rigorosamente dal backend?
- [ ] Sì — `exp` (scadenza) è presente e **non può** essere aggirato  
- [ ] Sì — `exp` è presente ma il server **non lo applica** durante la validazione  
- [ ] No — i claim di scadenza sono **mancanti** o ignorati, consentendo il replay indefinito della sessione  

### L'implementazione previene la key injection basata su header (kid, jku, x5u)?
- [ ] Sì — gli header sono sanitizzati e sono **consentite** solo chiavi/fonti interne affidabili  
- [ ] No — l'header `kid` (Key ID) è vulnerabile a directory traversal o SQL injection per puntare a un file noto  
- [ ] No — gli header `jku` o `x5u` **possono** essere puntati verso URL controllati dall'attaccante per caricare chiavi malevole

---

## WSTG-SESS-11 — Testing for Concurrent Sessions

Il test per le sessioni concorrenti valuta se un'applicazione permette a un singolo account utente di mantenere più sessioni attive simultanee attraverso diversi browser, dispositivi o indirizzi IP. La mancata limitazione delle sessioni concorrenti aumenta la finestra di opportunità per gli attaccanti di utilizzare session token rubati o credenziali compromesse senza allertare l'utente legittimo o terminare le sessioni esistenti. Questo comportamento viene tipicamente valutato autenticandosi da più sorgenti e monitorando la stabilità della sessione, rivelando spesso debolezze nella logica di gestione delle sessioni o una mancanza di regole di business focalizzate sulla sicurezza. Dal punto di vista di un attaccante, l'assenza di controllo sulla concorrenza permette una persistenza furtiva dopo una compromissione iniziale e facilita attacchi automatizzati paralleli.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Le sessioni concorrenti sono limitate dalla policy dell'applicazione?
- [ ] Sì — è permessa solo una sessione per utente; i nuovi login invalidano quelli vecchi *(Più sicuro)*  
- [ ] Sì — viene imposto un numero fisso di sessioni concorrenti che non può essere superato  
- [ ] No — è possibile un numero illimitato di sessioni concorrenti  

### Come risponde l'applicazione quando viene raggiunto il limite di sessioni?
- [ ] La sessione attiva più vecchia **viene terminata automaticamente** (mitigazione per session fixation/hijacking)  
- [ ] L'utente **viene invitato** a terminare le sessioni esistenti prima che la nuova sessione venga autorizzata  
- [ ] Il nuovo tentativo di login **viene bloccato** finché non viene effettuato un logout manuale da un altro dispositivo  
- [ ] Nessuna azione intrapresa — più sessioni rimangono **abilitate** simultaneamente  

### L'applicazione fornisce un'interfaccia di gestione delle sessioni per l'utente?
- [ ] Sì — gli utenti **possono** visualizzare le sessioni attive (IP, dispositivo, ora) e terminarle da remoto  
- [ ] Sì — gli utenti **possono** visualizzare le sessioni attive ma **non possono** terminarle da remoto  
- [ ] No — gli utenti **non possono** vedere o gestire altre sessioni attive associate al proprio account  

### L'utente viene notificato quando viene rilevata un'attività di login concorrente?
- [ ] Sì — viene generato un avviso (email, SMS o in-app) per i login da nuovi dispositivi o posizioni  
- [ ] No — non viene fornita alcuna notifica quando vengono stabilite sessioni concorrenti

---

## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Il Reflected Cross Site Scripting (XSS) si verifica quando un'applicazione include dati non attendibili in una risposta HTTP senza una sufficiente validazione o codifica (encoding), causando l'esecuzione del payload nel contesto del browser della vittima. Gli attaccanti inviano payload malevoli tramite tecniche di social engineering, tipicamente attraverso URL appositamente creati o form, per dirottare (hijack) le sessioni utente, esfiltrare cookie sensibili o eseguire azioni non autorizzate per conto dell'utente. Questa vulnerabilità si trova comunemente nei parametri di ricerca, nei messaggi di errore e in qualsiasi endpoint che rifletta l'input direttamente nell'interfaccia utente. L'attacco si basa sull'interazione della vittima con un link malevolo, rendendolo un vettore di attacco non persistente ma estremamente efficace per colpire specifici utenti amministrativi o autenticati.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Gli input forniti dall'utente sono riflessi nel corpo della risposta?
- [ ] No — l'input non viene **mai** riflesso verso l'utente  
- [ ] Sì — l'input viene riflesso ma è correttamente codificato e il **bypass non è possibile**  
- [ ] Sì — l'input viene riflesso **senza** alcuna codifica o sanitizzazione  

### È implementata una codifica dell'output sensibile al contesto (context-aware)?
- [ ] Sì — viene **applicata** una codifica corretta (HTML, Attributo, JavaScript) in base allo specifico contesto di riflessione  
- [ ] Sì — la codifica viene **applicata** ma è insufficiente per lo specifico contesto (es. codifica HTML all'interno di un tag `<script>`)  
- [ ] No — la codifica **not viene applicata**  

### È possibile bypassare la validazione dell'input o i filtri del Web Application Firewall (WAF)?
- [ ] No — i filtri bloccano efficacemente tutti i payload XSS comuni e offuscati  
- [ ] Sì — i filtri sono presenti ma il **bypass è possibile** utilizzando la codifica dei caratteri, tag non standard o poliglotti (polyglots)  
- [ ] No — non sono presenti filtri o meccanismi di validazione  

### Qual è il contesto di esecuzione dell'input riflesso?
- [ ] Sicuro — l'input è riflesso in una posizione non eseguibile (es. all'interno di tag standard `<div>` o `<span>`)  
- [ ] Rischioso — l'input è riflesso all'interno di attributi HTML o nei campi `src`/`href` dei tag  
- [ ] Critico — l'input è riflesso direttamente all'interno di blocchi `<script>`, gestori di eventi (event handlers) o template literals  

### Sono presenti moderni header di sicurezza del browser per mitigare l'impatto XSS?
- [ ] Sì — `Content-Security-Policy` (CSP) è **abilitato** con un script-src restrittivo  
- [ ] Sì — `Content-Security-Policy` (CSP) è **abilitato** ma contiene `unsafe-inline` o whitelist deboli  
- [ ] No — non sono presenti CSP o header legacy `X-XSS-Protection`

---

## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Lo Stored Cross Site Scripting (XSS), noto anche come XSS persistente, si verifica quando un'applicazione riceve dati da un utente e li memorizza in un database persistente, in un file system o in un altro supporto di memorizzazione senza un'adeguata validazione o codifica. Quando una vittima naviga successivamente in una pagina che recupera e visualizza questi dati non sanificati, lo script malevolo viene eseguito nel contesto del suo browser sotto l'origine dell'applicazione. Questa vulnerabilità è particolarmente pericolosa perché non richiede che la vittima clicchi su un link appositamente creato; qualsiasi utente che visualizzi la pagina interessata diventa un bersaglio, portando potenzialmente a session hijacking, azioni non autorizzate o credential harvesting. Gli attaccanti solitamente prendono di mira sezioni commenti, profili utente, bacheche e log amministrativi in cui l'input viene costantemente renderizzato per altri utenti o amministratori.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Strumenti:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Esistono punti di ingresso che memorizzano l'input dell'utente per una successiva visualizzazione ad altri utenti?
- [ ] No — l'applicazione non memorizza input controllabile dall'utente per un rendering successivo  
- [ ] Sì — l'input viene memorizzato (es. profili, commenti) ma **non** viene renderizzato in un contesto HTML  
- [ ] Sì — l'input viene memorizzato e renderizzato ad altri utenti o amministratori  

### Viene applicata la codifica dell'output o la sanificazione quando i dati memorizzati vengono renderizzati?
- [ ] Sì — la codifica dell'output context-aware **è applicata** e il bypass **non è possibile** *(Più sicuro)*  
- [ ] Sì — la sanificazione (es. `DOMPurify`) **è applicata** ma il bypass **è possibile** tramite errori di configurazione  
- [ ] No — i dati vengono renderizzati direttamente nel DOM **senza** codifica o sanificazione *(Alta)*  

### È possibile bypassare la validazione dell'input o la sanificazione utilizzando payload alternativi?
- [ ] No — i filtri gestiscono in modo sicuro varie codifiche, tag non standard e gestori di eventi (event handlers)  
- [ ] Sì — il bypass **è possibile** utilizzando la codifica HTML entity o tag specifici (es. `<svg>`, `<img>`)  
- [ ] Sì — il bypass **è possibile** tramite payload poliglotti o mutation-based XSS (mXSS)  

### Una Content Security Policy (CSP) fornisce un secondo livello di difesa?
- [ ] Sì — una CSP rigorosa **è abilitata** e impedisce l'esecuzione di script inline e da sorgenti non autorizzate  
- [ ] Sì — una CSP **è abilitata** ma è configurata in modo errato (es. `unsafe-inline` o `script-src` troppo ampio)  
- [ ] No — nessuna CSP **è abilitata** per l'applicazione  

### È possibile ottenere un "Stored XSS to RCE" o altre catene ad alto impatto (Blind XSS)?
- [ ] No — l'impatto è limitato al contesto del browser lato client  
- [ ] Sì — lo stored XSS **può** essere utilizzato per colpire gli amministratori (Blind XSS) nei pannelli interni  
- [ ] Sì — lo stored XSS **può** essere concatenato con altre vulnerabilità (es. CSRF, esfiltrazione di dati sensibili)

---

## WSTG-INPV-03 — Testing for HTTP Verb Tampering

L'HTTP Verb Tampering sfrutta le vulnerabilità nel modo in cui i server web e i framework delle applicazioni autorizzano l'accesso a risorse specifiche basandosi sul metodo HTTP utilizzato. Gli attaccanti tentano di bypassare i vincoli di sicurezza sostituendo i metodi standard come `GET` o `POST` con alternative quali `HEAD`, `PUT`, `OPTIONS` o persino stringhe arbitrarie e non standard che il backend potrebbe elaborare in modo incoerente. Questa vulnerabilità si verifica tipicamente quando le configurazioni di sicurezza — come i filtri web.xml di Java EE o le regole di autorizzazione .NET — elencano esplicitamente i metodi consentiti ma non tengono conto degli altri, oppure quando viene utilizzato un approccio "deny-by-method" invece di "deny-all". Un exploit riuscito può comportare l'accesso non autorizzato a funzioni amministrative, la modifica dei dati o la divulgazione di informazioni relative alla configurazione interna del server.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se è possibile eseguire funzioni amministrative o modifiche dei dati (PUT/DELETE) senza autenticazione.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### L'applicazione risponde a metodi HTTP non standard o arbitrari?
- [ ] No — l'applicazione rifiuta tutti i metodi tranne quelli esplicitamente richiesti  
- [ ] Sì — l'applicazione risponde ai metodi standard (OPTIONS, TRACE) ma **non è possibile** bypassare la sicurezza  
- [ ] Sì — l'applicazione accetta stringhe arbitrarie (es. FOO, TEST) e le elabora come richieste `GET` o `POST`  

### È possibile bypassare l'autenticazione/autorizzazione cambiando il metodo HTTP?
- [ ] No — i controlli di sicurezza sono applicati globalmente indipendentemente dal verbo HTTP  
- [ ] Sì — i controlli sono presenti ma il bypass **è possibile** utilizzando il metodo `HEAD`  
- [ ] Sì — i controlli di sicurezza **non sono applicati** ai metodi alternativi, consentendo l'accesso non autorizzato a endpoint riservati  

### I metodi pericolosi come `PUT` o `DELETE` sono abilitati sul server?
- [ ] No — i metodi pericolosi sono **disabilitati** o restituiscono un errore 405 Method Not Allowed  
- [ ] Sì — i metodi sono abilitati ma richiedono un'autenticazione valida con privilegi elevati  
- [ ] Sì — i metodi sono **abilitati** e consentono il caricamento non autorizzato di file o l'eliminazione di risorse *(Critico)*  

### Il metodo `OPTIONS` rivela informazioni sensibili sui verbi consentiti?
- [ ] No — `OPTIONS` è **disabilitato** o restituisce una risposta generica  
- [ ] Sì — `OPTIONS` restituisce l'header `Allow` ma non espone metodi interni riservati  
- [ ] Sì — `OPTIONS` rivela metodi interni o amministrativi che facilitano ulteriori exploit  

### Il metodo `TRACE` è abilitato, consentendo potenzialmente il Cross-Site Tracing (XST)?
- [ ] No — i metodi `TRACE` e `TRACK` sono **disabilitati**  
- [ ] Sì — `TRACE` è **abilitato** e riflette gli header HTTP, inclusi cookie o token sensibili

---

## WSTG-INPV-04 — Testing for HTTP Parameter Pollution

L'HTTP Parameter Pollution (HPP) si verifica quando un'applicazione riceve più parametri HTTP con lo stesso nome e li elabora in modo incoerente o insicuro. Fornendo parametri duplicati, un utente malintenzionato può manipolare la logica interna dell'applicazione, bypassando potenzialmente i Web Application Firewall (WAF), i filtri di validazione dell'input o i meccanismi di controllo degli accessi. Questa vulnerabilità dipende fortemente dal web server sottostante o dal framework dell'applicazione, che può scegliere la prima, l'ultima o una versione concatenata dei parametri ripetuti. Lo sfruttamento (Exploit) tipicamente prende di mira endpoint che trasmettono parametri a API di back-end, query al database o meccanismi di reindirizzamento URL, portando a impatti che vanno dal Cross-Site Scripting (XSS) all'esfiltrazione non autorizzata di dati.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### In che modo l'applicazione gestisce parametri HTTP multipli con lo stesso nome?
- [ ] No — i parametri duplicati vengono **rifiutati** o ignorati dal server  
- [ ] Sì — il server seleziona in modo coerente solo la **prima** o l'**ultima** occorrenza  
- [ ] Sì — il server **concatena** i valori, consentendo potenzialmente un'injection  

### L'HPP può essere sfruttato per bypassare controlli di sicurezza come un WAF o un filtro di input?
- [ ] No — i controlli di sicurezza ispezionano correttamente **tutte** le occorrenze di un parametro  
- [ ] Sì — un WAF o un filtro ispeziona solo la **prima** occorrenza, permettendo a una **seconda** occorrenza malevola di passare  
- [ ] Sì — la concatenazione consente a un utente malintenzionato di **suddividere** un payload su più parametri per eludere il rilevamento  

### L'applicazione riflette i parametri inquinati nella risposta senza una corretta sanificazione?
- [ ] No — i parametri vengono sanificati o codificati prima di essere riflessi nella UI  
- [ ] Sì — l'inquinamento produce contenuti riflessi, ma la sanificazione **viene applicata**  
- [ ] Sì — l'inquinamento produce contenuti riflessi e la sanificazione **non viene applicata**, portando a XSS o errori di logica  

### I parametri passati a chiamate API interne o sistemi di back-end sono vulnerabili all'inquinamento?
- [ ] No — le richieste di back-end utilizzano metodi di costruzione sicuri e non dinamici  
- [ ] Sì — l'HPP consente a un utente malintenzionato di **iniettare** o **sovrascrivere** parametri nella struttura della richiesta di back-end  

### L'applicazione mostra un comportamento diverso quando i parametri sono inquinati in richieste GET rispetto a richieste POST?
- [ ] No — il comportamento è coerente in tutti i metodi HTTP  
- [ ] Sì — solo i parametri GET sono vulnerabili all'inquinamento  
- [ ] Sì — solo i parametri POST (body) sono vulnerabili all'inquinamento

---

## WSTG-INPV-05 — SQL Injection

La SQL Injection si verifica quando l'input fornito dall'utente viene incorporato nelle query SQL senza una corretta parametrizzazione o sanitizzazione, consentendo agli aggressori di manipolare la logica della query. L'exploitation andata a buon fine può portare all'esfiltrazione non autorizzata di dati, al bypass dell'autenticazione, alla modifica dei dati e, in alcuni casi, alla Remote Code Execution tramite funzionalità del database come `xp_cmdshell` o `LOAD_FILE()`. Questa vulnerabilità interessa comunemente i moduli di login, le funzionalità di ricerca, i parametri delle REST API, gli header HTTP e qualsiasi endpoint che costruisce query SQL dinamiche a partire da input controllati dall'utente. Dal punto di vista di un attaccante, l'identificazione di un punto di iniezione rappresenta il gate primario per la compromissione completa del database e il potenziale movimento laterale all'interno dell'infrastruttura.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Strumenti:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### I parametri controllabili dall'utente sono elaborati utilizzando metodi di accesso al database sicuri?
- [ ] Sì — l'applicazione utilizza esclusivamente ORM o query parametrizzate e il bypass **non è possibile**  
- [ ] Sì — vengono utilizzate query parametrizzate ma il bypass **è possibile** tramite casi limite (es. clausole `ORDER BY`)  
- [ ] No — viene utilizzata la concatenazione di stringhe grezze per costruire query SQL **senza** parametrizzazione  

### Il meccanismo di autenticazione può essere aggirato tramite SQL injection?
- [ ] No — i parametri di login **non** sono vulnerabili a injection  
- [ ] Sì — il bypass dell'autenticazione **è possibile** utilizzando payload basati su tautologie (es. `' OR 1=1 --`)  

### È possibile l'esfiltrazione di dati tramite tecniche in-band o out-of-band?
- [ ] No — nessuna esfiltrazione di dati identificata  
- [ ] Sì — l'esfiltrazione UNION-based o error-based **è possibile**  
- [ ] Sì — l'esfiltrazione blind (Boolean o Time-based) **è possibile**  
- [ ] Sì — l'esfiltrazione out-of-band (DNS/HTTP) **è possibile**  

### Le funzioni dell'applicazione presentano vulnerabilità di SQL injection di secondo ordine?
- [ ] No — i dati memorizzati sono gestiti in modo sicuro quando vengono recuperati per query successive  
- [ ] Sì — l'input dell'utente viene memorizzato e successivamente utilizzato in una query SQL non sicura **senza** validazione  

### I privilegi dell'account di servizio del database sono limitati al minimo necessario?
- [ ] Sì — l'account di servizio ha il **minimo privilegio** (es. limitato a tabelle/azioni specifiche)  
- [ ] No — l'account di servizio ha privilegi elevati (es. `DROP TABLE`, privilegi `FILE`, o `xp_cmdshell` **abilitato**)

---

## WSTG-INPV-06 — Test per LDAP Injection

L'LDAP Injection si verifica quando un'applicazione incorpora dati forniti dall'utente nei filtri LDAP (Lightweight Directory Access Protocol) senza una corretta sanitizzazione o l'uso di escaping, consentendo a un utente malintenzionato di manipolare la logica della query. Iniettando caratteri speciali come asterischi, parentesi e operatori logici, gli attaccanti possono modificare il filtro di ricerca per bypassare i meccanismi di autenticazione o esfiltrare informazioni sensibili dalla directory, inclusi nomi utente, appartenenze a gruppi e attributi organizzativi. Questa vulnerabilità si manifesta tipicamente in funzionalità come le ricerche nelle directory aziendali, i portali per i dipendenti o i sistemi di Single Sign-On (SSO) che si interfacciano con Active Directory o OpenLDAP. Dal punto di vista di un attaccante, lo sfruttamento riuscito spesso comporta tecniche blind per enumerare la struttura della directory o i valori degli attributi bit per bit quando l'output diretto della query viene soppresso dall'applicazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Strumenti:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### L'applicazione utilizza LDAP per l'autenticazione o per le ricerche nella directory?
- [ ] No — LDAP **non è utilizzato** nell'architettura dell'applicazione  
- [ ] Sì — LDAP è utilizzato e tutti gli input sono rigorosamente sanitizzati o parametrizzati  
- [ ] Sì — LDAP è utilizzato e l'input dell'utente viene concatenato nei filtri **senza** un corretto escaping  

### I metacaratteri LDAP sono correttamente sottoposti a escaping o filtrati nell'input dell'utente?
- [ ] Sì — i caratteri come `(`, `)`, `&`, `|`, `*` e `\` **non sono consentiti** o sono correttamente sottoposti a escaping  
- [ ] No — i metacaratteri **possono** essere iniettati per alterare la struttura del filtro LDAP  

### Il meccanismo di autenticazione può essere bypassato tramite injection?
- [ ] No — la logica di login **non è suscettibile** alla manipolazione della query  
- [ ] Sì — l'autenticazione **può** essere bypassata utilizzando l'injection di OR logici (`|`) o wildcard (`*`) nei campi username o password  

### È possibile l'esfiltrazione cieca (blind) dei dati dal servizio di directory?
- [ ] No — i risultati della ricerca sono limitati e non si osservano differenze temporali (timing) o booleane  
- [ ] Sì — gli attributi della directory **possono** essere enumerati bit per bit tramite tecniche di blind injection basate su booleani  
- [ ] Sì — i record completi della directory **sono** riflessi nella risposta dell'applicazione a causa della manipolazione della query  

### I componenti secondari dell'applicazione (es. mailer, SSO) sono vulnerabili all'injection di attributi basata su LDAP?
- [ ] No — gli attributi LDAP sono gestiti in modo sicuro in tutti i componenti  
- [ ] Sì — un attaccante **può** modificare i propri attributi (es. email, appartenenza ai gruppi) tramite injection per ottenere un'escalation dei privilegi o reindirizzare le comunicazioni

---

## WSTG-INPV-07 — XML Injection

L'XML Injection si verifica quando un'applicazione incorpora dati forniti dall'utente in un documento o stream XML senza un'adeguata validazione, sanificazione o escaping dei metacaratteri XML. Iniettando specifici tag XML o elementi strutturali, un attaccante può manipolare la logica del documento, bypassare i meccanismi di autenticazione o interferire con l'integrità dei dati elaborati dal backend. Questa vulnerabilità si riscontra comunemente nei web service basati su SOAP, nelle API REST che accettano XML, nelle asserzioni SAML e nelle applicazioni che generano dinamicamente file di configurazione o report XML. Dal punto di vista di un attaccante, l'obiettivo è uscire dal contesto dei dati previsto per alterare la struttura del documento, portando potenzialmente a un'escalation dei privilegi non autorizzata o all'esecuzione di una logica di business non prevista.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Strumenti:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### L'applicazione accetta ed elabora input in formato XML dall'utente?
- [ ] No — l'applicazione **non** elabora input XML  
- [ ] Sì — l'input XML viene elaborato tramite API (SOAP/REST)  
- [ ] Sì — l'XML viene elaborato tramite caricamento di file (SVG, DOCX, ecc.)  

### I metacaratteri XML (es. `<`, `>`, `&`, `'`, `"`) sono correttamente sottoposti a escaping o sanificati?
- [ ] Sì — tutti i metacaratteri sono **correttamente** sottoposti a escaping o sanificati *(Più sicuro)*  
- [ ] Sì — viene applicato del filtraggio ma il bypass **è possibile** utilizzando la codifica  
- [ ] No — l'input grezzo viene inserito direttamente nelle strutture XML **senza** sanificazione  

### La validazione dello schema lato server (XSD/DTD) è applicata per tutti gli input XML?
- [ ] Sì — la validazione rigorosa dello schema è **abilitata** e impedisce modifiche strutturali  
- [ ] Sì — la validazione è **abilitata** ma **non** impedisce l'iniezione di tag all'interno dei campi dati  
- [ ] No — la validazione dello schema è **disabilitata** o non implementata  

### Un attaccante può iniettare nuovi tag XML per alterare la struttura o la logica del documento?
- [ ] No — la struttura rimane intatta indipendentemente dall'input  
- [ ] Sì — i tag possono essere iniettati ma **non possono** alterare la logica di business  
- [ ] Sì — l'iniezione di tag **è possibile** e consente il bypass della logica o la modifica dei dati *(Critico)*  

### Il parser XML può essere manipolato utilizzando sezioni CDATA o commenti XML?
- [ ] No — il parser gestisce correttamente o rifiuta i marcatori CDATA/commenti  
- [ ] Sì — l'iniezione di `<![CDATA[...]]>` o `<!-- ... -->` **è possibile** per nascondere o bypassare i filtri

---

## WSTG-INPV-08 — Testing for SSI Injection

La Server-Side Includes (SSI) Injection si verifica quando un'applicazione non riesce a sanitizzare l'input dell'utente prima di incorporarlo nei file HTML elaborati dal motore SSI del server. Gli attaccanti sfruttano questa vulnerabilità iniettando direttive SSI, come `<!--#exec cmd="id" -->`, per eseguire comandi shell arbitrari, leggere file locali o esfiltrare variabili d'ambiente. Questa vulnerabilità si trova tipicamente nelle applicazioni che servono estensioni di file legacy come `.shtml`, `.shtm` o `.stm`, o dove il web server è configurato esplicitamente per analizzare le SSI all'interno di file HTML standard. Un Exploit riuscito garantisce all'attaccante i medesimi permessi del processo del web server, portando spesso a una compromissione completa del sistema o all'esposizione di dati sensibili.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Il web server supporta ed elabora le direttive SSI?
- [ ] No — il server **non** supporta SSI o le estensioni di file come `.shtml` sono **disabilitate**  
- [ ] Sì — le SSI **sono abilitate** ma l'input dell'utente **non** viene riflesso nelle pagine elaborate  
- [ ] Sì — le SSI **sono abilitate** e l'input dell'utente **viene** riflesso nelle pagine elaborate  

### È possibile eseguire comandi di sistema arbitrari tramite la direttiva `#exec`?
- [ ] No — la direttiva `#exec` è **disabilitata** o limitata dalla configurazione del server  
- [ ] Sì — l'esecuzione di comandi **è possibile** tramite payload `#exec cmd` o `#exec cgi`  

### È possibile l'esfiltrazione di file sensibili o variabili d'ambiente?
- [ ] No — le direttive come `#include` o `#config` sono **disabilitate**  
- [ ] Sì — la lettura di file locali (es. `/etc/passwd`) **è possibile** tramite `#include virtual`  
- [ ] Sì — le variabili d'ambiente del server **possono** essere esfiltrate tramite `#printenv` o `#echo`  

### La validazione dell'input o i controlli WAF sono efficaci contro i payload SSI?
- [ ] Sì — una validazione dell'input rigorosa **è applicata** e il bypass **non è possibile**  
- [ ] Sì — la validazione/WAF **è applicata** ma il bypass **è possibile** utilizzando la codifica dei caratteri o una differente sintassi SSI  
- [ ] No — non è presente alcuna validazione o protezione WAF per i caratteri SSI (`<`, `!`, `#`, `-`, `"`)

---

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

## WSTG-INPV-10 — IMAP SMTP Injection

Le vulnerabilità di IMAP e SMTP injection si verificano quando un'applicazione web filtra in modo improprio i dati forniti dall'utente prima di incorporarli nei comandi inviati a un server di posta. Iniettando i caratteri Carriage Return e Line Feed (CRLF), un attaccante può terminare il comando previsto e aggiungere istruzioni e-mail arbitrarie, come l'aggiunta di destinatari supplementari (CC/BCC) o la modifica del corpo del messaggio. Questi difetti si riscontrano più frequentemente nei gateway web-to-mail, nei moduli di contatto e nei client di posta elettronica che comunicano direttamente con i server di posta tramite protocolli come IMAP4 o SMTP. Lo sfruttamento con successo di queste vulnerabilità consente il relay non autorizzato di spam, l'esfiltrazione di contenuti e-mail sensibili e, in alcuni casi, la totale compromissione dell'integrità del server di posta.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### L'applicazione elabora l'input fornito dall'utente per costruire comandi IMAP o SMTP?
- [ ] No — l'applicazione **non** interagisce con i server di posta tramite input dell'utente  
- [ ] Sì — l'applicazione interagisce con i server di posta ma utilizza una API o una libreria sicura  
- [ ] Sì — l'applicazione costruisce comandi di posta grezzi utilizzando parametri controllati dall'utente  

### L'input viene validato per impedire l'iniezione di sequenze Carriage Return (`\r`) e Line Feed (`\n`)?
- [ ] Sì — i caratteri CRLF sono **rigorosamente** filtrati, codificati o rifiutati  
- [ ] Sì — l'input è validato rispetto a una allowlist rigorosa di caratteri  
- [ ] No — i caratteri CRLF **non** vengono filtrati, consentendo la terminazione e l'iniezione dei comandi  

### Un attaccante può iniettare header di posta aggiuntivi come `Bcc:` o `Cc:`?
- [ ] No — la modifica degli header **non è possibile** a causa di rigorosi vincoli sull'input  
- [ ] Sì — l'iniezione di header aggiuntivi **è possibile** tramite sequenze CRLF  
- [ ] Sì — il relay di posta verso domini esterni **è abilitato** ed esploitabile *(Critico)*  

### L'applicazione consente la manipolazione dei comandi IMAP per accedere a caselle di posta non autorizzate?
- [ ] No — l'applicazione **non** utilizza IMAP o limita rigorosamente l'accesso alla casella di posta all'utente autenticato  
- [ ] Sì — l'iniezione di comandi IMAP **è possibile** ma limitata all'enumerazione dei metadati  
- [ ] Sì — l'iniezione di comandi IMAP consente la lettura o l'eliminazione delle e-mail di altri utenti  

### Il server di posta backend è vulnerabile al command pipelining?
- [ ] No — il server di posta rifiuta comandi raggruppati o comandi multipli in una singola sessione  
- [ ] Sì — il server di posta elabora comandi multipli iniettati tramite un singolo campo di input

---

## WSTG-INPV-11 — Code Injection

La Code Injection si verifica quando un'applicazione valuta frammenti di codice forniti dall'utente all'interno del proprio ambiente di runtime, tipicamente attraverso funzioni specifiche del linguaggio non sicure come `eval()`, `exec()` o `system()`. Questa vulnerabilità differisce dalla Command Injection poiché colpisce il contesto di esecuzione del linguaggio di programmazione (es. PHP, Python, Node.js) anziché la shell del sistema operativo sottostante. Gli aggressori sfruttano questi punti per eseguire logica arbitraria, esfiltrare variabili d'ambiente o ottenere una Remote Code Execution (RCE) completa sul server. I pentester dovrebbero analizzare le funzionalità che elaborano espressioni matematiche, template lato server o parametri di configurazione dinamici dove l'input potrebbe essere interpretato come codice eseguibile.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Le funzionalità dell'applicazione valutano l'input dinamico tramite funzioni di valutazione specifiche del linguaggio?
- [ ] No — le funzioni di valutazione dinamica **non sono** presenti nel codice sorgente  
- [ ] Sì — vengono utilizzate funzioni come `eval()`, `exec()` o `include()` ma **non** accettano input dall'utente  
- [ ] Sì — le funzioni vengono utilizzate e processano input fornito dall'utente  

### Vengono applicati meccanismi di validazione o sanificazione dell'input prima della valutazione?
- [ ] Sì — l'input è rigorosamente validato rispetto a una **whitelist predefinita**  
- [ ] Sì — l'input è sanificato tramite blacklisting, ma il bypass **è possibile**  
- [ ] No — l'input viene passato direttamente alla funzione di valutazione e **non sono applicati controlli**  

### L'ambiente di esecuzione è protetto da sandbox o limitato per impedire l'accesso a livello di sistema?
- [ ] Sì — l'esecuzione avviene in una sandbox altamente restrittiva dove le chiamate di sistema **non possono** essere effettuate  
- [ ] Sì — esistono alcune restrizioni, ma l'evasione dalla sandbox (sandbox escape) **è possibile**  
- [ ] No — l'ambiente consente il pieno accesso alle API del linguaggio sottostante e alle funzioni del sistema operativo *(Critica)*  

### La Code Injection può essere utilizzata per esfiltrare informazioni sensibili?
- [ ] No — l'esecuzione è "blind" e **non è possibile** alcuna comunicazione out-of-band  
- [ ] Sì — le variabili d'ambiente o i file locali **possono** essere esfiltrati tramite richieste HTTP/DNS  
- [ ] Sì — i risultati del codice eseguito sono **riflessi direttamente** nella risposta dell'applicazione  

### L'applicazione consente l'inclusione di file remoti o il caricamento dinamico di librerie?
- [ ] No — possono essere eseguiti solo percorsi di codice locali e predefiniti  
- [ ] Sì — l'applicazione **può** essere forzata a caricare ed eseguire codice da una sorgente remota o controllata dall'aggressore

---

## WSTG-INPV-12 — Command Injection

La Command Injection si verifica quando un'applicazione passa dati non sicuri forniti dall'utente, come input di moduli, header HTTP o cookie, a una shell di sistema. Questa vulnerabilità consente a un malintenzionato di eseguire comandi arbitrari del sistema operativo sul server, tipicamente con i privilegi del processo dell'applicazione web. Dal punto di vista di un attaccante, questa viene sfruttata utilizzando metacaratteri della shell come punti e virgola, pipe o backtick per concatenare comandi malevoli al flusso di esecuzione previsto. L'impatto è frequentemente catastofico, portando alla compromissione totale del sistema, all'esfiltrazione non autorizzata di dati o al movimento laterale all'interno della rete interna.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Strumenti:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### L'applicazione richiama comandi a livello di sistema o eseguibili della shell?
- [ ] No — l'applicazione **non** interagisce con la shell del sistema operativo  
- [ ] Sì — l'applicazione utilizza API interne o librerie di alto livello per i compiti di sistema  
- [ ] Sì — l'applicazione richiama direttamente comandi della shell tramite chiamate di sistema come `system()`, `exec()` o `popen()`  

### L'input dell'utente viene validato o sanificato prima di essere passato alle chiamate di sistema?
- [ ] Sì — viene applicata una validazione dell'input e una allow-list rigorosa  
- [ ] Sì — viene utilizzata la sanificazione o l'escaping, ma il bypass **è possibile**  
- [ ] No — l'input dell'utente viene passato direttamente alla shell **senza** validazione  

### I metacaratteri della shell possono essere utilizzati per manipolare il flusso di esecuzione dei comandi?
- [ ] No — i metacaratteri sono correttamente filtrati o sottoposti a escaping  
- [ ] Sì — l'esecuzione in-band, dove l'output del comando viene riflesso nella risposta, **è possibile**  
- [ ] Sì — l'esecuzione blind, dove nessun output viene riflesso, **è possibile**  

### L'esfiltrazione out-of-band (OOB) è possibile dall'host?
- [ ] No — il server non ha traffico in uscita o i segnali OOB (DNS/HTTP) falliscono  
- [ ] Sì — l'esfiltrazione di dati tramite segnali OOB basati su DNS o HTTP **è possibile**  

### Il processo dell'applicazione viene eseguito con privilegi di sistema elevati?
- [ ] No — l'applicazione viene eseguita con un account di servizio dedicato a bassi privilegi  
- [ ] Sì — l'applicazione viene eseguita come `root`, `Administrator` o un utente con privilegi elevati *(Critico)*

---

## WSTG-INPV-13 — Format String Injection

L'injection di stringhe di formato (Format String Injection) si verifica quando un'applicazione web passa l'input controllato dall'utente direttamente nell'argomento della stringa di formato di una funzione variadica, come la famiglia `printf` del C o wrapper di logging a basso livello. Sebbene sia meno comune nei moderni framework web a codice gestito (managed code), questa vulnerabilità rimane critica nelle applicazioni CGI legacy, nelle estensioni native o nei sistemi di logging in casi limite che interagiscono con librerie di sistema a basso livello. Gli attaccanti sfruttano questo comportamento fornendo specificatori di formato come `%x` o `%p` per esfiltrare indirizzi di memoria sensibili e dati dallo stack, oppure lo specificatore `%n` per eseguire scritture in memoria arbitrarie. Uno sfruttamento (Exploit) riuscito tipicamente si traduce nella completa compromissione del sistema tramite Remote Code Execution (RCE) o, come minimo, in una condizione di Denial-of-Service (DoS) attraverso il crash del processo.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### L'applicazione elabora l'input dell'utente attraverso codice nativo o interfacce CGI legacy?
- [ ] No — l'applicazione è costruita interamente su framework a codice gestito (es. JVM, CLR)  
- [ ] Sì — l'applicazione utilizza JNI, estensioni native C/C++ o CGI binari legacy dove le vulnerabilità di Format String Injection **possono** esistere  

### Gli input forniti dall'utente sono passati come argomento principale a funzioni sensibili al formato?
- [ ] No — gli input sono passati come argomenti di dati e le stringhe di formato sono **statiche** e **hardcoded**  
- [ ] Sì — l'input dell'utente è direttamente concatenato nell'argomento della stringa di formato, ma **viene applicata** una sanificazione  
- [ ] Sì — l'input dell'utente è usato direttamente come stringa di formato e **non viene applicata** alcuna sanificazione  

### È possibile esfiltrare contenuti della memoria utilizzando gli specificatori di formato?
- [ ] No — l'input è validato e specificatori come `%p`, `%x` o `%s` **non vengono elaborati**  
- [ ] Sì — l'inserimento di specificatori `%p` o `%x` comporta la visualizzazione di indirizzi di memoria dello stack o dello heap  
- [ ] Sì — dati sensibili (es. puntatori, stack cookies) **possono** essere esfiltrati tramite specificatori di formato  

### È possibile causare il crash dell'applicazione o manipolarla utilizzando lo specificatore `%n`?
- [ ] No — gli specificatori che permettono la scrittura in memoria sono **disabilitati** o bloccati dal compilatore/ambiente  
- [ ] Sì — l'applicazione va in crash (DoS) quando vengono fornite stringhe come `%s%s%s%s` o simili  
- [ ] Sì — scritture in memoria arbitrarie tramite `%n` sono **possibili**, portando potenzialmente a una Remote Code Execution (RCE)  

### Le moderne protezioni binarie (ASLR, DEP, Stack Canaries) sono aggirabili tramite questa injection?
- [ ] No — l'ambiente è configurato in modo sicuro (hardened) e il leak di memoria **non può** essere utilizzato per bypassare le protezioni  
- [ ] Sì — la Format String Injection **può** essere utilizzata per esfiltrare indirizzi base, rendendo **possibile** il bypass di ASLR/DEP

---

## WSTG-INPV-14 — Testing for Incubated Vulnerabilities

Le vulnerabilità incubate (incubated vulnerabilities), chiamate anche vulnerabilità persistenti o di secondo ordine (second-order vulnerabilities), si verificano quando un payload malevolo viene memorizzato dall'applicazione in uno stato "freddo" e successivamente eseguito o elaborato in un contesto differente. Questo pattern di attacco colpisce tipicamente i sistemi di back-end, le console amministrative o gli strumenti di reportistica automatizzata che recuperano i dati da un database o un filesystem condiviso senza un'adeguata ri-validazione o output encoding. I pentester devono tracciare il ciclo di vita dell'input dell'utente dal punto di ingresso (l'"incubatore") a ogni posizione in cui quel dato viene infine renderizzato o utilizzato da altri componenti o applicazioni secondarie. Un exploit riuscito porta spesso a conseguenze ad alto impatto come l'administrative session hijacking tramite stored XSS o l'esfiltrazione di dati attraverso second-order SQL injection nei moduli di reportistica interna.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Strumenti:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Esistono endpoint in cui l'input controllato dall'utente viene memorizzato per un successivo recupero da parte di altri utenti o processi?
- [ ] No — l'applicazione **non** memorizza l'input dell'utente per un uso successivo  
- [ ] Sì — l'input **è** memorizzato in campi del database, file di log o impostazioni di configurazione  

### Viene applicata la validazione o la sanitizzazione dell'input prima che i dati vengano scritti nel layer di storage persistente?
- [ ] Sì — una validazione rigorosa basata su allow-list **è applicata** al punto di ingresso  
- [ ] Sì — una sanitizzazione generica **è applicata** ma il bypass **è possibile** tramite encoding o caratteri non standard  
- [ ] No — l'input viene memorizzato nella sua forma grezza (raw) e non validata  

### I dati memorizzati sono correttamente codificati o sanitizzati quando vengono renderizzati in interfacce amministrative o secondarie?
- [ ] Sì — l'output encoding contestuale (context-aware) **è applicato** ovunque i dati vengano renderizzati  
- [ ] Sì — l'encoding **è applicato** in alcune posizioni ma **manca** in altre (ad es., dashboard amministrativa interna)  
- [ ] No — i dati vengono renderizzati senza alcun encoding o sanitizzazione, permettendo l'esecuzione del payload  

### I payload memorizzati nel database possono influenzare i processi batch di back-end o i sistemi di monitoraggio out-of-band?
- [ ] No — i processi di back-end utilizzano metodi di parsing sicuri o query parametrizzate (parameterized queries)  
- [ ] Sì — i payload memorizzati **possono** innescare interazioni nella logica di back-end (ad es., CSV injection, command injection nei log)  
- [ ] Sì — i payload memorizzati **possono** innescare richieste out-of-band (OOB) verso server controllati dall'attaccante

---

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

## WSTG-INPV-16 — HTTP Request Smuggling

L'HTTP Request Smuggling (HRS) si verifica quando una catena di server HTTP (come un load balancer e un server web back-end) interpreta i limiti di una richiesta in modo differente, tipicamente a causa di header `Content-Length` e `Transfer-Encoding` in conflitto. Un attaccante sfrutta questa desincronizzazione per "contrabbandare" (smuggle) una voce nel buffer delle richieste del server back-end, anteponendo efficacemente un segmento controllato dall'attaccante alla successiva richiesta di un utente legittimo. Questa tecnica consente attacchi gravi, tra cui session hijacking, l'aggiramento dei controlli di sicurezza e il cache poisoning. L'exploitation viene solitamente eseguita tramite analisi basata sul tempo (timing-based analysis) o osservando risposte impreviste dal back-end quando vengono inviate richieste successive al server.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Critica / Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Strumenti:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### I server front-end e back-end gestiscono l'header `Transfer-Encoding: chunked` in modo coerente?
- [ ] Sì — entrambi i server gestiscono il chunked encoding in modo identico e la desincronizzazione **non è possibile**  
- [ ] No — i server interpretano gli header in modo diverso ma viene applicata la **sanitizzazione/normalizzazione**  
- [ ] No — la desincronizzazione CL.TE (Content-Length/Transfer-Encoding) **è possibile** *(Critica)*  
- [ ] No — la desincronizzazione TE.CL (Transfer-Encoding/Content-Length) **è possibile** *(Critica)*  

### L'attaccante può avvelenare la cache lato server o lato client (cache poisoning) utilizzando una richiesta contrabbandata?
- [ ] No — il caching è **disabilitato** o non suscettibile ad avvelenamento  
- [ ] Sì — le richieste contrabbandate **possono** reindirizzare gli utenti verso domini malevoli o iniettare script tramite cache poisoning  

### È possibile catturare le richieste di altri utenti o i session token tramite la concatenazione delle richieste?
- [ ] No — lo smuggling **non** consente l'esposizione di dati tra utenti diversi  
- [ ] Sì — lo smuggling consente di appendere la richiesta dell'utente successivo a un corpo `POST` controllato dall'attaccante, rendendo l'esfiltrazione **possibile**  

### L'infrastruttura utilizza HTTP/2 ed è suscettibile alla desincronizzazione H2.CL o H2.TE?
- [ ] No — l'applicazione utilizza solo HTTP/1.1 o HTTP/2 è implementato in modo sicuro senza downgrade  
- [ ] Sì — si verificano downgrade in chiaro (cleartext) da HTTP/2 a HTTP/1.1 e la desincronizzazione **è possibile**  

### Gli header malformati (ad es. `Transfer-Encoding:  chunked` con uno spazio) sono gestiti in modo sicuro?
- [ ] Sì — gli header malformati vengono rifiutati o normalizzati in tutti i livelli  
- [ ] No — la desincronizzazione TE.TE **è possibile** a causa di una gestione incoerente di header malformati o oscurati

---

## WSTG-INPV-17 — Testing for Host Header Injection

L'Host Header Injection si verifica quando un'applicazione si fida dell'header `Host` fornito dall'utente e lo incorpora nella logica lato server, come la generazione di link o i reindirizzamenti, senza una validazione sufficiente. Gli attaccanti manipolano questo header per facilitare il Web Cache Poisoning, il Password Reset Poisoning reindirizzando token sensibili verso un dominio esterno, o per bypassare i controlli di sicurezza basati sul routing degli header. L'esfruttamento riuscito permette a un attaccante di esfiltrare dati di sessione, eseguire Account Takeover o reindirizzare utenti ignari verso infrastrutture malevole. Questa vulnerabilità si riscontra comunemente in ambienti bilanciati (Load-balanced) o in applicazioni che generano dinamicamente URL assoluti basati sul contesto della richiesta in arrivo.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se l'header Host viene utilizzato per generare link sensibili nelle email, come i reset della password.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Strumenti:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### L'applicazione riflette il valore dell'header `Host` in link, reindirizzamenti o script?
- [ ] No — l'applicazione utilizza URL di base hardcoded o una configurazione validata rigorosamente  
- [ ] Sì — l'header `Host` viene riflesso ma validato rigorosamente rispetto a una whitelist  
- [ ] Sì — l'header `Host` viene riflesso e il bypass **è possibile** tramite valori malformati (es. `expected.com:attacker.com`)  
- [ ] Sì — l'header `Host` viene riflesso **senza** alcuna validazione  

### Vengono elaborati header alternativi relativi all'host dall'applicazione o dall'infrastruttura?
- [ ] No — gli header come `X-Forwarded-Host`, `X-Host` o `Forwarded` vengono ignorati  
- [ ] Sì — gli header alternativi vengono elaborati ma **non** sovrascrivono la logica interna  
- [ ] Sì — gli header alternativi **possono** essere utilizzati per sovrascrivere il valore dell'header `Host` primario  

### L'header `Host` può essere manipolato per avvelenare le email generate dal sistema (es. reset della password)?
- [ ] No — i link nelle email utilizzano un dominio statico e fidato dalla configurazione del server  
- [ ] Sì — l'header `Host` influenza i link delle email ma la validazione **non** può essere bypassata  
- [ ] Sì — l'header `Host` **può** essere manipolato per reindirizzare i token di reset della password verso un dominio controllato dall'attaccante *(Critico)*  

### L'applicazione è suscettibile al Web Cache Poisoning tramite l'header `Host`?
- [ ] No — non è presente alcun meccanismo di caching o l'header `Host` fa parte della cache key  
- [ ] Sì — è presente una cache ma questa valida rigorosamente o ignora l'header `Host` per gli input non inclusi nella chiave (unkeyed inputs)  
- [ ] Sì — esiste una cache e **può** essere avvelenata da un header `Host` iniettato per servire contenuti malevole ad altri utenti  

### Il server accetta header `Host` arbitrari per il routing dei virtual host?
- [ ] No — il server restituisce un errore o una pagina predefinita per gli header `Host` non riconosciuti  
- [ ] Sì — il server accetta qualsiasi header `Host`, il che **è** utile per attività di reconnaissance o enumerazione di servizi interni

---

## WSTG-INPV-18 — Testing for Server-Side Template Injection

Il Server-Side Template Injection (SSTI) si verifica quando un'applicazione incorpora l'input controllato dall'utente all'interno di un motore di template lato server senza un'adeguata sanificazione o escaping. Ciò consente a un utente malintenzionato di iniettare direttive di template malevole, che vengono poi eseguite sul server, portando potenzialmente alla compromissione totale del sistema tramite Remote Code Execution (RCE). L'SSTI si manifesta tipicamente in funzionalità come la generazione dinamica di email, pagine di profilo e sistemi di notifica che utilizzano motori come Jinja2, Twig o FreeMarker. Dal punto di vista di un attaccante, questa vulnerabilità viene spesso sfruttata per sottrarre variabili d'ambiente sensibili, leggere file interni o eseguire comandi di sistema sfruttando gli oggetti e i metodi nativi del motore di template.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Strumenti:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### L'applicazione utilizza un motore di template lato server per elaborare l'input dell'utente?
- [ ] No — l'applicazione **non** utilizza template lato server per i contenuti dinamici  
- [ ] Sì — i template vengono utilizzati ma l'input dell'utente **non** è incorporato nelle direttive del template  
- [ ] Sì — l'input dell'utente è incorporato nei template **senza** una corretta sanificazione  

### Le espressioni aritmetiche di base vengono valutate quando vengono iniettate nei campi di input?
- [ ] No — payload come `{{7*7}}` o `${7*7}` vengono riflessi come stringhe letterali  
- [ ] Sì — le espressioni vengono valutate (ad esempio, restituendo `49`), confermando che il motore di template **è vulnerabile**  

### È possibile accedere agli oggetti o ai metodi integrati del motore di template?
- [ ] No — l'accesso a oggetti, metodi o configurazioni pericolosi è **limitato** o protetto da sandbox  
- [ ] Sì — gli oggetti integrati (ad esempio, `self`, `config`, `__class__`) **possono** essere consultati per sottrarre dati sensibili  
- [ ] Sì — la Remote Code Execution (RCE) tramite chiamate di sistema o librerie del sistema operativo **è possibile**  

### Esiste un meccanismo di sandbox o di allowlist che impedisce l'esecuzione di direttive pericolose?
- [ ] Sì — è presente una allowlist rigorosa o una sandbox sicura e il bypass **non è possibile**  
- [ ] Sì — è presente una sandbox ma il bypass **è possibile** tramite specifici payload dipendenti dal motore  
- [ ] No — nessuna sandbox o allowlist **è applicata** al motore di template

---

## WSTG-INPV-19 — Testing for Server-Side Request Forgery (SSRF)

Il Server-Side Request Forgery si verifica quando un utente malintenzionato può indurre un'applicazione web a effettuare richieste non autorizzate dal server verso risorse interne o esterne. Manipolando i parametri che prevedono URL, hostname o indirizzi IP, un attaccante può bypassare i controlli a livello di rete come firewall e ACL per sondare segmenti di rete interna, accedere ai servizi di metadati cloud (IMDS) o interagire con API interne e database vulnerabili. Questa vulnerabilità si manifesta tipicamente in funzionalità che prevedono il recupero di immagini, la generazione di PDF, webhook o l'upload di file tramite URL. Dal punto di vista di un attaccante, l'SSRF è una potente primitiva utilizzata per effettuare il pivoting in ambienti isolati, esfiltrare dati di configurazione sensibili o potenzialmente ottenere l'esecuzione di codice remoto attraverso l'interazione con servizi interni come Redis o Memcached.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Strumenti:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### L'applicazione elabora URL o hostname forniti dall'utente?
- [ ] No — nessuna funzionalità accetta URL o hostname esterni  
- [ ] Sì — la funzionalità esiste ma l'input **è rigorosamente validato** rispetto a una allow-list  
- [ ] Sì — la funzionalità esiste e accetta URL forniti dall'utente  

### Sono applicati controlli di validazione o blacklist alla destinazione richiesta?
- [ ] Sì — viene applicata una **allow-list** rigorosa di domini/IP affidabili  
- [ ] Sì — viene utilizzata una **deny-list** (es. blocco di `127.0.0.1`, `169.254.169.254`)  
- [ ] No — **nessuna validazione** o filtraggio viene applicato all'input  

### È possibile bypassare i filtri tramite encoding, redirect o DNS rebinding?
- [ ] No — i filtri sono robusti e gestiscono i redirect/risoluzione DNS in modo sicuro  
- [ ] Sì — il bypass **è possibile** utilizzando URL encoding o formati IP alternativi (esadecimale, ottale)  
- [ ] Sì — il bypass **è possibile** tramite redirect HTTP 3xx verso target interni  
- [ ] Sì — il bypass **è possibile** tramite attacchi di DNS rebinding per risolvere verso IP interni  

### L'applicazione può raggiungere servizi di metadati interni o interfacce locali?
- [ ] No — la rete locale e gli endpoint dei metadati cloud **non sono raggiungibili**  
- [ ] Sì — l'accesso a localhost (`127.0.0.1`) o ai servizi di loopback **è possibile**  
- [ ] Sì — l'accesso ai servizi di metadati cloud (es. AWS/GCP/Azure IMDS) **è possibile** *(Critico)*  

### Qual è la natura della risposta SSRF?
- [ ] Blind — nessuna risposta o metadato viene restituito all'utente  
- [ ] Parziale — i metadati della risposta (header, dimensione, timing) vengono restituiti all'utente  
- [ ] Completa — il corpo completo della risposta della risorsa interna **viene riflesso** all'utente

---

## WSTG-INPV-20 — Mass Assignment

Il Mass Assignment, noto anche come Overposting o Insecure Deserialization dei parametri, si verifica quando un'applicazione web associa automaticamente i parametri delle richieste HTTP in entrata alle proprietà di oggetti interni senza un filtraggio adeguato degli attributi. Questa vulnerabilità è diffusa nei moderni framework MVC e nelle API RESTful dove i dati JSON, XML o form-encoded vengono deserializzati direttamente nei modelli di dati lato applicazione. Gli attaccanti sfruttano questo comportamento iniettando parametri aggiuntivi non elencati in una richiesta — come `isAdmin`, `role` o `account_balance` — per modificare campi sensibili che gli sviluppatori non intendevano esporre al controllo dell'utente. L'exploitation riuscita si traduce tipicamente in privilege escalation, modifica non autorizzata dei dati o bypass dei flussi critici della business logic.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Media* |

> *La gravità diventa Alta se è possibile modificare attributi amministrativi, livelli di permesso o campi di fatturazione.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### L'applicazione utilizza il binding automatico per i parametri delle richieste in entrata?
- [ ] No — i parametri sono mappati manualmente a variabili specifiche o oggetti sicuri  
- [ ] Sì — viene utilizzato il binding automatico ma vengono applicate **white-list** rigorose o DTO (Data Transfer Objects)  
- [ ] Sì — viene utilizzato il binding automatico e gli attributi interni sensibili **possono** essere modificati  

### È possibile iniettare con successo attributi amministrativi o relativi ai permessi?
- [ ] No — i parametri iniettati vengono ignorati, rimossi o causano un errore di validazione  
- [ ] Sì — i parametri extra vengono accettati ma **non** influenzano lo stato dell'oggetto nel backend  
- [ ] Sì — l'iniezione di parametri come `is_admin`, `role` o `permissions` permette **con successo** la privilege escalation *(Critico)*  

### È possibile modificare la business logic sensibile o i campi finanziari tramite overposting?
- [ ] No — campi come `price`, `balance` o `status` sono protetti dal mass assignment  
- [ ] Sì — gli attributi di business sensibili **possono** essere modificati aggiungendoli al corpo della richiesta JSON o form  

### L'applicazione rivela strutture di oggetti interni che facilitano il parameter guessing?
- [ ] No — le risposte includono solo i campi pubblici previsti e la documentazione è limitata  
- [ ] Sì — i campi degli oggetti interni sono visibili nelle risposte GET, rendendo **possibile** il parameter discovery  
- [ ] Sì — la documentazione delle API (Swagger/OpenAPI) elenca esplicitamente le proprietà interne che **possono** essere assegnate

---

## WSTG-ERRH-01 — Testing for Improper Error Handling

La gestione impropria degli errori si verifica quando un'applicazione web rivela dettagli tecnici sensibili—come stack trace, informazioni sullo schema del database o percorsi di file interni—attraverso le sue risposte di errore. Gli attaccanti innescano questi errori inviando input malformati, accedendo a risorse inesistenti o inducendo eccezioni lato server per mappare l'architettura sottostante dell'applicazione e identificare potenziali vettori per ulteriori exploit. Questa fuga di informazioni (Information Leakage) serve spesso come precursore di attacchi più gravi, inclusi SQL Injection o Path Traversal, fornendo all'attaccante precise specifiche sull'ambiente. Una implementazione sicura deve fornire messaggi generici e user-friendly, catturando la diagnostica dettagliata solo in log sicuri lato server.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### L'applicazione utilizza un gestore di errori generico e globale per le eccezioni non gestite?
- [ ] Sì — le pagine di errore generiche sono **abilitate** e non rivelano **alcun** dettaglio tecnico  
- [ ] Sì — vengono utilizzate pagine di errore personalizzate ma la fuga di informazioni **è possibile** tramite header specifici  
- [ ] No — le pagine di errore predefinite del server (es. Tomcat, IIS, Nginx) sono **visibili**  

### Le informazioni tecniche possono essere enumerate attraverso input malformati o casi limite?
- [ ] No — gli errori sono gestiti correttamente con ID di riferimento univoci per il logging  
- [ ] Sì — stack trace o query al database **vengono rivelati** nelle risposte  
- [ ] Sì — percorsi di file interni, variabili d'ambiente o stringhe di versione del server **vengono rivelati**  

### Le risposte delle API espongono oggetti di errore dettagliati o informazioni di debug?
- [ ] No — le API restituiscono codici di errore standardizzati e messaggi JSON bonificati  
- [ ] Sì — le API restituiscono oggetti di debug dettagliati o dettagli completi sull'eccezione nel corpo della risposta  
- [ ] Sì — gli stack trace sono inclusi nei campi `details` o `exception` della risposta JSON  

### L'applicazione si comporta in modo differente (Time-based o Content-based) quando si verificano errori?
- [ ] No — le firme delle risposte e le tempistiche sono coerenti indipendentemente dal tipo di errore  
- [ ] Sì — le risposte differenziali **possono** essere utilizzate per enumerare stati validi vs non validi (es. User Enumeration)

---

## WSTG-ERRH-02 — Testing for Stack Traces

Le stack trace vengono generate quando un'applicazione non riesce a gestire correttamente un'eccezione, con la conseguente divulgazione dello stato di esecuzione interno e della gerarchia delle chiamate all'utente finale. Per un penetration tester, queste tracce sono inestimabili in quanto rivelano il framework dell'applicazione, le versioni delle librerie, i percorsi del file system interno e il flusso logico, riducendo significativamente l'impegno richiesto per la ricognizione. L'exploitation consiste nel provocare intenzionalmente errori tramite input malformati, tipi di dati imprevisti o violazioni delle condizioni al contorno per forzare l'applicazione in uno stato instabile ed esfiltrare informazioni sullo stack tecnologico sottostante. Questa fuga di informazioni fornisce spesso il contesto necessario per identificare componenti di terze parti vulnerabili o per creare exploit specifici per difetti logici scoperti all'interno dei percorsi di codice divulgati.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media* |

> *La gravità aumenta a Media se la stack trace rivela dettagli architetturali sensibili, query al database o parametri di configurazione interni.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Vengono restituite stack trace complete al client in caso di errori dell'applicazione?
- [ ] No — l'applicazione restituisce pagine di errore generiche o messaggi di errore personalizzati  
- [ ] Sì — le stack trace sono **abilitate** ma solo per moduli specifici e non sensibili  
- [ ] Sì — le stack trace complete sono **abilitate** e visibili nel corpo della risposta HTTP  

### I messaggi di errore divulgano informazioni sensibili sull'ambiente?
- [ ] No — i messaggi di errore non contengono dettagli tecnici o ambientali  
- [ ] Sì — i messaggi rivelano **percorsi di file interni** o **strutture di directory** lato server  
- [ ] Sì — i messaggi rivelano **schemi di database**, **query SQL** o **versioni di librerie di terze parti**  

### È possibile generare stack trace manipolando i parametri di input?
- [ ] No — l'input malformato viene gestito correttamente o rifiutato dalla validazione  
- [ ] Sì — le tracce possono essere generate da **tipi di dati imprevisti** (es. invio di un array dove è prevista una stringa)  
- [ ] Sì — le tracce sono generate da **null byte**, **caratteri speciali** o **valori limite**  

### Viene applicato in modo coerente un gestore globale degli errori in tutta l'applicazione?
- [ ] Sì — un gestore di eccezioni globale è **applicato coerentemente** su tutti gli endpoint testati  
- [ ] Sì — è presente un gestore ma **non è applicato** a endpoint legacy, API o aggiunti di recente  
- [ ] No — non è stato **rilevato** alcun meccanismo di gestione globale degli errori, con conseguenti errori predefiniti del server

---

## WSTG-CRYP-01 — Test per la Sicurezza Debole del Transport Layer

Il test per la sicurezza debole del transport layer comporta l'analisi dell'implementazione e della configurazione di SSL/TLS per garantire la riservatezza e l'integrità dei dati durante il transito. Gli attaccanti mirano a configurazioni errate come protocolli obsoleti (SSLv2, TLS 1.0), cipher suite deboli (NULL, RC4 o anonime) e certificati non validi o con firme deboli per eseguire attacchi Man-in-the-Middle (MitM). Questo test solitamente riguarda gli endpoint del server web o del load balancer dell'applicazione in cui viene stabilita la comunicazione crittografata. Uno sfruttamento riuscito consente a un avversario di intercettare dati sensibili, dirottare le sessioni utente (hijacking) o declassare le connessioni a stati non sicuri attraverso il protocol downgrade o la decrittazione del traffico.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Media* |

> *La gravità è Alta se vengono trasmessi dati sensibili (PII, credenziali, session token); Media per il traffico informativo generale.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Sono supportati protocolli TLS/SSL obsoleti o non sicuri?
- [ ] No — solo TLS 1.2 e TLS 1.3 sono **abilitati**  
- [ ] Sì — TLS 1.0 o TLS 1.1 sono **abilitati**  
- [ ] Sì — SSLv2 o SSLv3 sono **abilitati** *(Critico)*  

### Il server accetta cipher suite deboli o non sicure?
- [ ] No — sono **supportate** solo cipher robuste e moderne (es. AES-GCM, ChaCha20)  
- [ ] Sì — sono **supportate** cipher con crittografia debole (es. 3DES, RC4) o lunghezza in bit ridotta  
- [ ] Sì — sono **supportate** cipher NULL, anonime (ADH) o export *(Critico)*  

### Il certificato del server è valido e attendibile?
- [ ] Sì — il certificato è valido, corrisponde al dominio ed è firmato da una **CA attendibile**  
- [ ] No — il certificato è **scaduto** o utilizza un **algoritmo di firma debole** (es. SHA-1)  
- [ ] No — il certificato è **auto-firmato** o si verifica un **mismatch** del nome di dominio  

### Il server supporta la Secure Renegotiation e previene gli attacchi di Downgrade?
- [ ] Sì — la Secure Renegotiation è **supportata** e TLS Fallback SCSV è **abilitato**  
- [ ] No — la Secure Renegotiation **non è supportata**, consentendo potenzialmente l'iniezione MitM  
- [ ] No — il server è vulnerabile agli attacchi **POODLE** o **ROBOT**  

### La HTTP Strict Transport Security (HSTS) è implementata correttamente?

| Header | Presente | Mancante |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Sì — l'header HSTS è presente con un `max-age` lungo e `includeSubDomains` **abilitato**  
- [ ] Sì — l'header HSTS è presente ma manca `includeSubDomains` o ha un **max-age breve**  
- [ ] No — l'header HSTS **non è presente**

---

## WSTG-CRYP-02 — Testing for Padding Oracle

Le vulnerabilità di Padding Oracle si verificano quando il processo di decrittografia di un'applicazione rivela informazioni sulla validità o meno del padding di un testo cifrato (ciphertext) decifrato. Osservando queste risposte side-channel — come codici di stato HTTP distinti, messaggi di errore specifici o variazioni nei tempi di risposta — un attaccante può decrittografare i dati senza conoscere la chiave di cifratura e, potenzialmente, cifrare nuovamente testo in chiaro (plaintext) arbitrario. Questa vulnerabilità si manifesta tipicamente in cookie di sessione cifrati, token di autenticazione o parametri URL che utilizzano cifrari a blocchi (block ciphers) in modalità Cipher Block Chaining (CBC) con padding PKCS#7. Lo sfruttamento (exploitation) consente la perdita completa della riservatezza e può portare alla compromissione dell'integrità attraverso la costruzione di blocchi cifrati contraffatti.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica* |

> *La gravità è Critica se la vulnerabilità risiede nella gestione della sessione, nei token di identità o nella cifratura di PII.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Viene utilizzato un cifrario a blocchi in modalità CBC per dati sensibili?
- [ ] No — l'applicazione utilizza la cifratura autenticata (AES-GCM/ChaCha20-Poly1305)  
- [ ] Sì — l'applicazione utilizza cifrari a blocchi ma il padding **non è** richiesto (es. cifrari a flusso o modalità CTR)  
- [ ] Sì — l'applicazione utilizza la modalità CBC e il padding **viene applicato**  

### Il server rivela la validità del padding attraverso canali laterali (side channels)?
- [ ] No — il server restituisce messaggi di errore uniformi e tempi di risposta coerenti  
- [ ] Sì — il server restituisce codici di stato HTTP differenti (es. 200 vs 500) per errori di padding  
- [ ] Sì — il server restituisce messaggi di errore specifici (es. "Invalid Padding", "Decryption failed")  
- [ ] Sì — il server mostra differenze temporali misurabili durante il fallimento della decrittografia  

### È possibile la decrittografia automatizzata del ciphertext?
- [ ] No — i canali laterali **non sono** sufficientemente stabili per l'exploitation  
- [ ] Sì — il ciphertext **può** essere parzialmente decifrato (gli ultimi blocchi)  
- [ ] Sì — il recupero completo del plaintext **è possibile** utilizzando strumenti come `PadBuster`  

### L'attaccante può eseguire bit-flipping o contraffare ciphertext validi?
- [ ] No — i controlli di integrità (HMAC) vengono verificati **prima** della decrittografia  
- [ ] Sì — non è presente alcun controllo di integrità, ma la costruzione del payload **non è** fattibile  
- [ ] Sì — la costruzione di ciphertext arbitrari **è possibile**, consentendo il privilege escalation o l'injection di dati

---

## WSTG-CRYP-03 — Verifica dell'invio di informazioni sensibili tramite canali non crittografati

La trasmissione di dati sensibili tramite canali non crittografati, come il protocollo HTTP in chiaro, espone le informazioni all'intercettazione e alla modifica tramite attacchi Man-in-the-Middle (MITM). Questa vulnerabilità si verifica tipicamente quando le applicazioni web non riescono a imporre l'uso del Transport Layer Security (TLS) su tutti gli endpoint, in particolare nei componenti legacy, nei moduli di login o durante la transizione iniziale da HTTP a HTTPS. Dal punto di vista di un attaccante, ciò consente lo sniffing passivo di credenziali di autenticazione, session token e personally identifiable information (PII) su segmenti di rete compromessi o non attendibili, come il Wi-Fi pubblico. Garantire che tutti i dati sensibili siano incapsulati all'interno di un tunnel TLS robusto è fondamentale per mantenere la riservatezza e l'integrità delle interazioni degli utenti e prevenire il session hijacking.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Media |

> *La gravità diventa Alta se le credenziali di autenticazione, gli identificatori di sessione o le PII vengono trasmessi in chiaro.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### L'applicazione consente la comunicazione tramite HTTP non crittografato per endpoint sensibili?
- [ ] No — tutto il traffico dell'applicazione è rigorosamente forzato su HTTPS *(Più sicuro)*  
- [ ] Sì — le pagine non sensibili sono accessibili tramite HTTP, ma gli endpoint sensibili **non lo sono**  
- [ ] Sì — gli endpoint sensibili (login, checkout, profilo) **sono** accessibili tramite HTTP non crittografato *(Rischio Elevato)*  

### È presente un reindirizzamento globale da HTTP a HTTPS?
- [ ] Sì — l'applicazione esegue un reindirizzamento 301/302 verso HTTPS per tutte le richieste HTTP  
- [ ] Sì — il reindirizzamento viene eseguito solo per specifici endpoint sensibili  
- [ ] No — non viene applicato alcun reindirizzamento da HTTP a HTTPS  

### È implementato l'HTTP Strict Transport Security (HSTS)?
- [ ] Sì — HSTS è **abilitato** con un `max-age` esteso e include le direttive `includeSubDomains` e `preload`  
- [ ] Sì — HSTS è **abilitato** ma mancano le direttive `includeSubDomains` o `preload`  
- [ ] No — HSTS **non è abilitato** sul server dell'applicazione  

### I cookie sensibili sono protetti dalla trasmissione in chiaro?

| Nome Cookie | Flag Secure Presente | Flag Secure Mancante |
|-------------|:--------------------:|:-------------------:|
| `SessionID` |                      |                     |
| `AuthToken` |                      |                     |
| `CSRF-Token`|                      |                     |

### L'applicazione trasmette dati sensibili ad API di terze parti tramite canali non crittografati?
- [ ] No — tutte le chiamate API esterne vengono eseguite tramite tunnel crittografati (HTTPS)  
- [ ] Sì — alcune telemetrie non sensibili vengono inviate tramite HTTP  
- [ ] Sì — chiavi API, PII o credenziali **vengono** inviate a terze parti tramite HTTP non crittografato

---

## WSTG-CRYP-04 — Test per Primitive Crittografiche Deboli

Le primitive crittografiche deboli comportano l'uso di algoritmi obsoleti o insicuri, come MD5, SHA-1, DES o RC4, che sono suscettibili di attacchi di collisione, analisi delle frequenze o decrittazione tramite Brute Force. Queste debolezze si verificano tipicamente nell'hashing delle password, nella crittografia dei dati a riposo (data at rest), nella generazione di token di sessione o nella verifica delle firme digitali all'interno del backend dell'applicazione. Dal punto di vista di un attaccante, l'identificazione di queste primitive consente di compromettere l'integrità e la riservatezza dei dati, ad esempio craccando gli hash intercettati per recuperare le password in chiaro (cleartext) o contraffacendo i token di autenticazione. Le applicazioni moderne dovrebbero invece utilizzare primitive standard di settore, resistenti alle collisioni e ad alta entropia come Argon2, Bcrypt o AES-GCM per garantire una protezione robusta contro la crittoanalisi.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Vengono utilizzati algoritmi di hashing deprecati come MD5 o SHA-1 per l'archiviazione di dati sensibili?
- [ ] No — l'applicazione utilizza moderni algoritmi resistenti alle collisioni come Argon2 o Bcrypt  
- [ ] Sì — vengono utilizzati algoritmi deprecati ma **solo** per controlli di integrità non sensibili ai fini della sicurezza  
- [ ] Sì — gli algoritmi deprecati **vengono utilizzati** per password o token sensibili *(Alta)*  

### Vengono attualmente impiegati cifrari di crittografia simmetrica deboli come DES, 3DES o RC4?
- [ ] No — viene utilizzato AES (128 bit o superiore) in una modalità sicura come GCM o CBC  
- [ ] Sì — i cifrari legacy sono **abilitati** per la retrocompatibilità ma **non** sono l'impostazione predefinita  
- [ ] Sì — i cifrari legacy sono il metodo principale per proteggere i dati a riposo o in transito  

### L'applicazione implementa logiche crittografiche personalizzate ("roll-your-own") o non standard?
- [ ] No — l'applicazione aderisce rigorosamente a librerie crittografiche standard e sottoposte a peer-review  
- [ ] Sì — esistono wrapper personalizzati ma le primitive sottostanti **sono** standard di settore  
- [ ] Sì — algoritmi crittografici proprietari o logiche personalizzate **vengono applicati** a dati sensibili *(Critica)*  

### I salt crittografici e i vettori di inizializzazione (IV) sono gestiti secondo le best practice?
- [ ] Sì — i salt sono univoci per utente e gli IV sono imprevedibili per ogni operazione  
- [ ] Sì — i salt vengono utilizzati ma **non** sono univoci per tutti i record  
- [ ] No — i salt sono statici, hardcoded o assenti, e gli IV **vengono riutilizzati** tra più sessioni  

### La lunghezza in bit delle chiavi asimmetriche (RSA, Diffie-Hellman) è sufficiente per gli standard moderni?
- [ ] Sì — le chiavi RSA/DH sono a 2048 bit o superiori, oppure viene utilizzata la crittografia a curve ellittiche (ECC)  
- [ ] No — le chiavi sono inferiori a 2048 bit ma il bypass **non** è banale  
- [ ] No — le chiavi sono a 1024 bit o inferiori, rendendole **vulnerabili** alla fattorizzazione o ad attacchi computazionali

---

## WSTG-BUSL-01 — Test Business Logic Data Validation

Il testing della validazione dei dati della Business Logic si concentra sulla capacità dell'applicazione di identificare e rifiutare dati sintatticamente corretti ma logicamente non validi nel contesto del dominio di business. Gli attaccanti sfruttano queste falle inviando valori inaspettati—come quantità negative in un carrello della spesa, date nel passato per eventi futuri o interi estremamente grandi—per eludere i vincoli e manipolare lo stato dell'applicazione. Queste vulnerabilità risiedono spesso in workflow multi-step, processi di checkout e moduli di gestione dell'account dove la logica lato server non riesce a verificare l'integrità contestuale dei dati forniti dall'utente. Uno sfruttamento (Exploit) riuscito può portare a perdite finanziarie, compromissione dell'integrità e accesso non autorizzato a funzionalità o dati riservati.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### L'applicazione impone limiti di intervallo logico sugli input numerici?
- [ ] Sì — l'applicazione rifiuta correttamente valori negativi, zero o eccessivamente grandi dove inappropriato *(Più sicuro)*  
- [ ] Sì — l'applicazione rifiuta alcuni intervalli non validi ma **è** vulnerabile a integer overflow o bypass dei limiti (boundary bypass)  
- [ ] No — l'applicazione accetta qualsiasi valore numerico indipendentemente dal contesto di business *(Critico)*  

### I controlli di validazione lato server sono coerenti in tutti i layer dell'applicazione?
- [ ] Sì — la validazione è applicata in modo coerente sia a livello API/servizio che a livello database  
- [ ] Sì — la validazione **è applicata** nella UI/Frontend ma il bypass tramite richiesta API diretta **è possibile**  
- [ ] No — la validazione **non è applicata** o è incoerente, consentendo la corruzione logica dei dati  

### La business logic può essere sovvertita manipolando i dati in workflow multi-step?
- [ ] No — lo stato è mantenuto in modo sicuro sul server e i dati dei passaggi precedenti **non possono** essere manomessi  
- [ ] Sì — la validazione avviene nei primi passaggi ma l'invio finale **non** verifica nuovamente l'integrità dei dati  
- [ ] Sì — il bypass del workflow **è possibile** saltando passaggi o inviando dati fuori ordine  

### L'applicazione gestisce correttamente i dati relativi a "edge case" che eludono i vincoli di business?
- [ ] Sì — i vincoli sono robusti e gestiscono input insoliti ma sintatticamente validi (ad es. anni bisestili, cambi di fuso orario)  
- [ ] Sì — alcuni casi limite (edge case) sono gestiti ma combinazioni specifiche **possono** portare a comportamenti inaspettati  
- [ ] No — i casi limite portano direttamente a fallimenti logici, come acquisti gratuiti o cambi di stato non autorizzati

---

## WSTG-BUSL-02 — Test Ability to Forge Requests

La falsificazione delle richieste (Forging requests) all'interno di un contesto di business logic comporta la manipolazione di parametri definiti dall'applicazione per eseguire azioni al di fuori del workflow previsto o del livello di autorizzazione assegnato. Gli attaccanti prendono di mira processi multi-fase, come i sistemi di checkout o la registrazione di account, in cui lo stato viene mantenuto tramite parametri lato client che il server non riesce a ri-validare rispetto a una sorgente backend fidata. Alterando campi nascosti, valori JSON o parametri REST come prezzi, quantità o flag di stato interni, un attaccante può eludere i requisiti di pagamento, ottenere un'escalation dei privilegi o innescare cambiamenti di stato non intenzionali. Questa vulnerabilità si manifesta tipicamente in form web stateful o API in cui il server confida implicitamente nella rappresentazione dello stato di business fornita dal client durante una transazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica* |

> *La gravità diventa Critica se la richiesta falsificata consente perdite finanziarie, azioni amministrative non autorizzate o il completo account takeover.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### L'applicazione convalida i parametri transazionali rispetto a una "sorgente di verità" lato server?
- [ ] Sì — tutti i parametri sensibili (prezzo, ruolo, ID) sono validati rispetto al database e il bypass **non è possibile**  
- [ ] Sì — la validazione è applicata ma può essere elusa tramite parameter pollution o encoding alternativi  
- [ ] No — l'applicazione si fida dei valori lato client per determinare il costo o l'esito di una transazione *(Critico)*  

### I valori critici per il business (es. quantità, importi) possono essere modificati in valori non autorizzati?
- [ ] No — valori negativi, valori zero o valori eccessivamente alti vengono rifiutati dal server  
- [ ] Sì — il server accetta valori modificati ma solo entro un intervallo limitato  
- [ ] Sì — valori negativi o zero **possono** essere inviati, portando a bypass finanziari o di logica  

### Vengono utilizzati campi nascosti o parametri API per mantenere lo stato attraverso processi multi-fase?
- [ ] No — lo stato è mantenuto in modo sicuro tramite sessioni lato server o token firmati  
- [ ] Sì — lo stato viene passato tramite il client ma i controlli di integrità (HMAC/Firme) sono **abilitati** e robusti  
- [ ] Sì — lo stato viene passato tramite il client e i controlli di integrità sono **disabilitati** o possono essere rimossi  

### È possibile falsificare richieste che saltano passaggi intermedi obbligatori in un workflow?
- [ ] No — il server impone una sequenza rigorosa di operazioni per ogni transazione  
- [ ] Sì — i passaggi **possono** essere saltati ma l'azione finale richiede comunque dati validi dai passaggi precedenti  
- [ ] Sì — la richiesta finale di "invio" o "completamento" **può** essere falsificata direttamente per eludere le fasi di autorizzazione o pagamento

---

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

## WSTG-BUSL-04 — Test for Process Timing

Gli attacchi di process timing prevedono la misurazione del tempo impiegato da un'applicazione per elaborare specifiche richieste al fine di inferire informazioni sensibili sugli stati interni o sull'esistenza di dati. Analizzando discrepanze a livello di millisecondi nei tempi di risposta — spesso causate da logiche condizionali con uscita anticipata (early-exit), ricerche nel database o confronti crittografici non a tempo costante — gli attaccanti possono eseguire un'analisi side-channel per enumerare username validi, verificare frammenti di password o confermare l'esistenza di record specifici. Queste vulnerabilità si verificano tipicamente negli endpoint di autenticazione, nei moduli di reset della password e nei filtri API ad alta intensità di risorse dove la logica di backend si dirama in base alla validità dell'input. Dal punto di vista di un attaccante, queste differenze temporali fungono da "boolean oracle" che rivela verità sui dati di backend anche quando l'applicazione restituisce messaggi di errore generici e identici all'utente.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Strumenti:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### L'applicazione presenta tempi di risposta variabili per richieste di risorse valide rispetto a quelle non valide?
- [ ] No — i tempi di risposta sono identici indipendentemente dalla validità dell'input  
- [ ] Sì — esistono differenze di temporizzazione trascurabili ma **non** sono statisticamente significative  
- [ ] Sì — differenze di temporizzazione coerenti sono **osservabili** e forniscono un oracolo affidabile  

### Sono implementate funzioni di confronto a tempo costante o ritardi artificiali per normalizzare i tempi di risposta?
- [ ] Sì — algoritmi a tempo costante sono **applicati** a tutte le operazioni di confronto sensibili *(Più sicuro)*  
- [ ] Sì — ritardi artificiali o jitter sono **abilitati**, ma la logica sottostante rimane non a tempo costante  
- [ ] No — non è **applicata** alcuna mitigazione della temporizzazione, consentendo la misurazione diretta dei rami logici  

### Un attaccante può utilizzare l'analisi statistica per isolare il segnale di temporizzazione dal rumore di rete?
- [ ] No — il jitter di rete e il rumore dell'applicazione rendono l'analisi della temporizzazione **non possibile**  
- [ ] Sì — con una dimensione del campione sufficiente, il segnale di temporizzazione **può** essere isolato dal rumore  
- [ ] Sì — il delta di temporizzazione è sufficientemente grande che la normalizzazione statistica **non** è richiesta  

### L'account enumeration è possibile tramite le funzionalità di login o di reset della password?
- [ ] No — l'esistenza dell'account non può essere determinata tramite la temporizzazione  
- [ ] Sì — le discrepanze di temporizzazione consentono a un attaccante remoto di effettuare l'**enumeration** di username o email validi  

### Le operazioni ad alta intensità di risorse (es. elaborazione di file, ricerche complesse) espongono l'esistenza di dati tramite la temporizzazione?
- [ ] No — il consumo di risorse è uniforme indipendentemente dal fatto che un record venga trovato o meno  
- [ ] Sì — l'esistenza di record sensibili **può** essere inferita misurando la durata dell'elaborazione backend

---

## WSTG-BUSL-05 — Test dei limiti sul numero di volte in cui una funzione può essere utilizzata

Questo test valuta se un'applicazione impone vincoli di business logic sulla frequenza e sul numero totale di volte in cui una specifica azione o funzione può essere eseguita da un singolo utente, sessione o indirizzo IP. La mancata implementazione di questi limiti consente agli attaccanti di abusare di funzioni ad alto valore—come l'invio di SMS OTP, l'attivazione di email di password reset, l'applicazione di codici sconto o l'esecuzione di pesanti data exports—portando a perdite finanziarie, esaurimento delle risorse o danni reputazionali. Queste vulnerabilità si trovano tipicamente negli endpoint transazionali o nelle funzionalità di comunicazione e vengono sfruttate tramite automated scripts che ripetono l'azione ben oltre le soglie di business previste. Dal punto di vista di un attaccante, questo è un obiettivo primario per SMS pumping, mail bombing o workflow di brute-forcing che mancano di espliciti rate-limiting o controlli basati sul conteggio (count-based throttles).


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se la funzione coinvolge transazioni finanziarie, costi SMS o impatta sulla disponibilità a livello di sistema.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### L'applicazione definisce e impone limiti sulle funzioni di business sensibili?
- [ ] Sì — i limiti sono rigorosamente imposti e il bypass **non è possibile**  
- [ ] Sì — i limiti sono imposti ma sono troppo elevati per prevenire l'abuso  
- [ ] No — nessun limite è applicato all'esecuzione della funzione  

### I rate limits e i contatori di utilizzo sono imposti lato server?
- [ ] Sì — l'imposizione è rigorosamente lato server e **non può** essere manipolata  
- [ ] No — l'imposizione si affida alla logica lato client (es. JavaScript o campi nascosti)  
- [ ] No — non esiste alcuna imposizione a nessun livello  

### I limiti di utilizzo possono essere bypassati tramite la manipolazione di header o sessioni?
- [ ] No — il bypass tramite `X-Forwarded-For`, `Origin` o la rotazione della sessione **non è possibile**  
- [ ] Sì — i limiti **possono** essere bypassati tramite lo spoofing degli header IP o la cancellazione dei cookie  
- [ ] Sì — i limiti **possono** essere bypassati creando account o sessioni multiple  

### Il superamento del limite attiva una risposta difensiva appropriata?
- [ ] Sì — l'applicazione restituisce `429 Too Many Requests` e registra l'evento *(Più sicuro)*  
- [ ] Sì — l'applicazione restituisce un errore ma **non** registra il potenziale abuso  
- [ ] No — l'applicazione continua a elaborare le richieste ma ignora l'output  
- [ ] No — l'applicazione non fornisce alcuna risposta o va in crash sotto carico  

### L'impatto dell'abuso della funzione è significativo per il business?
- [ ] No — l'abuso della funzione ha un costo o un impatto sulle risorse trascurabile  
- [ ] Sì — l'abuso **può** portare a un consumo minore di risorse o al fastidio dell'utente  
- [ ] Sì — l'abuso **può** portare a costi finanziari significativi (es. tariffe SMS) o al denial of service *(Critico)*

---

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

## WSTG-BUSL-07 — Test delle Difese contro l'Abuso dell'Applicazione

Le difese contro l'abuso dell'applicazione valutano la capacità dell'applicazione di identificare, loggare e rispondere a comportamenti anomali degli utenti che deviano dalla logica di business prevista. A differenza della sicurezza tradizionale basata su firme, queste difese si concentrano sul rilevamento di pattern come richieste a raffica (rapid-fire), Credential Stuffing o enumerazione sequenziale delle risorse che indicano un abuso automatizzato. Dal punto di vista di un attaccante, questo test determina la resilienza dell'applicazione contro l'automazione e gli attacchi "low and slow" progettati per esfiltrare dati o esaurire le risorse senza attivare i normali avvisi di sicurezza. La mancata implementazione di queste difese spesso si traduce in massiccio Data Scraping, Account Takeover o Denial of Service attraverso funzionalità dell'applicazione legittime ma ad alta intensità di risorse.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Sono applicati meccanismi di rate limiting o throttling sugli endpoint sensibili?
- [ ] Sì — un Rate Limiting rigoroso **è applicato** e imposto su tutti gli endpoint sensibili *(Massima sicurezza)*  
- [ ] Sì — il Rate Limiting **è applicato** ma può essere bypassato tramite rotazione degli IP o manipolazione degli header  
- [ ] Sì — il Rate Limiting **è applicato** solo agli endpoint di autenticazione, lasciando gli altri esposti  
- [ ] No — nessun Rate Limiting o Throttling **è applicato** ad alcuna funzionalità dell'applicazione  

### L'applicazione rileva e blocca i pattern di interazione automatizzata?
- [ ] Sì — l'analisi comportamentale o i CAPTCHA **sono abilitati** e bloccano efficacemente i bot  
- [ ] Sì — l'anti-automazione **è abilitata** ma il bypass **è possibile** utilizzando browser headless o rotazione delle sessioni  
- [ ] No — l'applicazione **non può** distinguere tra un utente umano e uno script automatizzato  

### Il comportamento anomalo viene loggato e seguito da una risposta difensiva?
- [ ] Sì — l'abuso attiva il blocco automatizzato e avvisa il team di sicurezza  
- [ ] Sì — l'abuso viene loggato per audit ma **nessuna** azione difensiva automatizzata viene intrapresa  
- [ ] No — l'applicazione **non logga** né risponde a pattern di attività insoliti  

### Le difese possono essere bypassate manipolando identificativi o header lato client?
- [ ] No — le difese si basano sullo stato lato server e su telemetria verificata  
- [ ] Sì — i controlli **possono** essere bypassati cancellando i cookie o ruotando le stringhe di `User-Agent`  
- [ ] Sì — i controlli **possono** essere bypassati iniettando header come `X-Forwarded-For` o `X-Real-IP`

---

## WSTG-BUSL-08 — Test del Caricamento di Tipi di File Non Previsti

Il caricamento di tipi di file non previsti consente agli aggressori di bypassare i vincoli della business logic inviando file che l'applicazione non è destinata a elaborare, portando potenzialmente a Remote Code Execution, Cross-Site Scripting (XSS) o Denial of Service. Gli aggressori sondano queste vulnerabilità modificando le estensioni dei file, i tipi MIME e i magic bytes per indurre la logica di validazione lato server ad accettare contenuti malevoli. Ciò si verifica tipicamente nei caricamenti delle immagini del profilo, nei sistemi di gestione documentale o negli allegati dei ticket di supporto dove non esiste una validazione sufficiente sul back-end. Uno sfruttamento (Exploit) riuscito può compromettere il server ospite se il file caricato viene eseguito, o portare ad attacchi lato client se il file viene restituito ad altri utenti con un `Content-Type` errato.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### L'applicazione limita il caricamento dei file a un set specifico di estensioni?
- [ ] Sì — viene applicata una whitelist rigorosa di estensioni e il bypass **non è possibile**  
- [ ] Sì — è presente una whitelist di estensioni ma il bypass **è possibile** tramite doppie estensioni o null bytes  
- [ ] No — qualsiasi estensione di file **può** essere caricata  

### Il contenuto del file viene validato oltre l'estensione del file?
- [ ] Sì — la logica lato server verifica i Magic Bytes ed esegue un'ispezione approfondita del contenuto  
- [ ] Sì — la logica lato server verifica il tipo MIME solo tramite l'header `Content-Type`  
- [ ] No — nessuna validazione del contenuto **viene applicata** dopo il controllo dell'estensione  

### Un aggressore può eseguire codice caricando una web shell o uno script?
- [ ] No — i file caricati sono memorizzati in una directory non eseguibile o in un bucket di archiviazione (S3/Azure Blob)  
- [ ] Sì — i file sono memorizzati in una directory accessibile via web ma l'esecuzione **è disabilitata** tramite la configurazione del server  
- [ ] Sì — gli script malevoli caricati **possono** essere eseguiti direttamente tramite un percorso URL prevedibile *(Critica)*  

### Sono possibili attacchi lato client come XSS tramite i tipi di file caricati?
- [ ] No — i file vengono serviti con `Content-Disposition: attachment` e `X-Content-Type-Options: nosniff`  
- [ ] Sì — i file SVG o HTML **possono** essere caricati e renderizzati nel browser, portando a XSS  
- [ ] Sì — i metadati dell'immagine (EXIF) **vengono elaborati** e riflessi nella UI senza sanitizzazione

---

## WSTG-BUSL-09 — Test del Caricamento di File Malevoli

Le funzionalità di caricamento file consentono agli utenti di inviare dati al server, fornendo un vettore diretto per l'introduzione di contenuti malevoli nell'ambiente dell'applicazione. Gli attaccanti sfruttano logiche di validazione deboli per caricare web shell, malware o file appositamente creati — come SVG o HTML — per ottenere Remote Code Execution (RCE) o Cross-Site Scripting (XSS). Questa vulnerabilità si trova tipicamente in funzionalità quali impostazioni del profilo, sistemi di gestione documentale e gestori di allegati in cui il server non verifica correttamente i tipi di file, i contenuti e i permessi di esecuzione. Lo sfruttamento riuscito porta spesso alla compromissione totale del sistema, all'esfiltrazione di dati o all'uso del server come punto di appoggio (pivot point) per attacchi alla rete interna.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### L'applicazione fornisce funzionalità per il caricamento di file?
- [ ] No — non esiste alcuna funzionalità di caricamento file  
- [ ] Sì — la funzionalità esiste per gli utenti autenticati  
- [ ] Sì — la funzionalità è disponibile per gli utenti **non autenticati** *(Critico)*  

### La validazione dell'estensione del file è forzata lato server?
- [ ] Sì — viene applicata una allowlist rigorosa di estensioni e il bypass **non è possibile**  
- [ ] Sì — viene utilizzata una allowlist ma il bypass **è possibile** tramite case sensitivity o doppie estensioni  
- [ ] Sì — viene utilizzata una blocklist, che **può** essere bypassata con estensioni alternative (es. `.phtml`, `.asa`)  
- [ ] No — nessuna validazione dell'estensione è **applicata** lato server  

### Il contenuto del file viene validato oltre l'estensione o il tipo MIME?
- [ ] Sì — l'applicazione verifica i magic bytes ed esegue un'ispezione approfondita del contenuto  
- [ ] Sì — l'applicazione controlla solo l'header `Content-Type`, il quale **è** facilmente falsificabile  
- [ ] No — l'applicazione si affida interamente all'estensione del file per la validazione  

### I file caricati sono memorizzati in una directory con permessi di esecuzione?
- [ ] No — i file sono memorizzati su un servizio di storage dedicato (es. S3) o in una directory non eseguibile  
- [ ] Sì — i file sono memorizzati sul server web ma l'esecuzione è **disabilitata** tramite configurazione  
- [ ] Sì — i file sono memorizzati in una directory accessibile via web e l'esecuzione **è abilitata** *(Critico)*  

### I filtri di sicurezza possono essere bypassati tramite la manipolazione del nome del file?
- [ ] No — i nomi dei file vengono sanitizzati o rigenerati casualmente dal server  
- [ ] Sì — i filtri **possono** essere bypassati utilizzando null byte (es. `shell.php%00.jpg`)  
- [ ] Sì — i caratteri di path traversal (es. `../../`) **possono** essere utilizzati per caricare file in directory arbitrarie

---

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

## WSTG-CLNT-01 — Testing for DOM Based Cross Site Scripting

Il Cross-Site Scripting basato su DOM (DOM XSS) si verifica quando un'applicazione contiene JavaScript lato client che elabora dati provenienti da una sorgente non attendibile in modo non sicuro, solitamente scrivendo i dati nel Document Object Model (DOM) tramite un sink pericoloso. A differenza del Cross-Site Scripting (XSS) riflesso o memorizzato, la vulnerabilità risiede interamente nel codice lato client; il Payload spesso non viene affatto inviato al server, poiché viene elaborato da sink come `innerHTML`, `eval()` o `document.write()`. Gli attaccanti sfruttano questa vulnerabilità creando URL con frammenti o parametri malevoli che, quando caricati dal browser della vittima, eseguono JavaScript arbitrario nel contesto della sessione dell'utente. Ciò può portare all'esfiltrazione di token di sessione, al furto di dati sensibili e ad azioni non autorizzate eseguite per conto dell'utente autenticato.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Strumenti:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Vengono utilizzate sorgenti non attendibili per catturare dati controllati dall'utente in JavaScript?
- [ ] No — JavaScript **non** utilizza `location.hash`, `location.search` o `document.referrer`  
- [ ] Sì — vengono utilizzate sorgenti non attendibili ma i dati **non** vengono passati ad alcun sink di esecuzione  
- [ ] Sì — vengono utilizzate sorgenti non attendibili e i dati fluiscono direttamente nella logica lato client  

### I dati controllati dall'utente vengono passati a sink DOM pericolosi?
- [ ] No — sink come `innerHTML`, `outerHTML`, `document.write()` o `eval()` **non** vengono utilizzati  
- [ ] Sì — sono presenti sink pericolosi ma i dati vengono rigorosamente **sanificati** utilizzando una libreria sicura come DOMPurify  
- [ ] Sì — sono presenti sink pericolosi e i dati vengono elaborati con un filtraggio **insufficiente** o basato su regex personalizzate  
- [ ] Sì — sono presenti sink pericolosi e i dati vengono passati **senza** alcuna sanificazione o codifica *(Critico)*  

### Il sink di esecuzione può essere raggiunto tramite un payload basato su URL?
- [ ] No — il flusso da sorgente a sink (source-to-sink) **non** può essere attivato tramite input esterno  
- [ ] Sì — il flusso è **possibile** ma richiede un'interazione complessa dell'utente  
- [ ] Sì — il flusso è **possibile** e può essere attivato tramite un semplice frammento URL o parametro di query  

### Gli header di sicurezza moderni o le mitigazioni basate sul browser stanno neutralizzando il rischio?
- [ ] Sì — una `Content-Security-Policy` (CSP) rigorosa con `script-src` e `trusted-types` è **abilitata** e impedisce l'esecuzione  
- [ ] Sì — è presente una CSP ma `unsafe-inline` o hash deboli rendono **possibile** un bypass  
- [ ] No — non è presente alcuna CSP per mitigare l'esecuzione basata su DOM  

### L'applicazione utilizza framework lato client con protezioni integrate?
- [ ] Sì — il framework (ad es. Angular, React) codifica automaticamente i dati e il bypass **non è possibile**  
- [ ] Sì — il framework viene utilizzato ma sono abilitati "escape hatches" come `dangerouslySetInnerHTML`  
- [ ] No — l'applicazione utilizza JavaScript puro (vanilla JavaScript) o librerie legacy (ad es. vecchie versioni di jQuery) senza auto-escaping

---

## WSTG-CLNT-02 — Test dell'esecuzione di JavaScript

Il test dell'esecuzione di JavaScript si concentra sull'identificazione di istanze in cui un'applicazione gestisce in modo improprio i dati forniti dall'utente, portando all'esecuzione di script non autorizzati nel contesto del browser della vittima. Questa vulnerabilità si traduce tipicamente in Cross-Site Scripting (XSS), che consente agli aggressori di sottrarre i session cookie, manipolare il Document Object Model (DOM) o eseguire azioni per conto dell'utente. I Penetration tester esaminano sink come `innerHTML`, `document.write()` ed `eval()` dove possono essere elaborati dati non sanificati provenienti da sorgenti come frammenti URL, `localStorage` o `window.name`. Un Exploit andato a buon fine compromette l'integrità e la riservatezza della sessione dell'utente e può fungere da pivot per attacchi client-side più complessi.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Strumenti:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### L'applicazione elabora dati forniti dall'utente in sink JavaScript pericolosi?
- [ ] No — tutti i dati sono sanificati o utilizzati in sink sicuri come `textContent`  
- [ ] Sì — i dati sono elaborati in sink pericolosi ma la sanificazione/codifica **viene applicata**  
- [ ] Sì — i dati sono elaborati in sink pericolosi e la sanificazione **non viene applicata**  

### È presente una Content Security Policy (CSP) per mitigare l'esecuzione di script?
- [ ] Sì — una CSP restrittiva è **abilitata** e il bypass **non è possibile**  
- [ ] Sì — una CSP è **abilitata** ma il bypass **è possibile** tramite `unsafe-inline` o CDN non sicure  
- [ ] No — nessuna CSP è **abilitata**  

### JavaScript può essere eseguito tramite parametri URL o frammenti (DOM-based XSS)?
- [ ] No — l'input è correttamente codificato e l'esecuzione **non è possibile**  
- [ ] Sì — l'input si riflette nel DOM ma l'esecuzione **è impedita** dalle protezioni dei framework moderni  
- [ ] Sì — l'input viene eseguito direttamente nel browser e l'exploitation **è possibile**  

### Gli event handler o gli schemi URI sono vulnerabili alla script injection?
- [ ] No — l'input è sanificato per rimuovere gli attributi evento e gli schemi `javascript:`  
- [ ] Sì — gli event handler **possono** essere iniettati ma i filtri limitano la complessità del payload  
- [ ] Sì — l'esecuzione arbitraria di JavaScript tramite gli attributi `onerror`, `onload` o `href` **è possibile**

---

## WSTG-CLNT-03 — HTML Injection

L'HTML Injection si verifica quando un'applicazione non riesce a sanitizzare l'input fornito dall'utente prima di eseguirne il rendering all'interno del browser, consentendo a un utente malintenzionato di iniettare tag HTML arbitrari nel Document Object Model (DOM) della pagina web. Sebbene sia simile al Cross-Site Scripting (XSS), l'obiettivo principale dell'HTML Injection è manipolare la presentazione visiva della pagina o facilitare attacchi di phishing mediante l'iniezione di form malevoli e contenuti ingannevoli. Gli aggressori prendono di mira i parametri che vengono riflessi nell'interfaccia utente (UI), come i termini di ricerca, i campi del profilo utente o i messaggi di errore, per indurre le vittime a rivelare credenziali sensibili o a interagire con link di terze parti non autorizzati. Questa vulnerabilità rappresenta un rischio significativo per l'integrità dell'interfaccia utente dell'applicazione e può essere un precursore di attacchi di ingegneria sociale o di sessione più complessi.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Gli input controllati dall'utente vengono riflessi nel DOM senza una corretta codifica HTML?
- [ ] No — tutto l'input riflesso viene rigorosamente codificato in HTML prima di essere renderizzato  
- [ ] Sì — alcuni caratteri **sono** codificati, ma sono possibili **bypass** per determinati tag  
- [ ] Sì — l'input viene riflesso in formato grezzo (raw) e l'HTML injection **è possibile**  

### È possibile iniettare elementi HTML strutturali per alterare il layout della pagina?
- [ ] No — l'applicazione filtra o rimuove i tag strutturali come `<div>`, `<table>` o `<iframe>`  
- [ ] Sì — gli elementi strutturali **possono** essere iniettati ma **non** impattano significativamente la UI  
- [ ] Sì — un defacement significativo della UI **è possibile** tramite tag iniettati  

### È possibile iniettare elementi di phishing funzionali come form o link malevoli?
- [ ] No — i tag `<form>`, `<input>` e `<a>` sono efficacemente bloccati o sanitizzati  
- [ ] Sì — link malevoli **possono** essere iniettati per reindirizzare gli utenti verso siti esterni  
- [ ] Sì — elementi `<form>` funzionali **possono** essere iniettati per la sottrazione di credenziali *(Media)*  

### Gli header di sicurezza o le Content Security Policy (CSP) mitigano l'impatto dell'iniezione?
- [ ] Sì — è attiva una CSP restrittiva e vengono applicati `form-action` o `frame-src`  
- [ ] No — gli header di sicurezza sono presenti ma la CSP **è disabilitata** o contiene unsafe-inline/wildcard  
- [ ] No — non è abilitata alcuna CSP o header di sicurezza pertinente

---

## WSTG-CLNT-04 — Testing for Client-Side URL Redirect

La redirezione URL lato client (Client-side URL redirection) si verifica quando un'applicazione web accetta input controllati dall'utente — tipicamente attraverso parametri URL o frammenti hash — e li utilizza all'interno di un sink di redirezione lato client come `window.location` o `document.location.href`. Questa vulnerabilità viene sfruttata principalmente dagli attaccanti per condurre campagne di phishing sofisticate, poiché la vittima inizialmente si fida del dominio legittimo prima di essere silenziosamente reindirizzata a un sito malevolo. Oltre al phishing, i redirect lato client possono spesso essere concatenati per esfiltrare dati sensibili come token di accesso OAuth o identificativi di sessione, qualora la redirezione avvenga mentre tali valori sono presenti nell'URL o nell'header `Referer`. Nelle moderne Single Page Applications (SPA), questa vulnerabilità appare frequentemente nella logica di routing, nei parametri "return to" dopo l'autenticazione o nelle funzionalità di cambio lingua.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media* |

> *La gravità diventa Alta se la redirezione può essere utilizzata per esfiltrare token OAuth, ID di sessione o bypassare controlli di sicurezza in un flusso di autenticazione.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Strumenti:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### L'applicazione utilizza script lato client per gestire le redirezioni basate sull'input dell'utente?
- [ ] No — la redirezione è gestita esclusivamente lato server tramite codici di stato HTTP `3xx`  
- [ ] Sì — l'applicazione utilizza sink JavaScript come `window.location` per la navigazione basata su parametri URL  
- [ ] Sì — l'applicazione utilizza un router del framework lato client (es. React Router, Vue Router) che elabora percorsi forniti dall'utente  

### Sono implementati controlli di validazione dell'input o whitelist per l'URL di destinazione?
- [ ] Sì — viene applicata una **whitelist** rigorosa di domini/percorsi consentiti e il bypass **non è possibile**  
- [ ] Sì — la validazione è presente ma si basa su regex deboli o blacklist, e il bypass **è possibile**  
- [ ] No — l'applicazione accetta qualsiasi stringa arbitraria come destinazione della redirezione  

### La logica di redirezione può essere bypassata utilizzando comuni tecniche di offuscamento?
- [ ] No — i controlli gestiscono correttamente i caratteri codificati, gli URL relativi al protocollo (`//`) e i backslash  
- [ ] Sì — il bypass **è possibile** utilizzando URL encoding o double-encoding  
- [ ] Sì — il bypass **è possibile** utilizzando URL relativi al protocollo o caratteri `@` (es. `https://expected.com@attacker.com`)  
- [ ] Sì — il bypass **è possibile** sfruttando specifiche peculiarità (quirks) del browser o null byte injection  

### Il processo di redirezione espone dati sensibili al sito di destinazione?
- [ ] No — nessun dato sensibile è presente nell'URL o trasmesso tramite header durante la redirezione  
- [ ] Sì — dati sensibili (es. token di sessione, chiavi API) **vengono esposti** tramite l'header `Referer` al sito esterno  
- [ ] Sì — i dati sensibili presenti nel frammento dell'URL (`#`) **sono accessibili** agli script del sito di destinazione  

### È possibile eseguire JavaScript tramite il sink di redirezione (DOM-based XSS)?
- [ ] No — il sink consente solo la navigazione verso i protocolli `http` o `https`  
- [ ] Sì — il sink di redirezione consente URI `javascript:` o `data:`, rendendo possibile il **DOM-based XSS**

---

## WSTG-CLNT-05 — Test per CSS Injection

La CSS Injection si verifica quando un'applicazione consente all'input fornito dall'utente di influenzare i Cascading Style Sheets (CSS) di una pagina web senza un'adeguata sanitizzazione o escaping. Sebbene sia spesso percepita come un problema estetico, gli attaccanti possono sfruttare la CSS Injection per esfiltrare dati sensibili, come CSRF token o Session ID, utilizzando selettori di attributi (attribute selectors) e proprietà background-image per innescare richieste esterne. Questa vulnerabilità si verifica tipicamente negli endpoint in cui le preferenze dell'utente (ad esempio, temi personalizzati, colori dei caratteri) vengono riflesse all'interno di blocchi `<style>` o attributi `style` inline. Dal punto di vista di un attaccante, lo sfruttamento con successo può portare a UI redressing, phishing tramite la modifica non autorizzata del layout o l'esfiltrazione furtiva di dati in ambienti in cui policy restrittive di Content Security Policy potrebbero altrimenti bloccare attacchi basati su JavaScript.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se il punto di iniezione consente l'esfiltrazione di attributi sensibili come i CSRF token o se può essere utilizzato per l'UI redressing in flussi di lavoro sensibili.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### L'applicazione riflette l'input controllato dall'utente all'interno di contesti CSS?
- [ ] No — l'input **non** viene riflesso in tag style, attributi o file CSS  
- [ ] Sì — l'input viene riflesso ma è strettamente limitato a valori alfanumerici sicuri  
- [ ] Sì — l'input viene riflesso all'interno di un attributo `style` o di un blocco `<style>`  

### I metacaratteri e le funzioni specifiche del CSS sono correttamente sanitizzati?
- [ ] Sì — i caratteri come `{`, `}`, `:` e le funzioni come `url()` sono **correttamente sottoposti a escaping**  
- [ ] Sì — la sanitizzazione è presente ma il bypass **è possibile** tramite encoding o commenti CSS  
- [ ] No — nessuna sanitizzazione è applicata ai metacaratteri CSS  

### È possibile l'esfiltrazione di dati utilizzando i selettori CSS?
- [ ] No — i selettori **non possono** innescare richieste esterne o causare leak di dati  
- [ ] Sì — l'esfiltrazione parziale dei dati **è possibile** tramite selettori di attributi e `background-image`  
- [ ] Sì — l'esfiltrazione completa di token sensibili **è possibile** utilizzando tecniche di Brute Force automatizzate  

### Una Content Security Policy (CSP) mitiga il rischio di CSS Injection?
- [ ] Sì — `style-src` è limitato a 'self' e `img-src` o `connect-src` **sono limitati**  
- [ ] Sì — la CSP è presente ma utilizza 'unsafe-inline' o consente domini esterni con wildcard  
- [ ] No — nessuna CSP è implementata per impedire il caricamento di risorse esterne tramite CSS  

### La vulnerabilità può essere utilizzata per eseguire UI redressing o phishing?
- [ ] No — l'ambito dell'iniezione è troppo limitato per modificare il layout in modo significativo  
- [ ] Sì — la modifica della UI **è possibile**, consentendo la sovrapposizione di elementi malevoli  
- [ ] Sì — il controllo completo del layout della pagina **è possibile**, facilitando attacchi di phishing altamente convincenti

---

## WSTG-CLNT-06 — Testing for Client-Side Resource Manipulation

La manipolazione delle risorse lato client si verifica quando un'applicazione permette all'input controllato dall'utente di influenzare la sorgente o il percorso delle risorse caricate dal browser, come script, fogli di stile o iframe. Manipolando frammenti di URI, parametri di query o altre sorgenti DOM, un attaccante può indurre l'applicazione a caricare contenuti esterni malevoli o a eseguire codice non autorizzato. Questa vulnerabilità si riscontra tipicamente nella logica JavaScript che popola dinamicamente gli attributi `src` o `href` degli elementi HTML senza una validazione sufficiente rispetto a una allow-list fidata. Dal punto di vista di un attaccante, questa vulnerabilità viene sfruttata creando un URL che reindirizza il caricamento delle risorse verso un server controllato dall'attaccante, risultando potenzialmente in DOM-based Cross-Site Scripting (XSS), esfiltrazione di dati sensibili o UI redressing.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Strumenti:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### L'applicazione utilizza sink lato client per caricare o incorporare dinamicamente le risorse?
- [ ] No — le risorse vengono caricate solo tramite HTML statico o logica lato server  
- [ ] Sì — il JavaScript lato client popola dinamicamente gli attributi delle risorse come `src`, `href` o `data`  

### Sono implementati controlli di validazione o allow-list per le sorgenti URI delle risorse?
- [ ] Sì — allow-list rigorose e validazione dell'origine **vengono applicate** a tutte le risorse dinamiche  
- [ ] Sì — la validazione è presente ma si basa su blacklist deboli o regex facilmente bypassabili  
- [ ] No — non viene applicato alcun controllo di validazione ai percorsi delle risorse controllati dall'utente  

### È possibile bypassare la validazione dell'URI utilizzando protocolli alternativi o codifica?
- [ ] No — il bypass dei filtri delle risorse **non è possibile**  
- [ ] Sì — il bypass **è possibile** utilizzando protocolli diversi come `data:`, `vbscript:` o `javascript:`  
- [ ] Sì — il bypass **è possibile** utilizzando caratteri codificati o null byte per aggirare semplici confronti di stringhe  

### Un attaccante può reindirizzare il caricamento delle risorse verso un dominio esterno non fidato?
- [ ] No — i percorsi delle risorse sono limitati alla stessa origine o a una sottodirectory fissa  
- [ ] Sì — l'applicazione **può** essere forzata a caricare script o stili da un dominio esterno arbitrario  

### Qual è l'impatto massimo della manipolazione delle risorse?
- [ ] Basso — la manipolazione influisce solo su elementi non eseguibili come immagini o testo non sensibile  
- [ ] Medio — la manipolazione consente il CSS injection o un significativo UI redressing  
- [ ] Alto — la manipolazione porta a DOM-based XSS o all'esecuzione di JavaScript arbitrario *(Critico)*

---

## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Il Cross-Origin Resource Sharing (CORS) è un meccanismo basato sul browser che permette a un server di consentire esplicitamente l'accesso alle proprie risorse da origini differenti, allentando efficacemente la Same-Origin Policy (SOP). Le configurazioni errate si verificano quando un'applicazione si fida eccessivamente di origini arbitrarie, spesso riflettendo l'header `Origin` nell'header di risposta `Access-Control-Allow-Origin` o utilizzando whitelist basate su espressioni regolari (regex) implementate in modo approssimativo. Dal punto di vista di un attaccante, una policy CORS permissiva può essere sfruttata ospitando uno script malevolo su un dominio controllato che effettua richieste autenticate all'applicazione vulnerabile, consentendo all'attaccante di esfiltrare dati sensibili, come token CSRF o PII, direttamente dal corpo della risposta. Questa vulnerabilità è estremamente critica sugli endpoint API che elaborano sessioni autenticate e restituiscono informazioni non pubbliche.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Medio / Alto |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Strumenti:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### L'applicazione implementa gli header CORS sugli endpoint autenticati?
- [ ] No — il CORS **non è abilitato**; il browser utilizza per default la Same-Origin Policy restrittiva  
- [ ] Sì — gli header CORS sono presenti e limitati a una **whitelist** rigorosa  
- [ ] Sì — gli header CORS sono presenti ma configurati in modo **permissivo**  

### In che modo il server gestisce l'header `Access-Control-Allow-Origin` (ACAO)?
- [ ] ACAO è impostato su un dominio statico specifico e **fidato**  
- [ ] ACAO è impostato su `*` (wildcard) e le credenziali **non possono** essere inviate  
- [ ] ACAO è impostato su `*` (wildcard) e le credenziali **possono** essere inviate *(Nota: la maggior parte dei browser blocca questa configurazione)*  
- [ ] ACAO **riflette dinamicamente** il valore dell'header `Origin` dalla richiesta  

### L'header `Access-Control-Allow-Credentials` (ACAC) è impostato su `true`?
- [ ] No — ACAC **non è presente** o è impostato su `false`  
- [ ] Sì — ACAC è impostato su `true`, ma ACAO è **limitato** a origini fidate  
- [ ] Sì — ACAC è impostato su `true` e ACAO **riflette** origini arbitrarie *(Critico)*  

### La whitelist delle origini può essere bypassata tramite manipolazione di sottodomini o dell'origine null?
- [ ] No — la whitelist è applicata rigorosamente e **non può** essere bypassata  
- [ ] Sì — la whitelist accetta **qualsiasi** sottodominio del target (es. `attacker.target.com`)  
- [ ] Sì — la whitelist accetta l'origine `null` tramite iframe in sandbox  
- [ ] Sì — la whitelist è vulnerabile a bypass di tipo regex (es. `target.com.attacker.com` o `target.com-safe.com`)  

### La configurazione CORS consente l'esfiltrazione di dati sensibili?
- [ ] No — gli endpoint esposti **non** contengono dati sensibili o specifici della sessione  
- [ ] Sì — gli endpoint autenticati restituiscono PII, credenziali o token CSRF che **possono** essere letti da un'origine non autorizzata

---

## WSTG-CLNT-08 — Testing for Cross Site Flashing

Il Cross-Site Flashing (XSF) è una vulnerabilità client-side che si verifica quando un file Flash (SWF) gestisce in modo improprio l'input controllato dall'utente, consentendo a un attaccante di eseguire codice ActionScript malevolo o di interfacciarsi con l'ambiente JavaScript del browser. Manipolando parametri come `FlashVars` o stringhe di query URL passate a sink come `ExternalInterface.call`, `getURL` o `loadMovie`, un attaccante può eseguire azioni nel contesto del dominio vulnerabile, inclusi il session hijacking e l'esfiltrazione di dati. Questa vulnerabilità si riscontra principalmente in applicazioni enterprise legacy che ospitano ancora file SWF, dove l'insufficiente validazione dell'input consente al filmato Flash di essere riutilizzato come vettore per Cross-Site Scripting (XSS) o per bypassare la Same-Origin Policy (SOP) tramite configurazioni cross-domain permissive.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Sull'applicazione sono ospitati file Adobe Flash (.swf) legacy?
- [ ] No — non sono ospitati o referenziati file Flash all'interno del perimetro dell'applicazione  
- [ ] Sì — i file SWF sono presenti ma servono solo contenuti statici  
- [ ] Sì — i file SWF sono presenti e accettano parametri dinamici tramite `FlashVars` o stringhe URL  

### L'input passato ai sink ActionScript viene sanificato?
- [ ] Sì — tutti gli input sono rigorosamente validati e i sink ActionScript **non possono** essere manipolati  
- [ ] Sì — la validazione viene applicata ma il bypass **è possibile** tramite specifiche tecniche di codifica  
- [ ] No — l'input viene passato direttamente a sink sensibili come `ExternalInterface.call` o `getURL` *(Critico)*  

### Il file Flash può essere utilizzato per eseguire JavaScript arbitrario (XSS)?
- [ ] No — `ExternalInterface` è **disabilitato** o il parametro `allowScriptAccess` è impostato su `never`  
- [ ] Sì — `allowScriptAccess` è impostato su `sameDomain` ma il file SWF è ospitato sul dominio target  
- [ ] Sì — `allowScriptAccess` è impostato su `always`, consentendo XSS da qualsiasi dominio  

### La policy `crossdomain.xml` impedisce richieste cross-origin non autorizzate?
- [ ] Sì — `crossdomain.xml` è restrittivo e consente solo origini specifiche e fidate  
- [ ] No — il file `crossdomain.xml` **è mancante**  
- [ ] Sì — la policy è eccessivamente permissiva (es. `<allow-access-from domain="*" />`)  

### È possibile forzare il file SWF a caricare filmati esterni controllati da un attaccante?
- [ ] No — l'applicazione **non può** essere forzata a caricare file SWF esterni  
- [ ] Sì — le funzioni `loadMovie` o `loadMovieNum` accettano URL esterni non validati

---

## WSTG-CLNT-09 — Testing for Clickjacking

Il Clickjacking, noto anche come UI redressing, si verifica quando un utente malintenzionato utilizza livelli trasparenti o opachi per indurre un utente a fare clic su un pulsante o un collegamento su un'altra pagina quando questi intendeva fare clic sulla pagina di livello superiore. Questa vulnerabilità compromette l'integrità delle azioni dell'utente, portando potenzialmente a modifiche non autorizzate dei dati, cambiamenti dell'account o trasferimenti finanziari eseguiti per conto della vittima. Gli attaccanti tipicamente sfruttano questa vulnerabilità incorporando il sito web di destinazione all'interno di un `<iframe>` su un sito malevolo controllato e sovrapponendovi contenuti ingannevoli o esche allettanti. Si verifica più frequentemente su pagine che eseguono azioni sensibili che modificano lo stato, come i pulsanti "Elimina account", i moduli di reimpostazione della password o i pannelli di configurazione amministrativa.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Media / Alta* |

> *La severità diventa Alta se azioni sensibili (ad esempio, trasferimenti di fondi, eliminazione dell'account o escalation dei privilegi) possono essere attivate tramite clickjacking.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Strumenti:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### L'applicazione implementa header lato server per impedire il framing non autorizzato?
- [ ] Sì — `X-Frame-Options` o `Content-Security-Policy` **vengono applicati** e impediscono il framing *(Più sicuro)*  
- [ ] Sì — gli header sono presenti ma **configurati in modo errato** (ad esempio, `X-Frame-Options: ALLOW-FROM` che manca di un ampio supporto da parte dei browser)  
- [ ] No — non **viene applicato** alcun header di protezione contro il framing  

### La direttiva `frame-ancestors` della `Content-Security-Policy` (CSP) è implementata?
- [ ] Sì — `frame-ancestors 'none'` o `'self'` **viene applicato**  
- [ ] Sì — `frame-ancestors` è presente ma **consente** origini non attendibili o eccessivamente ampie  
- [ ] No — la direttiva è **mancante** o la CSP non è implementata  

### L'header `X-Frame-Options` (XFO) è configurato correttamente per il supporto dei browser legacy?
- [ ] Sì — `DENY` o `SAMEORIGIN` **viene applicato**  
- [ ] Sì — XFO è presente ma **ignorato** dai browser moderni perché esiste una direttiva CSP `frame-ancestors`  
- [ ] No — l'header XFO è **mancante** o impostato su un valore non sicuro  

### Le protezioni contro il framing possono essere bypassate utilizzando tecniche note?
- [ ] No — il bypass **non è possibile** tramite tecniche standard (double-framing, ecc.)  
- [ ] Sì — la protezione **può** essere bypassata a causa di regex o logica debole nella whitelist di `frame-ancestors`  
- [ ] Sì — la protezione **può** essere bypassata perché l'applicazione si affida esclusivamente al frame-busting basato su JavaScript  

### Un'azione sensibile è sfruttabile tramite un proof-of-concept di clickjacking?
- [ ] No — il framing è bloccato o non sono raggiungibili azioni sensibili  
- [ ] Sì — l'UI redressing **è possibile** ma richiede un'interazione complessa da parte dell'utente  
- [ ] Sì — l'UI redressing **è possibile** e consente azioni immediate che modificano lo stato *(Critico)*

---

## WSTG-CLNT-10 — Testing WebSockets

Il testing dei WebSocket si concentra sul canale di comunicazione full-duplex stabilito tra il client e il server, che elude i tradizionali pattern di richiesta-risposta HTTP. I pentester valutano la sicurezza del processo di handshake, la presenza di protezioni contro il Cross-Site WebSocket Hijacking (CSWSH) e il rigore della validazione dell'input applicata ai payload dei messaggi. L'exploitation in genere comporta l'hijacking di una sessione attiva per esfiltrare dati in tempo reale o l'iniezione di payload malevoli che innescano vulnerabilità server-side come Command Injection o SQLi attraverso la socket persistente. Poiché molti Web Application Firewall (WAF) tradizionali non ispezionano efficacemente il traffico WebSocket, questo protocollo fornisce spesso un vettore furtivo per bypassare le difese perimetrali.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Strumenti:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### La connessione WebSocket è stabilita su un canale sicuro?
- [ ] Sì — `wss://` (WebSocket Secure) è forzato per tutte le connessioni  
- [ ] No — viene utilizzato `ws://`, consentendo l'intercettazione tramite person-in-the-middle (PITM)  

### L'applicazione è protetta contro il Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] No — il server convalida rigorosamente l'header `Origin` durante l'handshake  
- [ ] Sì — la validazione dell'header `Origin` è debole o **può** essere bypassata tramite origin null o contraffatte (spoofed)  
- [ ] Sì — non viene eseguita alcuna validazione dell'header `Origin`, consentendo ad attaccanti cross-site di avviare una connessione *(Critico)*  

### La validazione e la sanificazione dell'input vengono applicate ai payload dei messaggi WebSocket?
- [ ] Sì — tutti i messaggi in entrata sono sanificati e validati rispetto a uno schema  
- [ ] Sì — esiste una parziale validazione, ma il bypass **è possibile** utilizzando payload codificati o non standard  
- [ ] No — i messaggi vengono elaborati direttamente, consentendo injection (XSS, SQLi, RCE) o manipolazione della logica  

### Vengono eseguiti controlli di autenticazione e autorizzazione su ogni messaggio WebSocket?
- [ ] Sì — la sessione viene verificata per ogni messaggio o il protocollo è stateless con token  
- [ ] Sì — l'autenticazione avviene durante l'handshake, ma l'autorizzazione per azioni specifiche **non** è applicata per singolo messaggio  
- [ ] No — l'autenticazione viene controllata solo all'handshake, consentendo al session hijacking di garantire l'accesso completo  

### Il server implementa rate limiting o restrizioni sulle risorse per il WebSocket?
- [ ] Sì — il rate limiting e le dimensioni massime dei messaggi sono **abilitati** e applicati  
- [ ] No — non esistono limiti, consentendo il Denial of Service (DoS) tramite flooding di messaggi o payload di grandi dimensioni

---

## WSTG-CLNT-11 — Test Web Messaging

Il Web Messaging, o `postMessage`, consente la comunicazione cross-origin tra oggetti window, come una pagina genitore e un popup generato o un iframe incorporato. Questo meccanismo è fondamentale per le moderne funzionalità web, ma introduce rischi significativi se l'applicazione ricevente non riesce a verificare l'identità del mittente o gestisce in modo improprio il payload del messaggio. Gli aggressori sfruttano queste vulnerabilità ospitando un sito dannoso che incorpora l'applicazione target e invia messaggi appositamente creati per innescare azioni indesiderate, esfiltrare dati sensibili o eseguire Cross-Site Scripting (XSS) basato su DOM. Uno sfruttamento (Exploit) riuscito si verifica quando i listener dei messaggi non convalidano rigorosamente la proprietà `origin` o passano `message.data` direttamente in sink di esecuzione pericolosi.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Medio / Alto |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Strumenti:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### L'applicazione utilizza l'API `postMessage` per la comunicazione tra documenti?
- [ ] No — i listener `postMessage` **non** sono presenti nel codice lato client  
- [ ] Sì — `postMessage` è utilizzato per la comunicazione tra componenti interni  
- [ ] Sì — `postMessage` è utilizzato per comunicare con domini o widget di terze parti  

### L'origine dei messaggi in arrivo è validata rigorosamente?
- [ ] Sì — l'origine viene verificata rispetto a una whitelist rigorosa utilizzando `===` o un confronto identico *(Più sicuro)*  
- [ ] Sì — l'origine è validata tramite una regex, ma la logica **è** suscettibile di bypass (es. `^https://trusted.com`)  
- [ ] No — la proprietà `origin` **non** viene controllata, consentendo a qualsiasi dominio di inviare messaggi  
- [ ] No — viene utilizzato un wildcard `*` nel `targetOrigin` del mittente, rischiando potenzialmente la fuga di dati verso listener dannosi  

### Il payload del messaggio viene sanificato prima di essere elaborato dall'applicazione?
- [ ] Sì — i dati sono trattati come testo semplice e non vengono utilizzati sink pericolosi  
- [ ] Sì — i dati vengono analizzati (es. `JSON.parse`) e validati prima dell'uso, e il bypass **non è possibile**  
- [ ] No — i dati del messaggio vengono passati direttamente in sink pericolosi come `innerHTML`, `eval()` o `setTimeout()`  
- [ ] No — i dati del messaggio vengono utilizzati per navigare nella finestra o modificare `location.href` senza convalida  

### Un attaccante può innescare azioni non autorizzate o esfiltrare dati tramite un messaggio dannoso?
- [ ] No — anche con un messaggio contraffatto, non è possibile accedere ad azioni o dati sensibili  
- [ ] Sì — un messaggio appositamente creato **può** innescare modifiche di stato sensibili (es. cambio password, aggiornamento profilo)  
- [ ] Sì — un messaggio appositamente creato **può** causare XSS basato su DOM o l'esfiltrazione di token CSRF/dati di sessione

---

## WSTG-CLNT-12 — Test del Browser Storage

Il test del browser storage consiste nell'analizzare come un'applicazione utilizzi meccanismi lato client come LocalStorage, SessionStorage, IndexedDB e WebSQL per la persistenza dei dati. Le informazioni sensibili memorizzate in modo non sicuro — inclusi PII, token di autenticazione o identificativi di sessione — possono essere compromesse se un attaccante realizza un attacco di Cross-Site Scripting (XSS) o ottiene l'accesso fisico al dispositivo dell'utente. I pentester devono determinare se i dati sono memorizzati in chiaro (cleartext), se rimangono accessibili dopo il logout e se il ciclo di vita dell'archiviazione è gestito in modo appropriato per prevenire il data leakage. Dal punto di vista di un attaccante, questi meccanismi di archiviazione sono obiettivi lucrativi per l'esfiltrazione di informazioni di stato che consentono il session hijacking o la persistenza dell'account a lungo termine.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se i token di sessione o PII sensibili sono memorizzati nel LocalStorage e risultano accessibili tramite XSS.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### L'applicazione memorizza informazioni sensibili nel browser storage?
- [ ] No — l'applicazione **non** utilizza il browser storage o memorizza solo stati dell'interfaccia utente (UI) non sensibili  
- [ ] Sì — i dati sensibili sono memorizzati ma **sono cifrati** utilizzando una crittografia lato client robusta  
- [ ] Sì — dati sensibili (token, PII, segreti) sono memorizzati in **chiaro** (cleartext) *(Media)*  

### I dati memorizzati sono accessibili a script non autorizzati (rischio XSS)?
- [ ] No — i dati sono memorizzati in cookie HttpOnly (non nel browser storage) o lo storage **non è utilizzato**  
- [ ] Sì — viene utilizzato LocalStorage/SessionStorage, rendendo i dati **completamente accessibili** tramite qualsiasi payload XSS *(Alta)*  

### Il browser storage viene pulito correttamente al logout dell'utente?
- [ ] Sì — tutto lo storage relativo all'applicazione viene **esplicitamente pulito** durante il processo di logout  
- [ ] No — lo storage persiste dopo il logout, ma **non** contiene dati di sessione sensibili  
- [ ] No — i token di autenticazione o i PII **persistono** nello storage dopo il termine della sessione *(Media)*  

### L'applicazione utilizza IndexedDB o WebSQL per dataset grandi o sensibili?
- [ ] No — questi meccanismi di archiviazione **non sono utilizzati**  
- [ ] Sì — i dati sono memorizzati e **correttamente sanitizzati** prima di essere renderizzati nel DOM  
- [ ] Sì — dataset sensibili sono memorizzati in **chiaro** (cleartext) all'interno delle strutture IndexedDB/WebSQL  

### L'integrità dei dati memorizzati può essere manipolata per influenzare la logica dell'applicazione?
- [ ] No — l'applicazione convalida o firma i dati prima di elaborarli dallo storage  
- [ ] Sì — la modifica dei valori nello storage (es. ruoli utente, flag) **è possibile** e comporta un'alterazione del comportamento dell'applicazione

---

## WSTG-CLNT-13 — Testing for Cross Site Script Inclusion

L'inclusione di script tra siti (Cross-Site Script Inclusion - XSSI) si verifica quando un'applicazione web esporta dati sensibili all'interno di un file JavaScript generato dinamicamente o di una risposta JSONP, che un sito controllato da un attaccante può successivamente includere tramite un tag `<script>`. Poiché l'inclusione di script bypassa la Same-Origin Policy (SOP), un attaccante può eseguire lo script nel proprio contesto e sovrascrivere oggetti globali o variabili per esfiltrare le informazioni sensibili. Questa vulnerabilità si manifesta tipicamente in endpoint autenticati che restituiscono dati specifici dell'utente come indirizzi email, API key o metadati di sessione in formato script. Dal punto di vista di un attaccante, l'exploitation prevede la creazione di una pagina malevola che faccia riferimento allo script vulnerabile e catturi le modifiche alle variabili globali o le chiamate a funzioni risultanti.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Media / Alta |

> *La severità diventa Alta se i dati trapelati includono CSRF tokens, identificatori di sessione o PII altamente sensibili.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Esistono endpoint autenticati che restituiscono JavaScript dinamico o JSONP contenente informazioni sensibili?
- [ ] No — tutti gli script sono statici e **non possono** contenere dati specifici dell'utente  
- [ ] Sì — esistono script dinamici ma **non contengono** informazioni sensibili  
- [ ] Sì — gli script dinamici contengono PII o token e **sono** accessibili tra origini diverse (cross-origin)  

### L'header `X-Content-Type-Options: nosniff` è implementato nelle risposte degli script sensibili?
- [ ] Sì — l'header è **applicato** e impedisce il MIME-type sniffing  
- [ ] No — l'header è **mancante**, consentendo potenzialmente l'esecuzione di contenuti non script come script  

### Gli script sensibili sono protetti da meccanismi anti-XSSI come prefissi non eseguibili o header personalizzati?
- [ ] Sì — gli script richiedono un header personalizzato o un token che **non può** essere inviato tramite un tag `<script>` standard  
- [ ] Sì — gli script utilizzano prefissi non eseguibili (es. `)]}'`) che **non possono** essere facilmente aggirati  
- [ ] No — gli script sono direttamente accessibili tramite richieste GET utilizzando credenziali ambientali *(Critico)*  

### L'esfiltrazione di dati sensibili è possibile sovrascrivendo variabili globali o prototipi?
- [ ] No — i dati sensibili hanno uno scope locale o sono protetti tramite funzionalità moderne dei browser  
- [ ] Sì — i dati sensibili sono assegnati a variabili globali e **possono** essere catturati  
- [ ] Sì — i dati trapelano attraverso callback JSONP che **possono** essere intercettate

---

## WSTG-CLNT-14 — Testing for Reverse Tabnabbing

Il Reverse Tabnabbing è una vulnerabilità client-side in cui una pagina aperta tramite `target="_blank"` mantiene un riferimento funzionale alla pagina genitore attraverso l'oggetto `window.opener`. Questo permette a un sito di destinazione controllato da un attaccante di reindirizzare programmaticamente la scheda originale e fidata verso un URL malevolo, tipicamente una pagina di phishing progettata per sottrarre credenziali. L'attacco sfrutta la fiducia intrinseca dell'utente nella scheda originale, poiché è improbabile che l'utente noti che la pagina precedentemente visitata ha navigato verso un dominio differente. Le moderne pratiche di sicurezza richiedono l'uso degli attributi `rel="noopener"` o `rel="noreferrer"` nei tag anchor, oppure l'impostazione della proprietà `opener` a null in JavaScript, per prevenire questa comunicazione cross-window.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media* |

> *La gravità è Bassa nella maggior parte dei browser moderni, poiché ora impostano di default `target="_blank"` come `rel="noopener"`. La gravità diventa Media se l'applicazione è destinata a browser legacy o utilizza `window.open()` senza impostare la proprietà `opener` a null.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Sono presenti collegamenti che si aprono in una nuova finestra o scheda?
- [ ] No — nessun link utilizza `target="_blank"` o reindirizzamenti equivalenti basati su JavaScript  
- [ ] Sì — `target="_blank"` è utilizzato esclusivamente su link interni e fidati  
- [ ] Sì — `target="_blank"` è utilizzato su link esterni o URL forniti dall'utente  

### La relazione `window.opener` è limitata sui tag anchor?
- [ ] Sì — `rel="noopener"` o `rel="noreferrer"` è **applicato coerentemente** a tutti i link con `target="_blank"` *(Massima sicurezza)*  
- [ ] Sì — gli attributi sono applicati ma **mancano** su link ad alto rischio o generati dall'utente  
- [ ] No — gli attributi **non sono applicati**, permettendo alla pagina figlia di accedere a `window.opener`  

### L'applicazione è vulnerabile al Reverse Tabnabbing tramite la funzione JavaScript `window.open()`?
- [ ] No — `window.open()` non viene utilizzato o imposta esplicitamente la proprietà `opener` a null  
- [ ] Sì — `window.open()` viene utilizzato e il riferimento `opener` **rimane accessibile** alla finestra figlia  

### Un attaccante può controllare la destinazione di un link che si apre in una nuova scheda?
- [ ] No — tutte le destinazioni dei link sono hardcoded e puntano a domini fidati  
- [ ] Sì — le destinazioni dei link sono fornite dall'utente (es. profili social media, link in biografia) e **non** sono bonificate per le protezioni contro il tabnabbing

---

## WSTG-APIT-01 — API Reconnaissance

La ricognizione API è il processo sistematico di identificazione e mappatura della superficie di attacco delle API per scoprire endpoint, metodi supportati e strutture dati sottostanti. Gli attaccanti eseguono questa fase per scoprire API non documentate (shadow API), versioni deprecate con vulnerabilità legacy e file di documentazione esposti, come definizioni Swagger o OpenAPI, che rivelano la logica di business interna. Analizzando pattern URL prevedibili, file JavaScript lato client e risultati di brute-force di directory, un avversario può costruire una mappa completa dell'API per identificare obiettivi per attacchi di Broken Object Level Authorization (BOLA) o Mass Assignment. Questa ricognizione tipicamente prende di mira percorsi comuni come `/api/v1/`, `/swagger.json` o `/graphql` per ottenere una roadmap delle funzionalità backend dell'applicazione.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Informativa / Bassa |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Strumenti:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### I file di documentazione API (Swagger, OpenAPI, WSDL) sono accessibili pubblicamente?
- [ ] No — La documentazione API non è accessibile o è limitata da autenticazione  
- [ ] Sì — La documentazione è accessibile ma richiede credenziali valide  
- [ ] Sì — La documentazione API (es. `/swagger-ui.html`) è **accessibile pubblicamente** senza autenticazione  

### È possibile scoprire endpoint API non documentati o "shadow" tramite fuzzing?
- [ ] No — Il fuzzing di directory ed endpoint non ha prodotto percorsi non documentati  
- [ ] Sì — Sono stati trovati endpoint non documentati, ma **non** è possibile accedervi senza autorizzazione  
- [ ] Sì — Sono stati trovati endpoint non documentati e **possono** essere consultati senza una valida autorizzazione  

### L'applicazione rivela versioni multiple dell'API (es. /v1/, /v2/, /beta/)?
- [ ] No — È accessibile solo la versione API corrente e protetta  
- [ ] Sì — Esistono versioni legacy, ma implementano i **medesimi** controlli di sicurezza della versione corrente  
- [ ] Sì — Le versioni legacy (es. `/v1/`) sono accessibili e **mancano** dei controlli di sicurezza delle versioni più recenti  

### Le risorse lato client (JavaScript/App Mobile) espongono strutture di endpoint API o chiavi?
- [ ] No — Il codice lato client **non** contiene percorsi API cablati (hardcoded) o chiavi sensibili  
- [ ] Sì — Il codice lato client contiene mappe degli endpoint API ma **nessuna** chiave sensibile  
- [ ] Sì — Chiavi API, segreti o URL di endpoint esclusivamente interni **sono** cablati (hardcoded) nelle risorse lato client  

### Sono individuabili parametri o header nascosti su endpoint noti?
- [ ] No — Il fuzzing dei parametri (es. tramite `Arjun`) **non** ha rivelato input nascosti  
- [ ] Sì — Sono stati scoperti parametri nascosti (es. `debug=true`, `admin=1`), ma **non** sono funzionali  
- [ ] Sì — Sono stati scoperti parametri nascosti che **possono** essere utilizzati per alterare il comportamento dell'applicazione

---

## WSTG-APIT-02 — API Broken Object Level Authorization

Il Broken Object Level Authorization (BOLA), noto anche come Insecure Direct Object Reference (IDOR), si verifica quando un'API non riesce a verificare se un utente disponga dei permessi appropriati per accedere o manipolare una specifica risorsa identificata da un ID. Gli attaccanti sfruttano questa vulnerabilità enumerando o indovinando sistematicamente gli identificatori — come ID numerici o UUID — nei percorsi delle richieste (request paths), nei parametri di query o nei corpi JSON per accedere a dati appartenenti ad altri utenti. Questa vulnerabilità è il problema più comune e impattante nella sicurezza delle API moderne, potendo portare a esfiltrazioni di dati di massa, modifiche non autorizzate o un completo account takeover. Dal punto di vista di un attaccante, l'obiettivo è identificare gli endpoint che accettano identificatori di oggetto e testare se la logica di autorizzazione lato server sia mancante o possa essere soggetta a bypass manipolando tali ID.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Strumenti:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Gli identificatori di oggetto sono prevedibili o enumerabili all'interno delle richieste API?
- [ ] No — gli identificatori sono lunghi, casuali e crittograficamente sicuri (es. UUIDv4)  
- [ ] Sì — gli identificatori sono interi sequenziali (es. `101`, `102`)  
- [ ] Sì — gli identificatori seguono un pattern prevedibile o derivano da informazioni pubbliche  

### L'API esegue la validazione lato server della proprietà dell'oggetto per ogni richiesta?
- [ ] Sì — i controlli di autorizzazione sono applicati a ogni richiesta e il bypass **non è possibile**  
- [ ] Sì — i controlli sono applicati ma il bypass **è possibile** tramite parameter pollution o method tunneling  
- [ ] No — l'applicazione si affida esclusivamente all'autenticazione dell'utente senza controllare la proprietà della risorsa specifica  

### È possibile accedere o modificare la risorsa di un altro utente cambiando l'identificatore?
- [ ] No — l'accesso non autorizzato alle risorse di altri utenti **non è possibile**  
- [ ] Sì — l'accesso in sola **lettura** non autorizzato (Horizontal BOLA) **è possibile**  
- [ ] Sì — la **modifica** o l'**eliminazione** non autorizzata (Horizontal BOLA) **è possibile**  

### L'API consente l'accesso a oggetti a livello amministrativo o di sistema sostituendo gli ID?
- [ ] No — le risorse amministrative sono protette da livelli di autorizzazione secondari  
- [ ] Sì — l'accesso o la modifica di risorse a livello di sistema (Vertical BOLA) **è possibile**  

### Il controllo di autorizzazione può essere aggirato spostando l'identificatore in una parte diversa della richiesta?
- [ ] No — la logica di autorizzazione è coerente indipendentemente dalla posizione del parametro  
- [ ] Sì — il bypass **è possibile** quando l'ID viene spostato dal percorso URL al corpo JSON o agli header  
- [ ] Sì — il bypass **è possibile** quando vengono forniti più ID e il server elabora quello non autorizzato

---

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