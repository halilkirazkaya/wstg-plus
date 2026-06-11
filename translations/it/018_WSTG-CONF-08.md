## WSTG-CONF-08 — Test RIA Cross Domain Policy

Le policy cross-domain delle Rich Internet Application (RIA) coinvolgono file come `crossdomain.xml` e `clientaccesspolicy.xml`, che definiscono il modo in cui i client web—nello specifico Adobe Flash e Microsoft Silverlight—gestiscono le richieste cross-origin. Questi file sono critici per la sicurezza poiché sovrascrivono esplicitamente la Same-Origin Policy (SOP), determinando quali domini esterni siano autorizzati a interagire con l'applicazione e a leggerne le risposte. Configurazioni eccessivamente permissive, come l'uso di wildcard nell'attributo `domain`, permettono agli attaccanti di ospitare oggetti RIA malevoli su siti di terze parti in grado di esfiltrare dati sensibili o eseguire azioni per conto di utenti autenticati. I pentester tipicamente individuano questi file nella root web per valutare il livello di fiducia concesso alle origini di terze parti e identificare potenziali fughe di dati cross-origin.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se l'applicazione gestisce dati utente sensibili o identificativi di sessione che possono essere esfiltrati tramite una policy permissiva.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### I file delle policy cross-domain (`crossdomain.xml` o `clientaccesspolicy.xml`) sono presenti nella root web?
- [ ] No — i file **non** possono essere trovati nella directory root  
- [ ] Sì — `crossdomain.xml` (Flash) **è** presente  
- [ ] Sì — `clientaccesspolicy.xml` (Silverlight) **è** presente  
- [ ] Sì — entrambi i file delle policy RIA **sono** presenti  

### L'attributo di dominio `allow-access-from` è limitato a origini fidate?
- [ ] No — i file esistono ma non contengono voci `allow-access-from`  
- [ ] Sì — domini specifici e fidati sono esplicitamente in whitelist e il bypass **non è possibile**  
- [ ] Sì — i domini sono in whitelist ma includono terze parti non fidate o sottodomini  
- [ ] Sì — viene utilizzata una wildcard `*`, consentendo l'accesso da **qualsiasi** origine *(Critico)*  

### La policy permette comunicazioni non sicure via HTTP per risorse HTTPS?
- [ ] No — `secure="true"` è impostato (o è il valore predefinito), impedendo alle origini HTTP di accedere ai dati HTTPS  
- [ ] Sì — `secure="false"` è impostato esplicitamente, permettendo ad attaccanti MITM di esfiltrare dati sicuri  

### Gli header HTTP sono eccessivamente permissivi nella configurazione cross-domain?
- [ ] No — `allow-http-request-headers-from` è limitato o **non abilitato**  
- [ ] Sì — header specifici sono consentiti per domini fidati  
- [ ] Sì — viene utilizzata la wildcard `*` per gli header, permettendo agli attaccanti di inviare header personalizzati da qualsiasi dominio  

### La configurazione `site-control` (master-policy) è restrittiva?
- [ ] No — `permitted-cross-domain-policies` è impostato su `none` o `master-only`  
- [ ] Sì — la policy permette ad altri file sul server di definire le proprie regole (`all`)  

---