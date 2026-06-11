## WSTG-INFO-02 — Fingerprinting del Web Server

Il fingerprinting del web server è il processo di identificazione del tipo di software specifico, della versione e del sistema operativo sottostante di un web server target. Gli attaccanti eseguono questa attività di ricognizione per identificare vulnerabilità note, come CVE non patchate o debolezze di configurazione, specifiche per determinate versioni di Apache, Nginx, IIS o altre tecnologie server. Ciò si ottiene tipicamente analizzando gli header di risposta HTTP (es. `Server`, `X-Powered-By`), esaminando comportamenti unici del protocollo o osservando informazioni trapelate attraverso pagine di errore e file predefiniti. Un fingerprinting efficace consente a un attaccante di personalizzare la propria strategia di Exploit, passando da una scansione generica ad attacchi ad alta probabilità contro difetti architetturali noti.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Informativo / Basso* |

> *La gravità diventa Alta se il fingerprinting rivela una versione del server obsoleta o a fine del ciclo di vita (EOL) con vulnerabilità critiche note.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Tool:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Sono presenti stringhe identificative negli header di risposta HTTP?
- [ ] No — gli header `Server` e `X-Powered-By` **non sono presenti** o contengono valori generici  
- [ ] Sì — gli header rivelano il software del server ma **non** i numeri di versione specifici  
- [ ] Sì — gli header rivelano il software specifico del server e i numeri di versione **esatti**  

### Le pagine di errore predefinite o le risposte generate dal sistema rivelano dettagli del server?
- [ ] No — vengono utilizzate pagine di errore personalizzate che **non** divulgano informazioni sul server  
- [ ] Sì — le pagine di errore predefinite rivelano il nome del software del server e/o la versione  
- [ ] Sì — le pagine di errore rivelano dettagli del server, la versione e il **sistema operativo sottostante**  

### La versione del server è divulgata tramite file predefiniti o strutture di directory?
- [ ] No — i file di installazione predefiniti, i manuali e gli script di test sono stati **rimossi**  
- [ ] Sì — i file predefiniti (es. `info.php`, `manual/`, `test.html`) sono **presenti** e divulgano dati sulla versione  

### È possibile dedurre accuratamente il tipo di server tramite l'analisi di comportamenti univoci?
- [ ] No — le risposte del server sono normalizzate e il fingerprinting basato sul comportamento **non è possibile**  
- [ ] Sì — comportamenti univoci (es. ordinamento degli header, codici di errore specifici o particolarità dello stack TCP/IP) **consentono** l'identificazione del server  

### La versione del server identificata è attualmente supportata e patchata?
- [ ] Sì — la versione identificata è attuale e **supportata** dal fornitore  
- [ ] No — la versione identificata è **obsoleta** o a **fine del ciclo di vita (EOL)** con vulnerabilità note  

---