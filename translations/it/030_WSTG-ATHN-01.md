## WSTG-ATHN-01 — Test per le credenziali trasmesse su un canale crittografato

Il test per le credenziali trasmesse su un canale crittografato garantisce che i dati di autenticazione sensibili, come nomi utente, password e session tokens, siano protetti dall'intercettazione durante il transito. Gli attaccanti posizionati sulla rete — ad esempio tramite attacchi Man-in-the-Middle (MitM) su Wi-Fi pubblici o reti interne compromesse — possono utilizzare strumenti di packet sniffing per catturare credenziali in chiaro se TLS/SSL non è applicato correttamente. Questa vulnerabilità si verifica tipicamente negli endpoint di login, nei moduli di reset della password e negli header di autenticazione delle API dove l'applicazione non impone l'uso di HTTPS o effettua reindirizzamenti errati da HTTP. Lo sfruttamento riuscito garantisce all'attaccante l'accesso completo agli account utente, portando alla compromissione totale dei dati dell'utente e al potenziale movimento laterale (Lateral Movement) all'interno dell'applicazione.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Strumenti:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### L'HTTPS è imposto per l'intero processo di autenticazione?
- [ ] Sì — HSTS è **abilitato** e tutto il traffico è forzato rigorosamente su HTTPS  
- [ ] Sì — avviene il reindirizzamento a HTTPS, ma HSTS **non è abilitato**  
- [ ] No — le credenziali **possono** essere inviate su HTTP non crittografato  

### Le pagine di login e i moduli sono forniti tramite un canale crittografato?
- [ ] Sì — la pagina contenente il modulo di login è distribuita tramite HTTPS  
- [ ] No — la pagina di login è servita su HTTP, consentendo il form-action hijacking o lo sniffing delle credenziali  

### Il flag `Secure` è applicato a tutti i cookie di sessione sensibili?
- [ ] Sì — il flag `Secure` è **applicato** a tutti i cookie di autenticazione e di sessione  
- [ ] Sì — il flag `Secure` è **applicato** solo ad alcuni cookie  
- [ ] No — il flag `Secure` **non è applicato**, consentendo l'esfiltrazione dei cookie tramite richieste non crittografate  

### Il server supporta versioni TLS deboli o cipher suite insicure?
- [ ] No — sono **abilitati** solo TLS moderni (1.2+) e cifrari forti  
- [ ] Sì — sono **supportate** versioni legacy (TLS 1.0/1.1) o cifrari deboli (RC4, DES, CBC)  

### L'applicazione impedisce la trasmissione delle credenziali verso domini di terze parti su canali non crittografati?
- [ ] Sì — nessun dato sensibile viene inviato a domini esterni o tutte le chiamate esterne sono forzate su HTTPS  
- [ ] No — header di autenticazione o credenziali vengono inviati a endpoint di terze parti (ad es. analytics o SSO) tramite HTTP  

---