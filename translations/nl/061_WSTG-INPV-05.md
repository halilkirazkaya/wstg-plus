## WSTG-INPV-05 — SQL Injection

SQL Injection vindt plaats wanneer door de gebruiker ingevoerde gegevens worden opgenomen in SQL-queries zonder de juiste parameterisering of validatie (sanitization), waardoor aanvallers de query-logica kunnen manipuleren. Succesvolle exploitatie kan leiden tot ongeautoriseerde data-exfiltratie, authentication bypass, datamodificatie en in sommige gevallen remote code execution via databasefuncties zoals `xp_cmdshell` of `LOAD_FILE()`. Deze kwetsbaarheid treedt vaak op bij inlogformulieren, zoekfunctionaliteiten, REST API-parameters, HTTP-headers en elk eindpunt dat dynamische SQL-queries opbouwt op basis van gebruikersinvoer. Vanuit het perspectief van een aanvaller is het identificeren van een injectiepunt de primaire toegangspoort tot volledige database-compromittering en mogelijke lateral movement binnen de infrastructuur.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Tools:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Worden door de gebruiker beïnvloedbare parameters verwerkt met behulp van veilige databasetoegangsmethoden?
- [ ] Ja — de applicatie maakt uitsluitend gebruik van ORM of geparameteriseerde queries en een bypass is **niet mogelijk**  
- [ ] Ja — er worden geparameteriseerde queries gebruikt, maar een bypass **is mogelijk** via randgevallen (bijv. `ORDER BY`-clausules)  
- [ ] Nee — er wordt gebruikgemaakt van directe string-concatenatie om SQL-queries op te bouwen **zonder** parameterisering  

### Kan het authenticatiemechanisme worden omzeild via SQL injection?
- [ ] Nee — inlogparameters zijn **niet** kwetsbaar voor injectie  
- [ ] Ja — authentication bypass **is mogelijk** met behulp van op tautologie gebaseerde payloads (bijv. `' OR 1=1 --`)  

### Is data-exfiltratie mogelijk via in-band of out-of-band technieken?
- [ ] Nee — geen data-exfiltratie geïdentificeerd  
- [ ] Ja — UNION-based of error-based exfiltratie **is mogelijk**  
- [ ] Ja — blind (Boolean of Time-based) exfiltratie **is mogelijk**  
- [ ] Ja — out-of-band (DNS/HTTP) exfiltratie **is mogelijk**  

### Vertoont de applicatiefunctionaliteit second-order SQL injection kwetsbaarheden?
- [ ] Nee — opgeslagen gegevens worden veilig verwerkt wanneer ze worden opgehaald voor latere queries  
- [ ] Ja — gebruikersinvoer wordt opgeslagen en later gebruikt in een onveilige SQL-query **zonder** validatie  

### Zijn de rechten van het database-serviceaccount beperkt tot het minimaal vereiste?
- [ ] Ja — het serviceaccount heeft **least privilege** (bijv. beperkt tot specifieke tabellen/acties)  
- [ ] Nee — het serviceaccount heeft verhoogde privileges (bijv. `DROP TABLE`, `FILE`-privileges of `xp_cmdshell` is **ingeschakeld**)  

---