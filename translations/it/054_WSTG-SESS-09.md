## WSTG-SESS-09 — Testing for Session Hijacking

Il Session Hijacking si verifica quando un utente malintenzionato cattura, predice o fissa un identificatore di sessione (SID) valido per ottenere l'accesso non autorizzato a una sessione attiva di un utente. Questa vulnerabilità si manifesta tipicamente attraverso una sicurezza insufficiente del transport layer, la mancanza di flag di sicurezza nei cookie o algoritmi di generazione del SID prevedibili che consentono a un attaccante di bypassare completamente l'autenticazione. Dal punto di vista di un attaccante, lo sfruttamento con successo di questa vulnerabilità permette l'impersonificazione completa della vittima senza richiederne le credenziali, spesso ottenuta tramite network sniffing, Cross-Site Scripting (XSS) o attacchi di session fixation.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Strumenti:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Gli identificatori di sessione sono protetti durante il transito?
- [ ] Sì — il flag `Secure` è **abilitato** e HSTS è applicato rigorosamente  
- [ ] Sì — il flag `Secure` è **abilitato** ma HSTS è **disabilitato**  
- [ ] No — il flag `Secure` **non è applicato**, consentendo l'intercettazione del SID su canali non criptati *(Alta)*  

### L'identificatore di sessione è protetto dall'accesso tramite script lato client?
- [ ] Sì — il flag `HttpOnly` è **applicato** a tutti i cookie relativi alla sessione  
- [ ] No — il flag `HttpOnly` **non è applicato**, consentendo l'esfiltrazione del SID tramite XSS *(Critica)*  

### L'applicazione implementa il session binding agli attributi del client?
- [ ] Sì — la sessione è legata agli attributi del client (IP/User-Agent) e il riutilizzo **non è possibile**  
- [ ] Sì — il session binding è presente ma il bypass **è possibile** tramite header spoofing  
- [ ] No — non esiste alcun session binding; il SID **è valido** se utilizzato da qualsiasi sorgente  

### L'identificatore di sessione è sufficientemente casuale da prevenirne la predizione?
- [ ] Sì — il SID utilizza un generatore di numeri pseudo-casuali crittograficamente sicuro (CSPRNG)  
- [ ] No — la lunghezza del SID è insufficiente o segue una sequenza/pattern **prevedibile**  

### Le sessioni simultanee sono gestite per limitare la finestra di attacco?
- [ ] No — l'applicazione impone rigorosamente una singola sessione attiva per utente  
- [ ] Sì — sessioni simultanee multiple sono **abilitate** ma monitorate  
- [ ] Sì — sessioni simultanee illimitate **sono possibili** attraverso diversi dispositivi/posizioni  

---