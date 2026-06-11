## WSTG-CLNT-05 — Test per CSS Injection

La CSS Injection si verifica quando un'applicazione consente all'input fornito dall'utente di influenzare i Cascading Style Sheets (CSS) di una pagina web senza un'adeguata sanitizzazione o escaping. Sebbene sia spesso percepita come un problema estetico, gli attaccanti possono sfruttare la CSS Injection per esfiltrare dati sensibili, come CSRF token o Session ID, utilizzando selettori di attributi (attribute selectors) e proprietà background-image per innescare richieste esterne. Questa vulnerabilità si verifica tipicamente negli endpoint in cui le preferenze dell'utente (ad esempio, temi personalizzati, colori dei caratteri) vengono riflesse all'interno di blocchi `<style>` o attributi `style` inline. Dal punto di vista di un attaccante, lo sfruttamento con successo può portare a UI redressing, phishing tramite la modifica non autorizzata del layout o l'esfiltrazione furtiva di dati in ambienti in cui policy restrittive di Content Security Policy potrebbero altrimenti bloccare attacchi basati su JavaScript.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se il punto di iniezione consente l'esfiltrazione di attributi sensibili come i CSRF token o se può essere utilizzato per l'UI redressing in flussi di lavoro sensibili.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### L'applicazione riflette l'input controllato dall'utente all'interno di contesti CSS?
- [ ] No — l'input **non** viene riflesso in tag style, attributi o file CSS  
- [ ] Sì — l'input viene riflesso ma è strettamente limitato a valori alfanumerici sicuri  
- [ ] Sì — l'input viene riflesso all'interno di un attributo `style` o di un blocco `<style>`  

### I metacaratteri e le funzioni specifiche del CSS sono correttamente sanitizzati?
- [ ] Sì — i caratteri come `{`, `}`, `:` e le funzioni come `url()` sono **correttamente sottoposti a escaping**  
- [ ] Sì — la sanitizzazione è presente ma il bypass **è possibile** tramite encoding o commenti CSS  
- [ ] No — nessuna sanitizzazione è applicata ai metacaratteri CSS  

### È possibile l'esfiltrazione di dati utilizzando i selettori CSS?
- [ ] No — i selettori **non possono** innescare richieste esterne o causare leak di dati  
- [ ] Sì — l'esfiltrazione parziale dei dati **è possibile** tramite selettori di attributi e `background-image`  
- [ ] Sì — l'esfiltrazione completa di token sensibili **è possibile** utilizzando tecniche di Brute Force automatizzate  

### Una Content Security Policy (CSP) mitiga il rischio di CSS Injection?
- [ ] Sì — `style-src` è limitato a 'self' e `img-src` o `connect-src` **sono limitati**  
- [ ] Sì — la CSP è presente ma utilizza 'unsafe-inline' o consente domini esterni con wildcard  
- [ ] No — nessuna CSP è implementata per impedire il caricamento di risorse esterne tramite CSS  

### La vulnerabilità può essere utilizzata per eseguire UI redressing o phishing?
- [ ] No — l'ambito dell'iniezione è troppo limitato per modificare il layout in modo significativo  
- [ ] Sì — la modifica della UI **è possibile**, consentendo la sovrapposizione di elementi malevoli  
- [ ] Sì — il controllo completo del layout della pagina **è possibile**, facilitando attacchi di phishing altamente convincenti  

---