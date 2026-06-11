## WSTG-CONF-02 — Test Application Platform Configuration

Il test della configurazione della piattaforma applicativa prevede l'audit del web server sottostante, dell'application server e delle impostazioni del framework per garantire che siano protetti contro exploit comuni. Gli attaccanti cercano attivamente credenziali di default, vulnerabilità della piattaforma non patchate e interfacce amministrative esposte per ottenere accessi non autorizzati o eseguire codice. Le configurazioni errate spesso comportano la divulgazione di variabili d'ambiente sensibili, percorsi di sistema interni o la presenza di applicazioni di esempio non necessarie che espandono la superficie di attacco. Da una prospettiva professionale, una piattaforma configurata in modo errato funge da punto di ingresso ad alta probabilità per ottenere l'accesso iniziale all'ambiente target.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se le interfacce amministrative sono accessibili con credenziali di default o se gli script di esempio consentono la Remote Code Execution.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Le credenziali o gli account di default sono attivi sulla piattaforma?
- [ ] No — tutti gli account di default sono **disabilitati** o le password sono state modificate  
- [ ] Sì — esistono account di default ma **non sono accessibili** dalla rete esterna  
- [ ] Sì — gli account di default sono **attivi** e accessibili tramite internet *(Critico)*  

### La piattaforma divulga informazioni sensibili sulla versione tramite header o pagine di errore?
- [ ] No — le stringhe di versione sono **nascoste** o generiche  
- [ ] Sì — informazioni dettagliate sulla versione **vengono divulgate** negli HTTP headers (es., `Server`, `X-Powered-By`)  
- [ ] Sì — stack traces completi e percorsi di sistema interni **vengono esposti** in messaggi di errore dettagliati  

### Le interfacce amministrative o di gestione sono correttamente limitate?
- [ ] No — le interfacce amministrative **non sono esposte**  
- [ ] Sì — le interfacce esistono ma sono **limitate** tramite IP allowlisting o Multi-Factor Authentication  
- [ ] Sì — le interfacce (es., `/admin`, `/manager`, `/console`) sono **accessibili pubblicamente** con autenticazione a singolo fattore  

### Sono presenti file di esempio, script o documentazione sul server di produzione?
- [ ] No — tutti i file non essenziali sono stati **rimossi**  
- [ ] Sì — file di esempio o documentazione **sono presenti** ma non espongono informazioni sensibili  
- [ ] Sì — script di esempio (es., `info.php`, `examples/`) **sono presenti** e forniscono un vettore di attacco diretto  

### Il server è configurato per supportare metodi HTTP insicuri?
- [ ] No — solo `GET` e `POST` sono **abilitati**  
- [ ] Sì — metodi potenzialmente rischiosi come `PUT`, `DELETE` o `TRACE` sono **abilitati** ma limitati  
- [ ] Sì — i metodi rischiosi sono **abilitati** e consentono la modifica non autorizzata di file o il furto di credenziali  

---