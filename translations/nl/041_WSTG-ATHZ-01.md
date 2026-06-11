## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Directory traversal en file inclusion kwetsbaarheden ontstaan wanneer een applicatie door de gebruiker beheersbare invoer gebruikt om paden naar bestanden of mappen te construeren zonder voldoende validatie of opschoning (sanitization). Aanvallers misbruiken deze gebreken door reeksen zoals `../` te injecteren om buiten de beoogde map te navigeren, wat mogelijk toegang geeft tot gevoelige systeembestanden, configuratiegegevens of de broncode van de applicatie. In ernstigere gevallen met Local File Inclusion (LFI) of Remote File Inclusion (RFI), kan een aanvaller Remote Code Execution (RCE) bewerkstelligen door kwaadaardige scripts in te sluiten of gebruik te maken van log poisoning en PHP wrappers. Deze kwetsbaarheden bevinden zich doorgaans in parameters die worden gebruikt voor het dynamisch laden van inhoud, template engines of image retrieval endpoints waar de logica aan de serverzijde bestandssysteempaden verwerkt.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Tools:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Accepteren parameters bestandsnamen of paden voor verwerking aan de serverzijde?
- [ ] Nee — geen enkele parameter lijkt interactie te hebben met het bestandssysteem  
- [ ] Ja — er bestaan parameters, maar deze maken gebruik van een strikte allowlist van bestandsidentificatoren  
- [ ] Ja — parameters accepteren directe bestandsnamen of paden  

### Wordt de invoer opgeschoond om Directory Traversal reeksen te voorkomen?
- [ ] Ja — invoer wordt gevalideerd tegen een strikte allowlist en traversal reeksen zijn **niet mogelijk**  
- [ ] Ja — invoer wordt opgeschoond door `../` reeksen te verwijderen, maar een recursieve bypass **is mogelijk**  
- [ ] Nee — er wordt **geen** opschoning of validatie toegepast op pad-gerelateerde invoer  

### Is het mogelijk om via traversal reeksen toegang te krijgen tot bestanden buiten de beperkte map?
- [ ] Nee — de applicatie of het OS voorkomt toegang tot bestanden buiten de gedefinieerde scope  
- [ ] Ja — toegang tot bestanden binnen de web root **is mogelijk**  
- [ ] Ja — toegang tot gevoelige systeembestanden (bijv. `/etc/passwd`, `C:\Windows\win.ini`) **is mogelijk** *(Kritiek)*  

### Staat de server het insluiten van externe URL's toe (RFI)?
- [ ] Nee — Remote File Inclusion is **uitgeschakeld** op server-/applicatieniveau  
- [ ] Ja — externe bestanden **kunnen** worden ingesloten, maar uitvoering **is niet mogelijk**  
- [ ] Ja — externe bestanden **kunnen** worden ingesloten en uitgevoerd op de server *(Kritiek)*  

### Kunnen filters worden omzeild met behulp van codering of speciale tekens?
- [ ] Nee — filters verwerken effectief verschillende coderingen en null bytes  
- [ ] Ja — bypass **is mogelijk** met behulp van URL-encoding, dubbele URL-encoding of 16-bit Unicode  
- [ ] Ja — bypass **is mogelijk** met behulp van null byte injectie (`%00`) of filesystem wrappers (bijv. `php://filter`)  

---