## WSTG-ERRH-02 — Testing for Stack Traces

Le stack trace vengono generate quando un'applicazione non riesce a gestire correttamente un'eccezione, con la conseguente divulgazione dello stato di esecuzione interno e della gerarchia delle chiamate all'utente finale. Per un penetration tester, queste tracce sono inestimabili in quanto rivelano il framework dell'applicazione, le versioni delle librerie, i percorsi del file system interno e il flusso logico, riducendo significativamente l'impegno richiesto per la ricognizione. L'exploitation consiste nel provocare intenzionalmente errori tramite input malformati, tipi di dati imprevisti o violazioni delle condizioni al contorno per forzare l'applicazione in uno stato instabile ed esfiltrare informazioni sullo stack tecnologico sottostante. Questa fuga di informazioni fornisce spesso il contesto necessario per identificare componenti di terze parti vulnerabili o per creare exploit specifici per difetti logici scoperti all'interno dei percorsi di codice divulgati.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media* |

> *La gravità aumenta a Media se la stack trace rivela dettagli architetturali sensibili, query al database o parametri di configurazione interni.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Vengono restituite stack trace complete al client in caso di errori dell'applicazione?
- [ ] No — l'applicazione restituisce pagine di errore generiche o messaggi di errore personalizzati  
- [ ] Sì — le stack trace sono **abilitate** ma solo per moduli specifici e non sensibili  
- [ ] Sì — le stack trace complete sono **abilitate** e visibili nel corpo della risposta HTTP  

### I messaggi di errore divulgano informazioni sensibili sull'ambiente?
- [ ] No — i messaggi di errore non contengono dettagli tecnici o ambientali  
- [ ] Sì — i messaggi rivelano **percorsi di file interni** o **strutture di directory** lato server  
- [ ] Sì — i messaggi rivelano **schemi di database**, **query SQL** o **versioni di librerie di terze parti**  

### È possibile generare stack trace manipolando i parametri di input?
- [ ] No — l'input malformato viene gestito correttamente o rifiutato dalla validazione  
- [ ] Sì — le tracce possono essere generate da **tipi di dati imprevisti** (es. invio di un array dove è prevista una stringa)  
- [ ] Sì — le tracce sono generate da **null byte**, **caratteri speciali** o **valori limite**  

### Viene applicato in modo coerente un gestore globale degli errori in tutta l'applicazione?
- [ ] Sì — un gestore di eccezioni globale è **applicato coerentemente** su tutti gli endpoint testati  
- [ ] Sì — è presente un gestore ma **non è applicato** a endpoint legacy, API o aggiunti di recente  
- [ ] No — non è stato **rilevato** alcun meccanismo di gestione globale degli errori, con conseguenti errori predefiniti del server  

---