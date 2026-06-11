## WSTG-BUSL-01 — Test Business Logic Data Validation

Il testing della validazione dei dati della Business Logic si concentra sulla capacità dell'applicazione di identificare e rifiutare dati sintatticamente corretti ma logicamente non validi nel contesto del dominio di business. Gli attaccanti sfruttano queste falle inviando valori inaspettati—come quantità negative in un carrello della spesa, date nel passato per eventi futuri o interi estremamente grandi—per eludere i vincoli e manipolare lo stato dell'applicazione. Queste vulnerabilità risiedono spesso in workflow multi-step, processi di checkout e moduli di gestione dell'account dove la logica lato server non riesce a verificare l'integrità contestuale dei dati forniti dall'utente. Uno sfruttamento (Exploit) riuscito può portare a perdite finanziarie, compromissione dell'integrità e accesso non autorizzato a funzionalità o dati riservati.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### L'applicazione impone limiti di intervallo logico sugli input numerici?
- [ ] Sì — l'applicazione rifiuta correttamente valori negativi, zero o eccessivamente grandi dove inappropriato *(Più sicuro)*  
- [ ] Sì — l'applicazione rifiuta alcuni intervalli non validi ma **è** vulnerabile a integer overflow o bypass dei limiti (boundary bypass)  
- [ ] No — l'applicazione accetta qualsiasi valore numerico indipendentemente dal contesto di business *(Critico)*  

### I controlli di validazione lato server sono coerenti in tutti i layer dell'applicazione?
- [ ] Sì — la validazione è applicata in modo coerente sia a livello API/servizio che a livello database  
- [ ] Sì — la validazione **è applicata** nella UI/Frontend ma il bypass tramite richiesta API diretta **è possibile**  
- [ ] No — la validazione **non è applicata** o è incoerente, consentendo la corruzione logica dei dati  

### La business logic può essere sovvertita manipolando i dati in workflow multi-step?
- [ ] No — lo stato è mantenuto in modo sicuro sul server e i dati dei passaggi precedenti **non possono** essere manomessi  
- [ ] Sì — la validazione avviene nei primi passaggi ma l'invio finale **non** verifica nuovamente l'integrità dei dati  
- [ ] Sì — il bypass del workflow **è possibile** saltando passaggi o inviando dati fuori ordine  

### L'applicazione gestisce correttamente i dati relativi a "edge case" che eludono i vincoli di business?
- [ ] Sì — i vincoli sono robusti e gestiscono input insoliti ma sintatticamente validi (ad es. anni bisestili, cambi di fuso orario)  
- [ ] Sì — alcuni casi limite (edge case) sono gestiti ma combinazioni specifiche **possono** portare a comportamenti inaspettati  
- [ ] No — i casi limite portano direttamente a fallimenti logici, come acquisti gratuiti o cambi di stato non autorizzati  

---