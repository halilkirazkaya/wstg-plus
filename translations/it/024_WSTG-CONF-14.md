## WSTG-CONF-14 — Test delle configurazioni errate di altre intestazioni di sicurezza HTTP

Il test per le configurazioni errate delle intestazioni di sicurezza HTTP comporta la verifica della presenza e della corretta implementazione delle intestazioni difensive che forniscono una protezione essenziale contro i comuni vettori di attacco web. Sebbene queste intestazioni non risolvano le vulnerabilità del codice sottostante, la loro assenza aumenta significativamente il rischio di attacchi Man-in-the-Middle (MITM), dello sfruttamento del MIME-sniffing e della perdita involontaria di dati cross-origin tramite referrer. Dal punto di vista di un attaccante, la mancanza di intestazioni di sicurezza rappresenta un indicatore ad alta visibilità di un ambiente scarsamente protetto (hardened), facilitando spesso catene di exploit più complesse come il session hijacking o l'esfiltrazione di informazioni. Queste configurazioni sono tipicamente valutate a livello di server e dovrebbero essere applicate in modo coerente a tutti i tipi di risposta, inclusi gli endpoint API e le risorse statiche.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audit della presenza delle intestazioni di sicurezza
| Intestazione | Presente | Mancante | Configurazione Raccomandata |
|--------------|:--------:|:--------:|-----------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` o `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Specifica per funzionalità, es., `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### In che modo le intestazioni di sicurezza vengono applicate nell'ambito dell'applicazione?
- [ ] Sì — le intestazioni sono applicate globalmente tramite un load balancer centrale o un reverse proxy e **non possono** essere sovrascritte  
- [ ] Sì — le intestazioni sono applicate alle risposte dei documenti principali ma sono **mancanti** nelle risposte API o negli asset statici  
- [ ] No — le intestazioni sono applicate in modo incoerente o sono **mancanti** in tutto il dominio dell'applicazione  

### L'intestazione Strict-Transport-Security (HSTS) è configurata correttamente?
- [ ] Sì — `max-age` è impostato su una durata lunga (es. 2 anni) e `includeSubDomains` è **abilitato**  
- [ ] Sì — `max-age` è impostato ma `includeSubDomains` è **disabilitato**, lasciando vulnerabili i sottodomini  
- [ ] No — l'intestazione HSTS **non è presente** o `max-age` è impostato su 0, consentendo attacchi di protocol downgrade  

### La Referrer-Policy impedisce la perdita di parametri URL sensibili?
- [ ] Sì — la policy è impostata su `no-referrer` o `same-origin` *(Più sicura)*  
- [ ] Sì — la policy è impostata su `strict-origin-when-cross-origin`  
- [ ] No — la policy **non è impostata** o è impostata su `unsafe-url`, con la potenziale perdita di token o PII sensibili nelle intestazioni referer  

### L'intestazione X-Content-Type-Options impedisce il MIME-sniffing?
- [ ] Sì — la direttiva `nosniff` è **presente** e correttamente implementata  
- [ ] No — l'intestazione è **mancante**, consentendo potenzialmente a un browser di eseguire file non eseguibili (es. immagini) come script  

---