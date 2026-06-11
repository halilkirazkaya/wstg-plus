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