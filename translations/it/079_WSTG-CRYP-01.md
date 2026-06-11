## WSTG-CRYP-01 — Test per la Sicurezza Debole del Transport Layer

Il test per la sicurezza debole del transport layer comporta l'analisi dell'implementazione e della configurazione di SSL/TLS per garantire la riservatezza e l'integrità dei dati durante il transito. Gli attaccanti mirano a configurazioni errate come protocolli obsoleti (SSLv2, TLS 1.0), cipher suite deboli (NULL, RC4 o anonime) e certificati non validi o con firme deboli per eseguire attacchi Man-in-the-Middle (MitM). Questo test solitamente riguarda gli endpoint del server web o del load balancer dell'applicazione in cui viene stabilita la comunicazione crittografata. Uno sfruttamento riuscito consente a un avversario di intercettare dati sensibili, dirottare le sessioni utente (hijacking) o declassare le connessioni a stati non sicuri attraverso il protocol downgrade o la decrittazione del traffico.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Media* |

> *La gravità è Alta se vengono trasmessi dati sensibili (PII, credenziali, session token); Media per il traffico informativo generale.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Sono supportati protocolli TLS/SSL obsoleti o non sicuri?
- [ ] No — solo TLS 1.2 e TLS 1.3 sono **abilitati**  
- [ ] Sì — TLS 1.0 o TLS 1.1 sono **abilitati**  
- [ ] Sì — SSLv2 o SSLv3 sono **abilitati** *(Critico)*  

### Il server accetta cipher suite deboli o non sicure?
- [ ] No — sono **supportate** solo cipher robuste e moderne (es. AES-GCM, ChaCha20)  
- [ ] Sì — sono **supportate** cipher con crittografia debole (es. 3DES, RC4) o lunghezza in bit ridotta  
- [ ] Sì — sono **supportate** cipher NULL, anonime (ADH) o export *(Critico)*  

### Il certificato del server è valido e attendibile?
- [ ] Sì — il certificato è valido, corrisponde al dominio ed è firmato da una **CA attendibile**  
- [ ] No — il certificato è **scaduto** o utilizza un **algoritmo di firma debole** (es. SHA-1)  
- [ ] No — il certificato è **auto-firmato** o si verifica un **mismatch** del nome di dominio  

### Il server supporta la Secure Renegotiation e previene gli attacchi di Downgrade?
- [ ] Sì — la Secure Renegotiation è **supportata** e TLS Fallback SCSV è **abilitato**  
- [ ] No — la Secure Renegotiation **non è supportata**, consentendo potenzialmente l'iniezione MitM  
- [ ] No — il server è vulnerabile agli attacchi **POODLE** o **ROBOT**  

### La HTTP Strict Transport Security (HSTS) è implementata correttamente?

| Header | Presente | Mancante |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Sì — l'header HSTS è presente con un `max-age` lungo e `includeSubDomains` **abilitato**  
- [ ] Sì — l'header HSTS è presente ma manca `includeSubDomains` o ha un **max-age breve**  
- [ ] No — l'header HSTS **non è presente**  

---