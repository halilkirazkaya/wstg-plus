## WSTG-INPV-04 — Testing for HTTP Parameter Pollution

L'HTTP Parameter Pollution (HPP) si verifica quando un'applicazione riceve più parametri HTTP con lo stesso nome e li elabora in modo incoerente o insicuro. Fornendo parametri duplicati, un utente malintenzionato può manipolare la logica interna dell'applicazione, bypassando potenzialmente i Web Application Firewall (WAF), i filtri di validazione dell'input o i meccanismi di controllo degli accessi. Questa vulnerabilità dipende fortemente dal web server sottostante o dal framework dell'applicazione, che può scegliere la prima, l'ultima o una versione concatenata dei parametri ripetuti. Lo sfruttamento (Exploit) tipicamente prende di mira endpoint che trasmettono parametri a API di back-end, query al database o meccanismi di reindirizzamento URL, portando a impatti che vanno dal Cross-Site Scripting (XSS) all'esfiltrazione non autorizzata di dati.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### In che modo l'applicazione gestisce parametri HTTP multipli con lo stesso nome?
- [ ] No — i parametri duplicati vengono **rifiutati** o ignorati dal server  
- [ ] Sì — il server seleziona in modo coerente solo la **prima** o l'**ultima** occorrenza  
- [ ] Sì — il server **concatena** i valori, consentendo potenzialmente un'injection  

### L'HPP può essere sfruttato per bypassare controlli di sicurezza come un WAF o un filtro di input?
- [ ] No — i controlli di sicurezza ispezionano correttamente **tutte** le occorrenze di un parametro  
- [ ] Sì — un WAF o un filtro ispeziona solo la **prima** occorrenza, permettendo a una **seconda** occorrenza malevola di passare  
- [ ] Sì — la concatenazione consente a un utente malintenzionato di **suddividere** un payload su più parametri per eludere il rilevamento  

### L'applicazione riflette i parametri inquinati nella risposta senza una corretta sanificazione?
- [ ] No — i parametri vengono sanificati o codificati prima di essere riflessi nella UI  
- [ ] Sì — l'inquinamento produce contenuti riflessi, ma la sanificazione **viene applicata**  
- [ ] Sì — l'inquinamento produce contenuti riflessi e la sanificazione **non viene applicata**, portando a XSS o errori di logica  

### I parametri passati a chiamate API interne o sistemi di back-end sono vulnerabili all'inquinamento?
- [ ] No — le richieste di back-end utilizzano metodi di costruzione sicuri e non dinamici  
- [ ] Sì — l'HPP consente a un utente malintenzionato di **iniettare** o **sovrascrivere** parametri nella struttura della richiesta di back-end  

### L'applicazione mostra un comportamento diverso quando i parametri sono inquinati in richieste GET rispetto a richieste POST?
- [ ] No — il comportamento è coerente in tutti i metodi HTTP  
- [ ] Sì — solo i parametri GET sono vulnerabili all'inquinamento  
- [ ] Sì — solo i parametri POST (body) sono vulnerabili all'inquinamento  

---