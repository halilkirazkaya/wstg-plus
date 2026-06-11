## WSTG-ATHZ-05 — Verifica delle vulnerabilità OAuth

Le debolezze di OAuth comprendono una varietà di falle nell'implementazione dei protocolli OAuth 2.0 o OpenID Connect (OIDC), che spesso risultano in un account takeover completo o nell'accesso non autorizzato ai dati. Queste vulnerabilità si manifestano tipicamente durante il flusso di autorizzazione, in particolare per quanto riguarda la validazione del `redirect_uri`, l'entropia e la verifica del parametro `state`, e la gestione sicura dei token di accesso o ID. Gli attaccanti le sfruttano intercettando gli authorization code, reindirizzando i token verso domini controllati dall'attaccante tramite open redirect, o eseguendo Cross-Site Request Forgery (CSRF) per collegare la sessione di una vittima all'account dell'attaccante. Poiché OAuth funge spesso da meccanismo di autenticazione primario, una singola configurazione errata può compromettere l'integrità dell'intero sistema di identità degli utenti.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Strumenti:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Il parametro `state` è implementato e validato per prevenire attacchi CSRF?
- [ ] Sì — `state` è obbligatorio, univoco per sessione e validato rigorosamente sul server  
- [ ] Sì — `state` è presente ma è statico o prevedibile tra diverse sessioni  
- [ ] Sì — `state` è presente ma l'applicazione **non** lo valida al momento del callback  
- [ ] No — il parametro `state` **non** viene utilizzato nella richiesta di autorizzazione *(Critico)*  

### Il `redirect_uri` è validato rigorosamente rispetto a una whitelist?
- [ ] Sì — vengono **accettate** solo corrispondenze esatte rispetto a una whitelist pre-registrata  
- [ ] Sì — la validazione è applicata ma il bypass **è possibile** tramite path traversal o manipolazione del sottodominio  
- [ ] Sì — la validazione è applicata ma il bypass **è possibile** tramite parameter pollution o fragment injection  
- [ ] No — l'applicazione **consente** URL arbitrari nel parametro `redirect_uri` *(Critico)*  

### Il `response_type` può essere manipolato per alterare il flusso?
- [ ] No — l'applicazione accetta solo il flusso previsto (es. `code`) e rifiuta gli altri  
- [ ] Sì — il passaggio da `code` a `token` (Implicit flow) **è possibile** e causa la perdita di token nell'URL  
- [ ] Sì — i flussi ibridi o valori di `response_type` non autorizzati sono **abilitati** ed elaborati  

### I token di accesso o gli authorization code vengono esposti tramite gli header Referer o la cronologia del browser?
- [ ] No — i token/code sono gestiti in modo sicuro e le pagine sensibili utilizzano una `Referrer-Policy` appropriata  
- [ ] Sì — i token/code **vengono** esposti a domini di terze parti tramite l'header `Referer`  
- [ ] Sì — i token/code **sono** visibili nella cronologia del browser a causa di caching non sicuro o richieste GET  

### L'applicazione applica il principio del "Minimo Privilegio" per gli scope OAuth?
- [ ] Sì — l'applicazione richiede solo gli scope minimi necessari per la funzionalità  
- [ ] No — l'applicazione richiede scope eccessivi o amministrativi per impostazione predefinita  
- [ ] No — l'utente **non può** revisionare o escludere scope specifici durante la fase di consenso  

---