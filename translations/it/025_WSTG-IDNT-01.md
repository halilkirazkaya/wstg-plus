## WSTG-IDNT-01 — Test delle Definizioni dei Ruoli

Il test delle definizioni dei ruoli prevede l'identificazione e la documentazione dei vari ruoli utente e dei livelli di autorizzazione definiti all'interno dell'applicazione per garantire che siano logicamente separati e aderiscano al principio del minimo privilegio (**least privilege**). Un attaccante analizza l'applicazione per mappare i ruoli ad alto privilegio rispetto ai ruoli utente standard, cercando eventuali ambiguità nei confini dei permessi o funzioni amministrative non documentate. L'identificazione di questi ruoli è il primo passo per scoprire percorsi di **privilege escalation**, poiché le incongruenze nelle definizioni dei ruoli portano spesso all'accesso non autorizzato a dati sensibili o funzionalità amministrative. Questa fase si verifica tipicamente durante la ricognizione iniziale e la mappatura della **business logic**, concentrandosi sui profili utente, sulle dashboard amministrative e sulle strutture dei permessi delle **API**.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### I ruoli sono chiaramente definiti e documentati nell'applicazione o nella sua documentazione?
- [ ] Sì — i ruoli sono completamente documentati e seguono il principio del **least privilege**  
- [ ] Sì — i ruoli sono documentati ma contengono **permessi sovrapposti**  
- [ ] No — non esiste alcuna documentazione o definizione formale dei ruoli  

### Esiste una chiara separazione tra le funzioni amministrative e quelle degli utenti standard?
- [ ] Sì — le funzioni amministrative sono **completamente isolate** dalle viste degli utenti standard  
- [ ] Sì — la separazione è presente ma gli endpoint amministrativi sono **prevedibili o indovinabili**  
- [ ] No — le funzioni amministrative e standard sono **mischiate** all'interno delle stesse interfacce  

### L'applicazione utilizza un meccanismo centralizzato per il Role-Based Access Control (RBAC)?
- [ ] Sì — il **RBAC** o **ABAC** centralizzato è **implementato** e applicato in modo coerente  
- [ ] Sì — esistono controlli decentralizzati ma sono **applicati in modo incoerente** tra i vari moduli  
- [ ] No — la logica di controllo degli accessi è **hardcoded** nelle singole pagine o componenti  

### Sono presenti ruoli non documentati o nascosti nel sistema?
- [ ] No — tutti i ruoli scoperti corrispondono alle funzionalità documentate  
- [ ] Sì — sono presenti ruoli nascosti o "shadow" (ad es., `super_admin`, `support`, `test`)  

### Un utente con bassi privilegi può enumerare i permessi o i ruoli di altri utenti?
- [ ] No — gli utenti **non possono** visualizzare o enumerare i ruoli/permessi di altre entità  
- [ ] Sì — gli utenti **possono** enumerare i ruoli tramite pagine di profilo, metadati o risposte **API** *(Media)*  

---