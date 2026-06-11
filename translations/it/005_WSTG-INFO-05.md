## WSTG-INFO-05 — Revisione del Contenuto della Pagina Web per Information Leakage

La revisione del contenuto della pagina web comporta l'ispezione manuale e automatizzata delle risposte dell'applicazione, inclusi i file HTML, JavaScript e CSS, per identificare informazioni sensibili involontariamente esposte agli utenti. Gli attaccanti esaminano il codice sorgente client-side alla ricerca di commenti degli sviluppatori, campi di input nascosti, percorsi interni del server, chiavi API (hardcoded API keys) e informazioni di debugging che possono favorire successive fasi di exploitation. Questa fuga di informazioni (leakage) si verifica spesso quando gli sviluppatori dimenticano di rimuovere metadati o codice di debug prima del passaggio in produzione, rivelando potenzialmente difetti logici o dettagli dell'infrastruttura backend. Dal punto di vista di un attaccante, questo rappresenta un metodo a basso sforzo per mappare la struttura interna dell'applicazione e scoprire punti di ingresso trascurati senza attivare avvisi di sicurezza o interagire con la logica backend del server.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Media* |

> *La severità diventa Alta se vengono scoperte credenziali hardcoded sensibili, chiavi private o variabili d'ambiente interne.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Strumenti:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Sono presenti commenti degli sviluppatori contenenti informazioni sensibili nel codice sorgente?
- [ ] No — nessun commento trovato o presenti solo commenti generici e non sensibili  
- [ ] Sì — i commenti esistono ma **non** contengono logica o dati sensibili  
- [ ] Sì — i commenti **rivelano** percorsi interni, query SQL o istruzioni amministrative  
- [ ] Sì — i commenti **rivelano** credenziali o note sensibili degli sviluppatori *(Alta)*  

### I campi di input HTML nascosti contengono dati sensibili o informazioni di stato?
- [ ] No — nessun campo nascosto trovato o contengono solo token non sensibili  
- [ ] Sì — i campi nascosti sono usati per gestire lo stato ma sono **cifrati** o **firmati**  
- [ ] Sì — i campi nascosti contengono password in **testo in chiaro (plaintext)**, ruoli utente o ID interni  

### Sono presenti segreti hardcoded, chiavi API o indirizzi IP interni nei file JavaScript?
- [ ] No — nessuna stringa sensibile o segreto trovato nei file JS  
- [ ] Sì — i file JS contengono chiavi API pubbliche con restrizioni **adeguate**  
- [ ] Sì — i file JS contengono chiavi API **non protette**, IP interni o endpoint privati  
- [ ] Sì — i file JS contengono credenziali amministrative **hardcoded** o chiavi private *(Critica)*  

### L'applicazione rivela versioni del framework o percorsi di file interni nel sorgente?
- [ ] No — le informazioni sul framework e i percorsi dei file sono **minimizzati (minified)** o **offuscati**  
- [ ] Sì — i nomi e le versioni del framework sono **visibili** ma aggiornati (patched)  
- [ ] Sì — percorsi di file interni o percorsi assoluti del server sono **esposti** nel codice sorgente  

### Il codice sorgente è incline alla perdita di dati a causa della mancanza di minimizzazione o offuscamento?
- [ ] No — il codice di produzione è **completamente minimizzato** e privo di commenti  
- [ ] Sì — il codice è minimizzato ma i commenti **rimangono** nel sorgente  
- [ ] Sì — le source maps (file `.map`) sono **accessibili pubblicamente**, consentendo la ricostruzione completa del codice sorgente  

---