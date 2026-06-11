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