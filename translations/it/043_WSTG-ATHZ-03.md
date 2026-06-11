## WSTG-ATHZ-03 — Testing for Privilege Escalation

Il privilege escalation si verifica quando un utente malintenzionato sfrutta delle vulnerabilità per ottenere l'accesso a risorse o funzionalità riservate a utenti con livelli di autorizzazione superiori o identità diverse. Nell'escalation verticale (vertical escalation), un utente standard tenta di accedere a funzioni amministrative, mentre l'escalation orizzontale (horizontal escalation) comporta l'accesso a dati appartenenti a un altro utente con lo stesso livello di privilegi. Questi difetti si manifestano tipicamente in access control lists (ACL) implementate in modo errato, insecure direct object references (IDOR) o nella manipolazione dei parametri all'interno dei token di sessione o di identità. Dal punto di vista di un attaccante, ciò si ottiene spesso manipolando ID numerici, modificando parametri basati sui ruoli nei cookie o nei JWT, o chiamando direttamente endpoint API nascosti che mancano di controlli di autorizzazione lato server.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Strumenti:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### L'applicazione implementa più livelli di privilegio o il multi-tenancy?
- [ ] No — l'applicazione è un sistema a utente singolo o a ruolo singolo  
- [ ] Sì — sono definiti più ruoli o tenant e **richiedono** test di autorizzazione  

### È possibile un privilege escalation orizzontale (accesso allo stesso livello)?
- [ ] No — i controlli di autorizzazione **sono applicati** a tutti gli identificatori di risorse e ai proprietari delle sessioni  
- [ ] Sì — l'accesso ai dati di altri utenti **è possibile** tramite IDOR o manipolazione dei parametri  
- [ ] Sì — il data leakage **è possibile** ma la modifica dei dati di altri utenti **non è possibile**  

### È possibile un privilege escalation verticale (da basso ad alto)?
- [ ] No — gli endpoint amministrativi applicano rigorosamente i controlli di accesso basati sui ruoli (RBAC)  
- [ ] Sì — le funzioni amministrative **possono** essere accessibili da utenti con privilegi bassi tramite l'accesso diretto agli URL  
- [ ] Sì — le funzioni amministrative **possono** essere accessibili manipolando header relativi ai ruoli, cookie o claim JWT  

### I ruoli o i permessi degli utenti possono essere modificati tramite Mass Assignment o Parameter Pollution?
- [ ] No — i campi basati sui ruoli sono rigorosamente in sola lettura e **non possono** essere modificati dagli utenti  
- [ ] Sì — i permessi dell'utente **possono** essere elevati includendo parametri nascosti (ad es. `role=admin`) negli aggiornamenti del profilo o nella registrazione  

### I controlli di autorizzazione sono applicati in modo coerente in tutte le versioni delle API e nei metodi HTTP?
- [ ] Sì — i controlli **sono applicati** in modo coerente in tutte le versioni e i metodi  
- [ ] No — le versioni legacy delle API (ad es. `/v1/`) o specifici metodi HTTP (ad es. `PUT`, `DELETE`, `PATCH`) **bypassano** l'autorizzazione  
- [ ] No — le funzioni amministrative sono nascoste nell'interfaccia utente ma **abilitate** a livello API per tutti gli utenti  

---