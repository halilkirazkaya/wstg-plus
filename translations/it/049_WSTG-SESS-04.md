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