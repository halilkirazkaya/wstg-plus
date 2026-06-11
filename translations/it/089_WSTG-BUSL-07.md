## WSTG-BUSL-07 — Test delle Difese contro l'Abuso dell'Applicazione

Le difese contro l'abuso dell'applicazione valutano la capacità dell'applicazione di identificare, loggare e rispondere a comportamenti anomali degli utenti che deviano dalla logica di business prevista. A differenza della sicurezza tradizionale basata su firme, queste difese si concentrano sul rilevamento di pattern come richieste a raffica (rapid-fire), Credential Stuffing o enumerazione sequenziale delle risorse che indicano un abuso automatizzato. Dal punto di vista di un attaccante, questo test determina la resilienza dell'applicazione contro l'automazione e gli attacchi "low and slow" progettati per esfiltrare dati o esaurire le risorse senza attivare i normali avvisi di sicurezza. La mancata implementazione di queste difese spesso si traduce in massiccio Data Scraping, Account Takeover o Denial of Service attraverso funzionalità dell'applicazione legittime ma ad alta intensità di risorse.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Sono applicati meccanismi di rate limiting o throttling sugli endpoint sensibili?
- [ ] Sì — un Rate Limiting rigoroso **è applicato** e imposto su tutti gli endpoint sensibili *(Massima sicurezza)*  
- [ ] Sì — il Rate Limiting **è applicato** ma può essere bypassato tramite rotazione degli IP o manipolazione degli header  
- [ ] Sì — il Rate Limiting **è applicato** solo agli endpoint di autenticazione, lasciando gli altri esposti  
- [ ] No — nessun Rate Limiting o Throttling **è applicato** ad alcuna funzionalità dell'applicazione  

### L'applicazione rileva e blocca i pattern di interazione automatizzata?
- [ ] Sì — l'analisi comportamentale o i CAPTCHA **sono abilitati** e bloccano efficacemente i bot  
- [ ] Sì — l'anti-automazione **è abilitata** ma il bypass **è possibile** utilizzando browser headless o rotazione delle sessioni  
- [ ] No — l'applicazione **non può** distinguere tra un utente umano e uno script automatizzato  

### Il comportamento anomalo viene loggato e seguito da una risposta difensiva?
- [ ] Sì — l'abuso attiva il blocco automatizzato e avvisa il team di sicurezza  
- [ ] Sì — l'abuso viene loggato per audit ma **nessuna** azione difensiva automatizzata viene intrapresa  
- [ ] No — l'applicazione **non logga** né risponde a pattern di attività insoliti  

### Le difese possono essere bypassate manipolando identificativi o header lato client?
- [ ] No — le difese si basano sullo stato lato server e su telemetria verificata  
- [ ] Sì — i controlli **possono** essere bypassati cancellando i cookie o ruotando le stringhe di `User-Agent`  
- [ ] Sì — i controlli **possono** essere bypassati iniettando header come `X-Forwarded-For` o `X-Real-IP`  

---