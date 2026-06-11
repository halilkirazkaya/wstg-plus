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