## WSTG-ATHN-02 — Testing for Default Credentials

Il test per le credenziali predefinite (Default Credentials) consiste nell'identificare interfacce amministrative, servizi o componenti applicativi che utilizzano ancora nomi utente e password impostati in fabbrica o forniti dal produttore. Questa vulnerabilità rappresenta un obiettivo ad alta priorità per gli attaccanti poiché spesso fornisce un percorso immediato verso la compromissione totale del sistema, l'accesso amministrativo o la Remote Code Execution con un impegno minimo. Si verifica tipicamente in software standard (off-the-shelf), piattaforme CMS, strumenti di gestione di database e hardware integrato come dispositivi IoT o appliance di rete. L'Exploit viene solitamente eseguito confrontando le versioni del software identificate con database pubblici di password predefinite o utilizzando strumenti automatizzati di Credential Stuffing durante le fasi iniziali di ricognizione e sfruttamento.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Critica / Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Strumenti:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Le interfacce amministrative o di gestione sono esposte su Internet o su segmenti di rete non attendibili?
- [ ] No — nessuna interfaccia di gestione è accessibile  
- [ ] Sì — le interfacce sono presenti ma limitate da controlli a livello di rete (es. VPN, IP Allowlist)  
- [ ] Sì — le interfacce **sono** accessibili pubblicamente e richiedono autenticazione  

### Le credenziali predefinite fornite dal fornitore sono state modificate per tutti i componenti identificati?
- [ ] Sì — tutte le credenziali predefinite **sono state modificate** in tutti i componenti *(Massima sicurezza)*  
- [ ] Sì — le credenziali **sono state modificate** per l'applicazione principale, ma i componenti secondari (es. CMS, DB) mantengono quelle predefinite  
- [ ] No — le credenziali predefinite **sono attive** su una o più interfacce identificabili *(Critico)*  

### L'applicazione impone il cambio della password al primo accesso per i nuovi account o per gli account amministrativi?
- [ ] Sì — il cambio della password **è obbligatorio** prima di poter eseguire qualsiasi azione amministrativa  
- [ ] No — l'applicazione **consente** l'uso delle credenziali predefinite a tempo indeterminato  

### I meccanismi di blocco (lockout) o i controlli di Rate Limiting sono attivi per prevenire il test automatizzato delle credenziali predefinite?
- [ ] Sì — viene applicato un Rate Limiting aggressivo o il blocco dell'account (lockout)  
- [ ] Sì — i controlli sono presenti ma il bypass **è possibile** tramite manipolazione degli header o rotazione degli IP  
- [ ] No — non è abilitato alcun meccanismo di Rate Limiting o di blocco  

### I plugin di terze parti, i middleware o gli strumenti amministrativi (es. phpMyAdmin, Tomcat Manager) utilizzano credenziali univoche?
- [ ] No — non esistono componenti di terze parti nell'ambiente  
- [ ] Sì — tutti i componenti di terze parti utilizzano credenziali univoche e non predefinite  
- [ ] No — almeno un componente di terze parti **è accessibile** tramite credenziali predefinite note  

---