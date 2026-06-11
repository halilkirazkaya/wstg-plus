## WSTG-CLNT-06 — Testing for Client-Side Resource Manipulation

La manipolazione delle risorse lato client si verifica quando un'applicazione permette all'input controllato dall'utente di influenzare la sorgente o il percorso delle risorse caricate dal browser, come script, fogli di stile o iframe. Manipolando frammenti di URI, parametri di query o altre sorgenti DOM, un attaccante può indurre l'applicazione a caricare contenuti esterni malevoli o a eseguire codice non autorizzato. Questa vulnerabilità si riscontra tipicamente nella logica JavaScript che popola dinamicamente gli attributi `src` o `href` degli elementi HTML senza una validazione sufficiente rispetto a una allow-list fidata. Dal punto di vista di un attaccante, questa vulnerabilità viene sfruttata creando un URL che reindirizza il caricamento delle risorse verso un server controllato dall'attaccante, risultando potenzialmente in DOM-based Cross-Site Scripting (XSS), esfiltrazione di dati sensibili o UI redressing.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Strumenti:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### L'applicazione utilizza sink lato client per caricare o incorporare dinamicamente le risorse?
- [ ] No — le risorse vengono caricate solo tramite HTML statico o logica lato server  
- [ ] Sì — il JavaScript lato client popola dinamicamente gli attributi delle risorse come `src`, `href` o `data`  

### Sono implementati controlli di validazione o allow-list per le sorgenti URI delle risorse?
- [ ] Sì — allow-list rigorose e validazione dell'origine **vengono applicate** a tutte le risorse dinamiche  
- [ ] Sì — la validazione è presente ma si basa su blacklist deboli o regex facilmente bypassabili  
- [ ] No — non viene applicato alcun controllo di validazione ai percorsi delle risorse controllati dall'utente  

### È possibile bypassare la validazione dell'URI utilizzando protocolli alternativi o codifica?
- [ ] No — il bypass dei filtri delle risorse **non è possibile**  
- [ ] Sì — il bypass **è possibile** utilizzando protocolli diversi come `data:`, `vbscript:` o `javascript:`  
- [ ] Sì — il bypass **è possibile** utilizzando caratteri codificati o null byte per aggirare semplici confronti di stringhe  

### Un attaccante può reindirizzare il caricamento delle risorse verso un dominio esterno non fidato?
- [ ] No — i percorsi delle risorse sono limitati alla stessa origine o a una sottodirectory fissa  
- [ ] Sì — l'applicazione **può** essere forzata a caricare script o stili da un dominio esterno arbitrario  

### Qual è l'impatto massimo della manipolazione delle risorse?
- [ ] Basso — la manipolazione influisce solo su elementi non eseguibili come immagini o testo non sensibile  
- [ ] Medio — la manipolazione consente il CSS injection o un significativo UI redressing  
- [ ] Alto — la manipolazione porta a DOM-based XSS o all'esecuzione di JavaScript arbitrario *(Critico)*  

---