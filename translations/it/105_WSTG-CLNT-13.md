## WSTG-CLNT-13 — Testing for Cross Site Script Inclusion

L'inclusione di script tra siti (Cross-Site Script Inclusion - XSSI) si verifica quando un'applicazione web esporta dati sensibili all'interno di un file JavaScript generato dinamicamente o di una risposta JSONP, che un sito controllato da un attaccante può successivamente includere tramite un tag `<script>`. Poiché l'inclusione di script bypassa la Same-Origin Policy (SOP), un attaccante può eseguire lo script nel proprio contesto e sovrascrivere oggetti globali o variabili per esfiltrare le informazioni sensibili. Questa vulnerabilità si manifesta tipicamente in endpoint autenticati che restituiscono dati specifici dell'utente come indirizzi email, API key o metadati di sessione in formato script. Dal punto di vista di un attaccante, l'exploitation prevede la creazione di una pagina malevola che faccia riferimento allo script vulnerabile e catturi le modifiche alle variabili globali o le chiamate a funzioni risultanti.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Media / Alta |

> *La severità diventa Alta se i dati trapelati includono CSRF tokens, identificatori di sessione o PII altamente sensibili.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Esistono endpoint autenticati che restituiscono JavaScript dinamico o JSONP contenente informazioni sensibili?
- [ ] No — tutti gli script sono statici e **non possono** contenere dati specifici dell'utente  
- [ ] Sì — esistono script dinamici ma **non contengono** informazioni sensibili  
- [ ] Sì — gli script dinamici contengono PII o token e **sono** accessibili tra origini diverse (cross-origin)  

### L'header `X-Content-Type-Options: nosniff` è implementato nelle risposte degli script sensibili?
- [ ] Sì — l'header è **applicato** e impedisce il MIME-type sniffing  
- [ ] No — l'header è **mancante**, consentendo potenzialmente l'esecuzione di contenuti non script come script  

### Gli script sensibili sono protetti da meccanismi anti-XSSI come prefissi non eseguibili o header personalizzati?
- [ ] Sì — gli script richiedono un header personalizzato o un token che **non può** essere inviato tramite un tag `<script>` standard  
- [ ] Sì — gli script utilizzano prefissi non eseguibili (es. `)]}'`) che **non possono** essere facilmente aggirati  
- [ ] No — gli script sono direttamente accessibili tramite richieste GET utilizzando credenziali ambientali *(Critico)*  

### L'esfiltrazione di dati sensibili è possibile sovrascrivendo variabili globali o prototipi?
- [ ] No — i dati sensibili hanno uno scope locale o sono protetti tramite funzionalità moderne dei browser  
- [ ] Sì — i dati sensibili sono assegnati a variabili globali e **possono** essere catturati  
- [ ] Sì — i dati trapelano attraverso callback JSONP che **possono** essere intercettate  

---