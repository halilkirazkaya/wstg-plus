## WSTG-ATHN-03 — Testing for Weak Lock Out Mechanism

Un meccanismo di blocco dell'account (account lockout mechanism) è un controllo critico di difesa in profondità progettato per mitigare attacchi di Brute Force e Credential Stuffing disabilitando un account dopo un numero specifico di tentativi di autenticazione falliti. Senza una robusta politica di blocco, gli attaccanti possono utilizzare strumenti automatizzati per indovinare sistematicamente le password attraverso più account (Password Spraying) o colpire un singolo account di alto valore fino al successo. Questa vulnerabilità si manifesta tipicamente sui portali di login, moduli di reset della password ed endpoint API che mancano di Rate Limiting o protezione a livello di account. Dal punto di vista di un attaccante, un meccanismo di blocco debole o assente consente tentativi di password infiniti, mentre uno eccessivamente aggressivo può essere sfruttato per causare un Denial of Service (DoS) distribuito contro utenti legittimi bloccando intenzionalmente i loro account.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se l'applicazione gestisce PII sensibili, dati finanziari o se gli account amministrativi sono suscettibili al Brute Force senza alcun controllo secondario.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Strumenti:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Viene applicato un meccanismo di blocco dell'account dopo un numero prestabilito di tentativi falliti?
- [ ] Sì — l'account viene bloccato dopo una soglia definita e ragionevole (es. 5-10 tentativi)  
- [ ] Sì — l'account viene bloccato ma solo dopo un numero eccessivamente elevato di tentativi  
- [ ] No — l'account **non può** essere bloccato e consente il Brute Force indefinito  

### Il meccanismo di blocco può essere eluso utilizzando tecniche comuni di bypass?
- [ ] No — il blocco è applicato lato server e **non** è influenzato da modifiche lato client  
- [ ] Sì — è **possibile** eludere il blocco ruotando gli indirizzi IP o utilizzando un pool di proxy  
- [ ] Sì — è **possibile** eludere il blocco manipolando gli header HTTP come `X-Forwarded-For` o `User-Agent`  
- [ ] Sì — è **possibile** eludere il blocco cancellando i cookie o avviando nuove sessioni  

### Il meccanismo di blocco fornisce un percorso per il ripristino dell'account o una scadenza?
- [ ] Sì — l'account viene sbloccato automaticamente dopo una durata stabilita o tramite verifica via email  
- [ ] Sì — l'account richiede l'intervento di un amministratore per lo sblocco  
- [ ] No — il blocco è permanente senza un chiaro percorso di ripristino, aumentando il rischio di DoS  

### L'applicazione distingue tra un nome utente valido e uno non valido durante il blocco?
- [ ] No — la risposta dell'applicazione è identica sia per gli utenti esistenti che per quelli non esistenti  
- [ ] Sì — l'applicazione rivela se un account è bloccato, consentendo l'**enumerazione dei nomi utente** (API o Username Enumeration)  

### Il meccanismo di blocco è suscettibile a un attacco Denial of Service (DoS)?
- [ ] No — controlli come CAPTCHA o il Rate Limiting basato su IP prevengono il blocco di massa degli account  
- [ ] Sì — un attaccante **può** bloccare sistematicamente tutti gli utenti conosciuti fornendo password errate  

---