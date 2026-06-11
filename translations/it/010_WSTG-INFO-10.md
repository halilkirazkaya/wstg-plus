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