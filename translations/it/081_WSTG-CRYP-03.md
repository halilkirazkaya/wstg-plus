## WSTG-CRYP-03 — Verifica dell'invio di informazioni sensibili tramite canali non crittografati

La trasmissione di dati sensibili tramite canali non crittografati, come il protocollo HTTP in chiaro, espone le informazioni all'intercettazione e alla modifica tramite attacchi Man-in-the-Middle (MITM). Questa vulnerabilità si verifica tipicamente quando le applicazioni web non riescono a imporre l'uso del Transport Layer Security (TLS) su tutti gli endpoint, in particolare nei componenti legacy, nei moduli di login o durante la transizione iniziale da HTTP a HTTPS. Dal punto di vista di un attaccante, ciò consente lo sniffing passivo di credenziali di autenticazione, session token e personally identifiable information (PII) su segmenti di rete compromessi o non attendibili, come il Wi-Fi pubblico. Garantire che tutti i dati sensibili siano incapsulati all'interno di un tunnel TLS robusto è fondamentale per mantenere la riservatezza e l'integrità delle interazioni degli utenti e prevenire il session hijacking.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Media |

> *La gravità diventa Alta se le credenziali di autenticazione, gli identificatori di sessione o le PII vengono trasmessi in chiaro.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### L'applicazione consente la comunicazione tramite HTTP non crittografato per endpoint sensibili?
- [ ] No — tutto il traffico dell'applicazione è rigorosamente forzato su HTTPS *(Più sicuro)*  
- [ ] Sì — le pagine non sensibili sono accessibili tramite HTTP, ma gli endpoint sensibili **non lo sono**  
- [ ] Sì — gli endpoint sensibili (login, checkout, profilo) **sono** accessibili tramite HTTP non crittografato *(Rischio Elevato)*  

### È presente un reindirizzamento globale da HTTP a HTTPS?
- [ ] Sì — l'applicazione esegue un reindirizzamento 301/302 verso HTTPS per tutte le richieste HTTP  
- [ ] Sì — il reindirizzamento viene eseguito solo per specifici endpoint sensibili  
- [ ] No — non viene applicato alcun reindirizzamento da HTTP a HTTPS  

### È implementato l'HTTP Strict Transport Security (HSTS)?
- [ ] Sì — HSTS è **abilitato** con un `max-age` esteso e include le direttive `includeSubDomains` e `preload`  
- [ ] Sì — HSTS è **abilitato** ma mancano le direttive `includeSubDomains` o `preload`  
- [ ] No — HSTS **non è abilitato** sul server dell'applicazione  

### I cookie sensibili sono protetti dalla trasmissione in chiaro?

| Nome Cookie | Flag Secure Presente | Flag Secure Mancante |
|-------------|:--------------------:|:-------------------:|
| `SessionID` |                      |                     |
| `AuthToken` |                      |                     |
| `CSRF-Token`|                      |                     |

### L'applicazione trasmette dati sensibili ad API di terze parti tramite canali non crittografati?
- [ ] No — tutte le chiamate API esterne vengono eseguite tramite tunnel crittografati (HTTPS)  
- [ ] Sì — alcune telemetrie non sensibili vengono inviate tramite HTTP  
- [ ] Sì — chiavi API, PII o credenziali **vengono** inviate a terze parti tramite HTTP non crittografato  

---