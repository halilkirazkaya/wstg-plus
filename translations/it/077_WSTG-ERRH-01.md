## WSTG-ERRH-01 — Testing for Improper Error Handling

La gestione impropria degli errori si verifica quando un'applicazione web rivela dettagli tecnici sensibili—come stack trace, informazioni sullo schema del database o percorsi di file interni—attraverso le sue risposte di errore. Gli attaccanti innescano questi errori inviando input malformati, accedendo a risorse inesistenti o inducendo eccezioni lato server per mappare l'architettura sottostante dell'applicazione e identificare potenziali vettori per ulteriori exploit. Questa fuga di informazioni (Information Leakage) serve spesso come precursore di attacchi più gravi, inclusi SQL Injection o Path Traversal, fornendo all'attaccante precise specifiche sull'ambiente. Una implementazione sicura deve fornire messaggi generici e user-friendly, catturando la diagnostica dettagliata solo in log sicuri lato server.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### L'applicazione utilizza un gestore di errori generico e globale per le eccezioni non gestite?
- [ ] Sì — le pagine di errore generiche sono **abilitate** e non rivelano **alcun** dettaglio tecnico  
- [ ] Sì — vengono utilizzate pagine di errore personalizzate ma la fuga di informazioni **è possibile** tramite header specifici  
- [ ] No — le pagine di errore predefinite del server (es. Tomcat, IIS, Nginx) sono **visibili**  

### Le informazioni tecniche possono essere enumerate attraverso input malformati o casi limite?
- [ ] No — gli errori sono gestiti correttamente con ID di riferimento univoci per il logging  
- [ ] Sì — stack trace o query al database **vengono rivelati** nelle risposte  
- [ ] Sì — percorsi di file interni, variabili d'ambiente o stringhe di versione del server **vengono rivelati**  

### Le risposte delle API espongono oggetti di errore dettagliati o informazioni di debug?
- [ ] No — le API restituiscono codici di errore standardizzati e messaggi JSON bonificati  
- [ ] Sì — le API restituiscono oggetti di debug dettagliati o dettagli completi sull'eccezione nel corpo della risposta  
- [ ] Sì — gli stack trace sono inclusi nei campi `details` o `exception` della risposta JSON  

### L'applicazione si comporta in modo differente (Time-based o Content-based) quando si verificano errori?
- [ ] No — le firme delle risposte e le tempistiche sono coerenti indipendentemente dal tipo di errore  
- [ ] Sì — le risposte differenziali **possono** essere utilizzate per enumerare stati validi vs non validi (es. User Enumeration)  

---