## WSTG-CONF-05 — Enumerazione delle Interfacce di Amministrazione dell'Infrastruttura e dell'Applicazione

Le interfacce amministrative rappresentano il piano di controllo della gestione di un'applicazione e della sua infrastruttura sottostante, garantendo spesso l'accesso privilegiato alle configurazioni di sistema, ai dati degli utenti e alle operazioni lato server. Gli attaccanti danno la priorità all'enumerazione di queste interfacce attraverso il Brute Force delle directory, l'analisi del file `robots.txt` o l'osservazione di pattern URL prevedibili per trovare punti di ingresso che potrebbero mancare di un'autenticazione robusta o basarsi su credenziali predefinite. La scoperta di un pannello di amministrazione esposto aumenta significativamente il rischio di compromissione totale del sistema, modifica non autorizzata dei dati o interruzione del servizio. Queste interfacce si trovano frequentemente su porte non standard o sottodomini, rendendole bersagli primari per scanner automatizzati e ricognizione manuale.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Medio / Alto* |

> *La gravità diventa Alta se l'interfaccia consente modifiche alla configurazione a livello di sistema, gestione degli utenti o esfiltrazione di dati senza autenticazione a più fattori.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Le interfacce amministrative sono individuabili tramite directory Fuzzing o ricognizione?
- [ ] No — nessuna interfaccia amministrativa è esposta o individuabile  
- [ ] Sì — le interfacce sono individuabili ma l'accesso **è limitato** a intervalli IP interni  
- [ ] Sì — le interfacce **sono** individuabili e accessibili pubblicamente  

### L'autenticazione è obbligatoria sui portali amministrativi individuati?
- [ ] Sì — l'autenticazione a più fattori (MFA) **è obbligatoria**  
- [ ] Sì — l'autenticazione a fattore singolo **è obbligatoria**  
- [ ] No — non è richiesta alcuna autenticazione per accedere all'interfaccia *(Critico)*  

### I percorsi amministrativi sono nascosti o non standard per prevenirne l'individuazione?
- [ ] Sì — i percorsi utilizzano una denominazione non standard, casuale o non prevedibile  
- [ ] No — i percorsi utilizzano convenzioni di denominazione comuni (es. `/admin`, `/manager`, `/console`) e **possono** essere facilmente indovinati  

### L'interfaccia fornisce l'accesso a funzionalità sensibili del sistema o dell'applicazione?
- [ ] No — l'interfaccia è una dashboard di stato in "sola lettura" con impatto minimo  
- [ ] Sì — l'interfaccia **può** modificare la configurazione dell'applicazione o i dati degli utenti  
- [ ] Sì — l'interfaccia **può** eseguire comandi a livello di sistema o gestire il database  

---