## WSTG-INFO-04 — Enumerazione delle Applicazioni sul Web Server

L'enumerazione delle applicazioni è il processo di identificazione di tutte le applicazioni e i servizi web ospitati su un singolo web server o indirizzo IP. Poiché i moderni web server ospitano spesso molteplici host virtuali, ambienti di staging legacy o interfacce amministrative su porte e percorsi non standard, la mancata identificazione di questi asset nascosti lascia una parte significativa della superficie di attacco non testata. Gli attaccanti utilizzano tecniche come i DNS zone transfers, il brute-forcing degli host virtuali e i reverse IP lookups per scoprire questi punti di ingresso dimenticati o oscurati, che sono spesso meno sicuri dell'applicazione di produzione principale.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Informativa / Bassa |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Strumenti:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Sono associati più indirizzi IP o alias DNS all'infrastruttura di destinazione?
- [ ] No — esistono solo un singolo IP e un record A  
- [ ] Sì — sono stati identificati più indirizzi IP o alias, ampliando il perimetro  

### Il web server ospita più Host Virtuali (vhosts) sullo stesso IP?
- [ ] No — solo il dominio principale è servito dal server  
- [ ] Sì — sono stati identificati host virtuali aggiuntivi, ma l'accesso **non è possibile** (es. 403 Forbidden)  
- [ ] Sì — sono stati identificati e risultano accessibili host virtuali nascosti, di sviluppo o di staging  

### Le applicazioni sono ospitate su porte non standard?
- [ ] No — sono aperte solo le porte standard (80/443)  
- [ ] Sì — le porte non standard sono aperte ma **non** ospitano applicazioni web  
- [ ] Sì — sono state identificate applicazioni web aggiuntive su porte non standard (es. 8080, 8443, 9000)  

### Sono mappate applicazioni distinte su percorsi URI specifici?
- [ ] No — una singola applicazione serve l'intera directory radice (root)  
- [ ] Sì — sono state scoperte più applicazioni tramite l'enumerazione delle directory (es. `/api`, `/portal`, `/backup`)  

### I servizi di ricognizione di terze parti rivelano applicazioni storiche o secondarie?
- [ ] No — nessun reperto significativo da Shodan, Censys o Wayback Machine  
- [ ] Sì — snapshot storici o scansioni dei servizi rivelano applicazioni attualmente attive ma non collegate direttamente  

---