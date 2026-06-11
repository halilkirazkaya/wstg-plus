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