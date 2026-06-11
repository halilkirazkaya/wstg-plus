## WSTG-INPV-08 — Testing for SSI Injection

La Server-Side Includes (SSI) Injection si verifica quando un'applicazione non riesce a sanitizzare l'input dell'utente prima di incorporarlo nei file HTML elaborati dal motore SSI del server. Gli attaccanti sfruttano questa vulnerabilità iniettando direttive SSI, come `<!--#exec cmd="id" -->`, per eseguire comandi shell arbitrari, leggere file locali o esfiltrare variabili d'ambiente. Questa vulnerabilità si trova tipicamente nelle applicazioni che servono estensioni di file legacy come `.shtml`, `.shtm` o `.stm`, o dove il web server è configurato esplicitamente per analizzare le SSI all'interno di file HTML standard. Un Exploit riuscito garantisce all'attaccante i medesimi permessi del processo del web server, portando spesso a una compromissione completa del sistema o all'esposizione di dati sensibili.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Il web server supporta ed elabora le direttive SSI?
- [ ] No — il server **non** supporta SSI o le estensioni di file come `.shtml` sono **disabilitate**  
- [ ] Sì — le SSI **sono abilitate** ma l'input dell'utente **non** viene riflesso nelle pagine elaborate  
- [ ] Sì — le SSI **sono abilitate** e l'input dell'utente **viene** riflesso nelle pagine elaborate  

### È possibile eseguire comandi di sistema arbitrari tramite la direttiva `#exec`?
- [ ] No — la direttiva `#exec` è **disabilitata** o limitata dalla configurazione del server  
- [ ] Sì — l'esecuzione di comandi **è possibile** tramite payload `#exec cmd` o `#exec cgi`  

### È possibile l'esfiltrazione di file sensibili o variabili d'ambiente?
- [ ] No — le direttive come `#include` o `#config` sono **disabilitate**  
- [ ] Sì — la lettura di file locali (es. `/etc/passwd`) **è possibile** tramite `#include virtual`  
- [ ] Sì — le variabili d'ambiente del server **possono** essere esfiltrate tramite `#printenv` o `#echo`  

### La validazione dell'input o i controlli WAF sono efficaci contro i payload SSI?
- [ ] Sì — una validazione dell'input rigorosa **è applicata** e il bypass **non è possibile**  
- [ ] Sì — la validazione/WAF **è applicata** ma il bypass **è possibile** utilizzando la codifica dei caratteri o una differente sintassi SSI  
- [ ] No — non è presente alcuna validazione o protezione WAF per i caratteri SSI (`<`, `!`, `#`, `-`, `"`)  

---