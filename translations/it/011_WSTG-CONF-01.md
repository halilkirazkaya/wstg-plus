## WSTG-CONF-01 — Test della configurazione dell'infrastruttura di rete

Il test della configurazione dell'infrastruttura di rete comporta l'identificazione di debolezze nei componenti server e di rete sottostanti che supportano l'applicazione web. Gli attaccanti prendono di mira servizi malconfigurati, protocolli obsoleti e porte aperte non necessarie per ottenere un punto d'appoggio, muoversi lateralmente o eseguire attività di reconnaissance. Le evidenze comuni includono versioni TLS insicure, banner di servizio eccessivamente dettagliati (verbose) e interfacce amministrative esposte che dovrebbero essere limitate alle reti interne. Sfruttando queste falle a livello di infrastruttura, un avversario può compromettere la riservatezza dei dati in transito o ottenere l'accesso non autorizzato al sistema operativo host.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Stato del Test** | Non eseguito |
| **Gravità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Tool:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Sono presenti porte aperte non necessarie o servizi esposti sull'host dell'applicazione?
- [ ] No — solo le porte richieste (es. 443) sono **accessibili**  
- [ ] Sì — sono esposti servizi aggiuntivi, ma seguono una configurazione **sicura**  
- [ ] Sì — servizi non essenziali (es. FTP, Telnet, SMB) sono **esposti** pubblicamente  

### I banner o gli header dei servizi espongono informazioni sensibili sulla versione?
- [ ] No — i banner sono rimossi o generici  
- [ ] Sì — i banner rivelano il software del server (es. Apache/nginx) ma **non** i numeri di versione  
- [ ] Sì — i banner descrittivi (verbose) rivelano versioni software specifiche, facilitando l'**exploitation**  

### La configurazione TLS segue le moderne best practice di sicurezza?
- [ ] Sì — solo TLS 1.2/1.3 sono **abilitati** con cipher suites forti  
- [ ] Sì — i protocolli legacy (TLS 1.0/1.1) sono **abilitati**  
- [ ] No — cipher deboli o protocolli insicuri (SSLv2/v3) sono **abilitati**  

### Le interfacce amministrative o di gestione sono limitate a reti autorizzate?
- [ ] Sì — le interfacce (es. SSH, RDP, pannelli di controllo) **non sono accessibili** dalla rete internet pubblica  
- [ ] No — le interfacce sono pubblicamente **accessibili** ma richiedono l'autenticazione a più fattori (MFA)  
- [ ] No — le interfacce sono pubblicamente **accessibili** con autenticazione a fattore singolo o credenziali di default  

---