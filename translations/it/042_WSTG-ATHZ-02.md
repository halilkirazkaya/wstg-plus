## WSTG-ATHZ-02 — Testing for Bypassing Authorization Schema

L'aggiramento degli schemi di autorizzazione si verifica quando un'applicazione non riesce a imporre correttamente i controlli di accesso, consentendo a un utente malintenzionato di accedere a risorse o funzioni al di fuori dei permessi previsti. Questa vulnerabilità si manifesta tipicamente come Horizontal Privilege Escalation, dove un attaccante accede a dati appartenenti a un altro utente dello stesso livello, o Vertical Privilege Escalation, dove un utente con privilegi limitati ottiene capacità amministrative. Gli attaccanti sfruttano queste falle manipolando i parametri delle richieste, come i resource IDs, modificando i session tokens o utilizzando header HTTP come `X-Original-URL` per eludere le restrizioni basate sul percorso. Garantire un'autorizzazione robusta è fondamentale per mantenere la riservatezza dei dati e prevenire azioni amministrative non autorizzate che potrebbero compromettere l'intero sistema.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Strumenti:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### L'escalation orizzontale dei privilegi è impedita tra utenti dello stesso ruolo?
- [ ] Sì — i controlli di autorizzazione **sono applicati** a ogni richiesta di risorsa e il bypass **non è possibile**  
- [ ] Sì — i controlli di autorizzazione **sono applicati** ma possono essere aggirati tramite IDOR o parameter tampering  
- [ ] No — gli utenti **possono** accedere ai dati appartenenti ad altri utenti semplicemente modificando un resource ID *(Alta)*  

### L'escalation verticale dei privilegi è impedita per gli utenti con privilegi limitati?
- [ ] No — l'applicazione ha un solo livello di privilegio  
- [ ] Sì — le funzioni amministrative **non sono accessibili** agli utenti con privilegi limitati  
- [ ] Sì — le funzioni amministrative sono nascoste nell'interfaccia utente (UI) ma **sono accessibili** tramite URL diretto  
- [ ] No — gli utenti con privilegi limitati **possono** eseguire azioni amministrative manipolando ruoli o permessi *(Critica)*  

### L'autorizzazione può essere aggirata utilizzando l'HTTP verb tampering?
- [ ] No — i controlli di accesso sono imposti indipendentemente dal metodo HTTP utilizzato  
- [ ] Sì — la modifica del metodo (ad es. da `GET` a `POST` o `PUT`) **aggira** i controlli di autorizzazione  

### Gli endpoint amministrativi o riservati sono accessibili senza alcuna autenticazione?
- [ ] No — tutti gli endpoint sensibili richiedono una sessione valida e permessi appropriati  
- [ ] Sì — alcuni endpoint amministrativi sono accessibili se l'attaccante conosce l'URL diretto  
- [ ] Sì — l'accesso non autenticato a funzioni sensibili **è possibile** *(Critica)*  

### Gli header HTTP consentono l'elusione dell'autorizzazione basata sul percorso (path-based)?
- [ ] No — l'applicazione **non è** suscettibile a bypass basati sugli header  
- [ ] Sì — header come `X-Original-URL`, `X-Rewrite-URL` o `X-Forwarded-For` **possono** essere utilizzati per aggirare i controlli di accesso  

---