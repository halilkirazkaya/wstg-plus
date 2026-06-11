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