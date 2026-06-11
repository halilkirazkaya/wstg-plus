## WSTG-INPV-10 — IMAP SMTP Injection

Le vulnerabilità di IMAP e SMTP injection si verificano quando un'applicazione web filtra in modo improprio i dati forniti dall'utente prima di incorporarli nei comandi inviati a un server di posta. Iniettando i caratteri Carriage Return e Line Feed (CRLF), un attaccante può terminare il comando previsto e aggiungere istruzioni e-mail arbitrarie, come l'aggiunta di destinatari supplementari (CC/BCC) o la modifica del corpo del messaggio. Questi difetti si riscontrano più frequentemente nei gateway web-to-mail, nei moduli di contatto e nei client di posta elettronica che comunicano direttamente con i server di posta tramite protocolli come IMAP4 o SMTP. Lo sfruttamento con successo di queste vulnerabilità consente il relay non autorizzato di spam, l'esfiltrazione di contenuti e-mail sensibili e, in alcuni casi, la totale compromissione dell'integrità del server di posta.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### L'applicazione elabora l'input fornito dall'utente per costruire comandi IMAP o SMTP?
- [ ] No — l'applicazione **non** interagisce con i server di posta tramite input dell'utente  
- [ ] Sì — l'applicazione interagisce con i server di posta ma utilizza una API o una libreria sicura  
- [ ] Sì — l'applicazione costruisce comandi di posta grezzi utilizzando parametri controllati dall'utente  

### L'input viene validato per impedire l'iniezione di sequenze Carriage Return (`\r`) e Line Feed (`\n`)?
- [ ] Sì — i caratteri CRLF sono **rigorosamente** filtrati, codificati o rifiutati  
- [ ] Sì — l'input è validato rispetto a una allowlist rigorosa di caratteri  
- [ ] No — i caratteri CRLF **non** vengono filtrati, consentendo la terminazione e l'iniezione dei comandi  

### Un attaccante può iniettare header di posta aggiuntivi come `Bcc:` o `Cc:`?
- [ ] No — la modifica degli header **non è possibile** a causa di rigorosi vincoli sull'input  
- [ ] Sì — l'iniezione di header aggiuntivi **è possibile** tramite sequenze CRLF  
- [ ] Sì — il relay di posta verso domini esterni **è abilitato** ed esploitabile *(Critico)*  

### L'applicazione consente la manipolazione dei comandi IMAP per accedere a caselle di posta non autorizzate?
- [ ] No — l'applicazione **non** utilizza IMAP o limita rigorosamente l'accesso alla casella di posta all'utente autenticato  
- [ ] Sì — l'iniezione di comandi IMAP **è possibile** ma limitata all'enumerazione dei metadati  
- [ ] Sì — l'iniezione di comandi IMAP consente la lettura o l'eliminazione delle e-mail di altri utenti  

### Il server di posta backend è vulnerabile al command pipelining?
- [ ] No — il server di posta rifiuta comandi raggruppati o comandi multipli in una singola sessione  
- [ ] Sì — il server di posta elabora comandi multipli iniettati tramite un singolo campo di input  

---