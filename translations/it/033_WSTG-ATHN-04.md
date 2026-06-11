## WSTG-ATHN-04 — Test per l'aggiramento dello schema di autenticazione (Bypassing Authentication Schema)

L'aggiramento dello schema di autenticazione (Bypassing Authentication Schema) comporta l'identificazione e lo sfruttamento di falle nella logica applicativa che permettono a un attaccante di ottenere l'accesso a risorse protette senza fornire credenziali valide. Questa vulnerabilità si verifica tipicamente quando l'applicazione si affida a controlli lato client, non riesce a imporre l'autorizzazione lato server su ogni richiesta, o lascia esposte in produzione "backdoor" amministrative ed endpoint di debug. Gli attaccanti sfruttano queste debolezze eseguendo forced browsing verso URL sensibili, manipolando parametri HTTP come `authenticated=true`, o effettuando lo spoofing degli header per indurre l'applicazione a presumere che una sessione sia già stata stabilita. Uno sfruttamento (Exploit) riuscito comporta una compromissione totale del perimetro di autenticazione, portando potenzialmente all'esfiltrazione non autorizzata di dati o alla completa compromissione del sistema.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Test Status** | Non Eseguito |
| **Severity** | Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Strumenti:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### È possibile accedere direttamente a pagine interne sensibili senza una sessione attiva?
- [ ] No — l'accesso diretto comporta il reindirizzamento al login o una risposta 403/404  
- [ ] Sì — alcune pagine sensibili sono accessibili ma forniscono dati limitati o non sensibili  
- [ ] Sì — le pagine amministrative sensibili o specifiche dell'utente **sono** completamente accessibili senza autenticazione *(Critica)*  

### L'applicazione si affida a flag o parametri lato client per determinare lo stato di autenticazione?
- [ ] No — lo stato di autenticazione è gestito rigorosamente tramite lo stato della sessione lato server  
- [ ] Sì — esistono flag lato client ma la loro modifica **non** garantisce l'accesso alle aree protette  
- [ ] Sì — la modifica dei parametri (ad es., `is_authenticated=true`, `user_role=admin`) **permette** il bypass dell'autenticazione  

### L'autenticazione può essere bypassata tramite lo spoofing o la manipolazione di specifici header HTTP?
- [ ] No — gli header come `X-Forwarded-For`, `Referer` o header personalizzati non hanno alcun impatto sull'autenticazione  
- [ ] Sì — la manipolazione degli header (ad es., `X-Custom-IP-Authorization`, `X-Remote-User`) **è possibile** e garantisce l'accesso non autorizzato  

### Esistono percorsi alternativi o artefatti di sviluppo che aggirano il normale flusso di login?
- [ ] No — tutte le risorse protette sono presidiate da un controllo di autenticazione centralizzato e pronto per la produzione  
- [ ] Sì — endpoint legacy, rotte di debug (ad es., `/debug/login`) o versioni API dimenticate **sono** accessibili senza credenziali  

### L'applicazione omette di riautenticare o verificare le sessioni per azioni ad alto privilegio?
- [ ] No — le azioni ad alto privilegio o sensibili richiedono un token di sessione fresco o valido  
- [ ] Sì — una volta trovato un bypass su un endpoint, esso **può** essere utilizzato per eseguire azioni amministrative in tutta l'applicazione  

---