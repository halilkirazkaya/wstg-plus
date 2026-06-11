## WSTG-CLNT-09 — Testing for Clickjacking

Il Clickjacking, noto anche come UI redressing, si verifica quando un utente malintenzionato utilizza livelli trasparenti o opachi per indurre un utente a fare clic su un pulsante o un collegamento su un'altra pagina quando questi intendeva fare clic sulla pagina di livello superiore. Questa vulnerabilità compromette l'integrità delle azioni dell'utente, portando potenzialmente a modifiche non autorizzate dei dati, cambiamenti dell'account o trasferimenti finanziari eseguiti per conto della vittima. Gli attaccanti tipicamente sfruttano questa vulnerabilità incorporando il sito web di destinazione all'interno di un `<iframe>` su un sito malevolo controllato e sovrapponendovi contenuti ingannevoli o esche allettanti. Si verifica più frequentemente su pagine che eseguono azioni sensibili che modificano lo stato, come i pulsanti "Elimina account", i moduli di reimpostazione della password o i pannelli di configurazione amministrativa.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Media / Alta* |

> *La severità diventa Alta se azioni sensibili (ad esempio, trasferimenti di fondi, eliminazione dell'account o escalation dei privilegi) possono essere attivate tramite clickjacking.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Strumenti:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### L'applicazione implementa header lato server per impedire il framing non autorizzato?
- [ ] Sì — `X-Frame-Options` o `Content-Security-Policy` **vengono applicati** e impediscono il framing *(Più sicuro)*  
- [ ] Sì — gli header sono presenti ma **configurati in modo errato** (ad esempio, `X-Frame-Options: ALLOW-FROM` che manca di un ampio supporto da parte dei browser)  
- [ ] No — non **viene applicato** alcun header di protezione contro il framing  

### La direttiva `frame-ancestors` della `Content-Security-Policy` (CSP) è implementata?
- [ ] Sì — `frame-ancestors 'none'` o `'self'` **viene applicato**  
- [ ] Sì — `frame-ancestors` è presente ma **consente** origini non attendibili o eccessivamente ampie  
- [ ] No — la direttiva è **mancante** o la CSP non è implementata  

### L'header `X-Frame-Options` (XFO) è configurato correttamente per il supporto dei browser legacy?
- [ ] Sì — `DENY` o `SAMEORIGIN` **viene applicato**  
- [ ] Sì — XFO è presente ma **ignorato** dai browser moderni perché esiste una direttiva CSP `frame-ancestors`  
- [ ] No — l'header XFO è **mancante** o impostato su un valore non sicuro  

### Le protezioni contro il framing possono essere bypassate utilizzando tecniche note?
- [ ] No — il bypass **non è possibile** tramite tecniche standard (double-framing, ecc.)  
- [ ] Sì — la protezione **può** essere bypassata a causa di regex o logica debole nella whitelist di `frame-ancestors`  
- [ ] Sì — la protezione **può** essere bypassata perché l'applicazione si affida esclusivamente al frame-busting basato su JavaScript  

### Un'azione sensibile è sfruttabile tramite un proof-of-concept di clickjacking?
- [ ] No — il framing è bloccato o non sono raggiungibili azioni sensibili  
- [ ] Sì — l'UI redressing **è possibile** ma richiede un'interazione complessa da parte dell'utente  
- [ ] Sì — l'UI redressing **è possibile** e consente azioni immediate che modificano lo stato *(Critico)*  

---