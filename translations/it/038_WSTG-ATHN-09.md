## WSTG-ATHN-09 — Testing for Weak Password Change or Reset Functionalities

I meccanismi di modifica e reset della password sono componenti critiche della gestione dell'autenticazione che, se implementate in modo errato, consentono agli attaccanti di compromettere gli account utente. Queste vulnerabilità solitamente coinvolgono token di reset prevedibili, mancanza di account lockouts durante i tentativi di Brute Force, invio non sicuro dei token o la possibilità di modificare le password senza verificare la password corrente. Gli attaccanti sfruttano queste falle intercettando o indovinando i link di recupero, eseguendo Host Header Injection per reindirizzare le email di reset, o utilizzando la Session Fixation per mantenere l'accesso dopo un cambio password. Garantire che questi processi richiedano una verifica robusta e utilizzino la casualità crittografica è essenziale per mantenere l'integrità degli account e prevenire l'accesso non autorizzato.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### È richiesta la password corrente per impostare una nuova password durante una modifica standard?
- [ ] Sì — la password corrente **è richiesta** e verificata  
- [ ] Sì — la password corrente **è richiesta** ma può essere bypassata tramite Parameter Pollution o manipolazione  
- [ ] No — la password corrente **non è** richiesta, consentendo l'account takeover tramite Session Hijacking *(Alta)*  

### I token di reset della password sono crittograficamente sicuri e non prevedibili?
- [ ] Sì — i token sono ad alta entropia, casuali e **non prevedibili**  
- [ ] Sì — i token vengono generati ma mostrano bassa entropia o pattern prevedibili (es. basati su timestamp)  
- [ ] No — i token sono **prevedibili** o basati su dati utente statici come email/ID codificati in Base64 *(Critica)*  

### Il link di reset della password può essere reindirizzato tramite Host Header Injection?
- [ ] No — l'applicazione ignora o convalida l'header `Host` per la generazione dei link  
- [ ] Sì — l'applicazione utilizza l'header `Host` o `X-Forwarded-Host` per costruire i link, ma la validazione **è presente**  
- [ ] Sì — i link **possono** essere reindirizzati verso un dominio controllato dall'attaccante tramite la manipolazione degli header *(Alta)*  

### Il token di reset ha un ciclo di vita limitato e un'invalidazione immediata?
- [ ] Sì — i token scadono rapidamente e vengono invalidati **immediatamente** dopo l'uso o la modifica della password  
- [ ] Sì — i token scadono dopo una lunga durata (es. 24+ ore) o consentono **utilizzi multipli**  
- [ ] No — i token **non scadono mai** o rimangono validi dopo che la password è stata modificata con successo  

### Sono presenti Rate Limiting o blocchi (lockouts) sugli endpoint di reset e modifica della password?
- [ ] Sì — Rate Limiting rigoroso e CAPTCHA **sono applicati** per prevenire l'enumerazione e il Brute Force  
- [ ] Sì — esistono controlli limitati ma il bypass **è possibile** tramite rotazione degli IP o header spoofing  
- [ ] No — nessun Rate Limiting **è applicato**, consentendo l'enumerazione massiva degli account o il Brute Force dei token  

---