## WSTG-CRYP-04 — Test per Primitive Crittografiche Deboli

Le primitive crittografiche deboli comportano l'uso di algoritmi obsoleti o insicuri, come MD5, SHA-1, DES o RC4, che sono suscettibili di attacchi di collisione, analisi delle frequenze o decrittazione tramite Brute Force. Queste debolezze si verificano tipicamente nell'hashing delle password, nella crittografia dei dati a riposo (data at rest), nella generazione di token di sessione o nella verifica delle firme digitali all'interno del backend dell'applicazione. Dal punto di vista di un attaccante, l'identificazione di queste primitive consente di compromettere l'integrità e la riservatezza dei dati, ad esempio craccando gli hash intercettati per recuperare le password in chiaro (cleartext) o contraffacendo i token di autenticazione. Le applicazioni moderne dovrebbero invece utilizzare primitive standard di settore, resistenti alle collisioni e ad alta entropia come Argon2, Bcrypt o AES-GCM per garantire una protezione robusta contro la crittoanalisi.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Vengono utilizzati algoritmi di hashing deprecati come MD5 o SHA-1 per l'archiviazione di dati sensibili?
- [ ] No — l'applicazione utilizza moderni algoritmi resistenti alle collisioni come Argon2 o Bcrypt  
- [ ] Sì — vengono utilizzati algoritmi deprecati ma **solo** per controlli di integrità non sensibili ai fini della sicurezza  
- [ ] Sì — gli algoritmi deprecati **vengono utilizzati** per password o token sensibili *(Alta)*  

### Vengono attualmente impiegati cifrari di crittografia simmetrica deboli come DES, 3DES o RC4?
- [ ] No — viene utilizzato AES (128 bit o superiore) in una modalità sicura come GCM o CBC  
- [ ] Sì — i cifrari legacy sono **abilitati** per la retrocompatibilità ma **non** sono l'impostazione predefinita  
- [ ] Sì — i cifrari legacy sono il metodo principale per proteggere i dati a riposo o in transito  

### L'applicazione implementa logiche crittografiche personalizzate ("roll-your-own") o non standard?
- [ ] No — l'applicazione aderisce rigorosamente a librerie crittografiche standard e sottoposte a peer-review  
- [ ] Sì — esistono wrapper personalizzati ma le primitive sottostanti **sono** standard di settore  
- [ ] Sì — algoritmi crittografici proprietari o logiche personalizzate **vengono applicati** a dati sensibili *(Critica)*  

### I salt crittografici e i vettori di inizializzazione (IV) sono gestiti secondo le best practice?
- [ ] Sì — i salt sono univoci per utente e gli IV sono imprevedibili per ogni operazione  
- [ ] Sì — i salt vengono utilizzati ma **non** sono univoci per tutti i record  
- [ ] No — i salt sono statici, hardcoded o assenti, e gli IV **vengono riutilizzati** tra più sessioni  

### La lunghezza in bit delle chiavi asimmetriche (RSA, Diffie-Hellman) è sufficiente per gli standard moderni?
- [ ] Sì — le chiavi RSA/DH sono a 2048 bit o superiori, oppure viene utilizzata la crittografia a curve ellittiche (ECC)  
- [ ] No — le chiavi sono inferiori a 2048 bit ma il bypass **non** è banale  
- [ ] No — le chiavi sono a 1024 bit o inferiori, rendendole **vulnerabili** alla fattorizzazione o ad attacchi computazionali  

---