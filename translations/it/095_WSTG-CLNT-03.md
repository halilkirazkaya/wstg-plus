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