## WSTG-CRYP-02 — Testing for Padding Oracle

Le vulnerabilità di Padding Oracle si verificano quando il processo di decrittografia di un'applicazione rivela informazioni sulla validità o meno del padding di un testo cifrato (ciphertext) decifrato. Osservando queste risposte side-channel — come codici di stato HTTP distinti, messaggi di errore specifici o variazioni nei tempi di risposta — un attaccante può decrittografare i dati senza conoscere la chiave di cifratura e, potenzialmente, cifrare nuovamente testo in chiaro (plaintext) arbitrario. Questa vulnerabilità si manifesta tipicamente in cookie di sessione cifrati, token di autenticazione o parametri URL che utilizzano cifrari a blocchi (block ciphers) in modalità Cipher Block Chaining (CBC) con padding PKCS#7. Lo sfruttamento (exploitation) consente la perdita completa della riservatezza e può portare alla compromissione dell'integrità attraverso la costruzione di blocchi cifrati contraffatti.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica* |

> *La gravità è Critica se la vulnerabilità risiede nella gestione della sessione, nei token di identità o nella cifratura di PII.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Viene utilizzato un cifrario a blocchi in modalità CBC per dati sensibili?
- [ ] No — l'applicazione utilizza la cifratura autenticata (AES-GCM/ChaCha20-Poly1305)  
- [ ] Sì — l'applicazione utilizza cifrari a blocchi ma il padding **non è** richiesto (es. cifrari a flusso o modalità CTR)  
- [ ] Sì — l'applicazione utilizza la modalità CBC e il padding **viene applicato**  

### Il server rivela la validità del padding attraverso canali laterali (side channels)?
- [ ] No — il server restituisce messaggi di errore uniformi e tempi di risposta coerenti  
- [ ] Sì — il server restituisce codici di stato HTTP differenti (es. 200 vs 500) per errori di padding  
- [ ] Sì — il server restituisce messaggi di errore specifici (es. "Invalid Padding", "Decryption failed")  
- [ ] Sì — il server mostra differenze temporali misurabili durante il fallimento della decrittografia  

### È possibile la decrittografia automatizzata del ciphertext?
- [ ] No — i canali laterali **non sono** sufficientemente stabili per l'exploitation  
- [ ] Sì — il ciphertext **può** essere parzialmente decifrato (gli ultimi blocchi)  
- [ ] Sì — il recupero completo del plaintext **è possibile** utilizzando strumenti come `PadBuster`  

### L'attaccante può eseguire bit-flipping o contraffare ciphertext validi?
- [ ] No — i controlli di integrità (HMAC) vengono verificati **prima** della decrittografia  
- [ ] Sì — non è presente alcun controllo di integrità, ma la costruzione del payload **non è** fattibile  
- [ ] Sì — la costruzione di ciphertext arbitrari **è possibile**, consentendo il privilege escalation o l'injection di dati  

---