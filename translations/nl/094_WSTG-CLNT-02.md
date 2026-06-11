## WSTG-CLNT-02 — Testen op JavaScript-uitvoering

Het testen van JavaScript-uitvoering richt zich op het identificeren van instanties waarbij een applicatie onjuist omgaat met door de gebruiker verstrekte gegevens, wat leidt tot de uitvoering van ongeautoriseerde scripts binnen de browsercontext van het slachtoffer. Deze kwetsbaarheid resulteert doorgaans in Cross-Site Scripting (XSS), waarmee aanvallers sessiecookies kunnen exfiltreren, het Document Object Model (DOM) kunnen manipuleren of acties kunnen uitvoeren namens de gebruiker. Penetratietesters onderzoeken sinks zoals `innerHTML`, `document.write()` en `eval()` waar ongezuiverde gegevens uit bronnen zoals URL-fragmenten, `localStorage` of `window.name` kunnen worden verwerkt. Succesvolle exploitatie brengt de integriteit en vertrouwelijkheid van de sessie van de gebruiker in gevaar en kan dienen als een pivot voor complexere aanvallen aan de clientzijde.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Tools:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### Verwerkt de applicatie door de gebruiker verstrekte gegevens in gevaarlijke JavaScript-sinks?
- [ ] Nee — alle gegevens zijn gesaneerd of worden gebruikt in veilige sinks zoals `textContent`  
- [ ] Ja — gegevens worden verwerkt in gevaarlijke sinks, maar sanering/codering **wordt toegepast**  
- [ ] Ja — gegevens worden verwerkt in gevaarlijke sinks en sanering **wordt niet toegepast**  

### Is er een Content Security Policy (CSP) aanwezig om scriptuitvoering te beperken?
- [ ] Ja — een restrictieve CSP is **ingeschakeld** en bypass **is niet mogelijk**  
- [ ] Ja — een CSP is **ingeschakeld**, maar bypass **is mogelijk** via `unsafe-inline` of onveilige CDN's  
- [ ] Nee — er is geen CSP **ingeschakeld**  

### Kan JavaScript worden uitgevoerd via URL-parameters of fragmenten (DOM-based XSS)?
- [ ] Nee — invoer is correct gecodeerd en uitvoering **is niet mogelijk**  
- [ ] Ja — invoer wordt gereflecteerd in de DOM, maar uitvoering **wordt voorkomen** door beveiligingen van moderne frameworks  
- [ ] Ja — invoer wordt direct uitgevoerd in de browser en exploitatie **is mogelijk**  

### Zijn event handlers of URI-schema's kwetsbaar voor script-injectie?
- [ ] Nee — invoer is gesaneerd om event-attributen en `javascript:`-schema's te verwijderen  
- [ ] Ja — event handlers **kunnen** worden geïnjecteerd, maar filters beperken de complexiteit van de payload  
- [ ] Ja — willekeurige JavaScript-uitvoering via `onerror`, `onload` of `href`-attributen **is mogelijk**  

---