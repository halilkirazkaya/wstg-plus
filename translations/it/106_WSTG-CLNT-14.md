## WSTG-CLNT-14 — Testing for Reverse Tabnabbing

Il Reverse Tabnabbing è una vulnerabilità client-side in cui una pagina aperta tramite `target="_blank"` mantiene un riferimento funzionale alla pagina genitore attraverso l'oggetto `window.opener`. Questo permette a un sito di destinazione controllato da un attaccante di reindirizzare programmaticamente la scheda originale e fidata verso un URL malevolo, tipicamente una pagina di phishing progettata per sottrarre credenziali. L'attacco sfrutta la fiducia intrinseca dell'utente nella scheda originale, poiché è improbabile che l'utente noti che la pagina precedentemente visitata ha navigato verso un dominio differente. Le moderne pratiche di sicurezza richiedono l'uso degli attributi `rel="noopener"` o `rel="noreferrer"` nei tag anchor, oppure l'impostazione della proprietà `opener` a null in JavaScript, per prevenire questa comunicazione cross-window.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media* |

> *La gravità è Bassa nella maggior parte dei browser moderni, poiché ora impostano di default `target="_blank"` come `rel="noopener"`. La gravità diventa Media se l'applicazione è destinata a browser legacy o utilizza `window.open()` senza impostare la proprietà `opener` a null.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Sono presenti collegamenti che si aprono in una nuova finestra o scheda?
- [ ] No — nessun link utilizza `target="_blank"` o reindirizzamenti equivalenti basati su JavaScript  
- [ ] Sì — `target="_blank"` è utilizzato esclusivamente su link interni e fidati  
- [ ] Sì — `target="_blank"` è utilizzato su link esterni o URL forniti dall'utente  

### La relazione `window.opener` è limitata sui tag anchor?
- [ ] Sì — `rel="noopener"` o `rel="noreferrer"` è **applicato coerentemente** a tutti i link con `target="_blank"` *(Massima sicurezza)*  
- [ ] Sì — gli attributi sono applicati ma **mancano** su link ad alto rischio o generati dall'utente  
- [ ] No — gli attributi **non sono applicati**, permettendo alla pagina figlia di accedere a `window.opener`  

### L'applicazione è vulnerabile al Reverse Tabnabbing tramite la funzione JavaScript `window.open()`?
- [ ] No — `window.open()` non viene utilizzato o imposta esplicitamente la proprietà `opener` a null  
- [ ] Sì — `window.open()` viene utilizzato e il riferimento `opener` **rimane accessibile** alla finestra figlia  

### Un attaccante può controllare la destinazione di un link che si apre in una nuova scheda?
- [ ] No — tutte le destinazioni dei link sono hardcoded e puntano a domini fidati  
- [ ] Sì — le destinazioni dei link sono fornite dall'utente (es. profili social media, link in biografia) e **non** sono bonificate per le protezioni contro il tabnabbing  

---