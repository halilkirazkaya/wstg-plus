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