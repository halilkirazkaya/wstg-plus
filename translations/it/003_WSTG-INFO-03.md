## WSTG-INFO-03 — Revisione dei Metafile del Webserver per Fughe di Informazioni (Information Leakage)

I metafile del server web come `robots.txt`, `sitemap.xml` e `security.txt` hanno lo scopo di guidare i crawler e fornire metadati amministrativi, ma spesso rivelano inavvertitamente strutture di directory sensibili o endpoint nascosti. Gli attaccanti analizzano questi file per enumerare la superficie di attacco dell'applicazione, identificando le direttive "Disallow" che spesso puntano a pannelli amministrativi non collegati, directory di backup o ambienti di sviluppo. Oltre alle istruzioni per i motori di ricerca, i file nella directory `.well-known/` o file come `humans.txt` possono rivelare stack tecnologici, informazioni sugli sviluppatori o contatti di sicurezza dell'organizzazione. Una revisione sistematica di questi file permette a un tester di ottenere approfondimenti strutturali e tecnici senza eseguire una scoperta tramite Brute Force aggressivo.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Stato del Test** | Non Eseguito |
| **Severità** | Bassa / Informativa* |

> *La severità diventa Media se i metafile rivelano interfacce amministrative non autenticate o backup di configurazioni sensibili.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Il file `robots.txt` esiste ed espone directory sensibili?
- [ ] No — `robots.txt` **non** esiste  
- [ ] Sì — `robots.txt` esiste ma contiene solo percorsi pubblici standard  
- [ ] Sì — `robots.txt` rivela percorsi di directory sensibili o interfacce amministrative  
- [ ] Sì — `robots.txt` rivela credenziali o versioni specifiche del software nei commenti  

### È presente un file `sitemap.xml` che rivela la struttura nascosta dell'applicazione?
- [ ] No — `sitemap.xml` **non** esiste  
- [ ] Sì — `sitemap.xml` è presente ma elenca solo URL pubblici  
- [ ] Sì — `sitemap.xml` elenca endpoint non collegati o risorse esclusivamente interne a cui **è possibile** accedere  

### I metafile di sicurezza e organizzativi espongono dettagli tecnici?
- [ ] No — nessun file di metadati trovato nella root o in `.well-known/`  
- [ ] Sì — `security.txt` o `humans.txt` sono presenti ma contengono informazioni standard  
- [ ] Sì — i metafile rivelano nomi host interni, identità degli sviluppatori o dettagli dello stack tecnologico che **possono** agevolare il Social Engineering o ulteriori attacchi  

### Sono presenti metafile legacy o specifici del framework?
- [ ] No — nessun file specifico del framework (es. `info.php`, `composer.json`, `package.json`) trovato  
- [ ] Sì — file come `README.md`, `CHANGELOG.txt` o `.DS_Store` sono presenti ma bonificati  
- [ ] Sì — sono presenti file specifici del framework o della versione che **permettono** un Fingerprinting tecnologico preciso  

---