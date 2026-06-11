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