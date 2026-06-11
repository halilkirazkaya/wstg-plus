## WSTG-CONF-09 — Testen van Bestandsrechten

Het testen van bestandsrechten omvat het auditen van de toegangscontroleniveaus die zijn toegewezen aan bestanden en mappen op de webserver om te garanderen dat gevoelige bronnen niet overmatig worden blootgesteld aan ongeautoriseerde gebruikers of processen. Onjuist geconfigureerde rechten kunnen aanvallers in staat stellen om gevoelige configuratiebestanden met database-credentials te lezen, propriëtaire broncode in te zien of server-side scripts aan te passen om Remote Code Execution (RCE) te bewerkstelligen. Deze kwetsbaarheid treedt doorgaans op tijdens de implementatiefase wanneer standaard "world-readable" of "world-writable" flags achterblijven op kritieke applicatiemappen, zoals configuratiepaden, logmappen of opslagplaatsen voor uploads. Vanuit het perspectief van een aanvaller is het ontdekken van een onjuist beveiligd bestand vaak de primaire katalysator voor Privilege Escalation of een volledige systeemovername.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als gevoelige configuratiebestanden (bijv. `.env`, `web.config`) leesbaar zijn of als schrijftoegang tot de web root mogelijk is.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Tools:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Zijn gevoelige applicatieconfiguratiebestanden beschermd tegen ongeautoriseerde toegang?
- [ ] Ja — configuratiebestanden (bijv. `.env`, `config.php`) zijn **niet toegankelijk** via webverzoeken  
- [ ] Ja — bestanden zijn aanwezig, maar toegang is **uitsluitend beperkt** tot geautoriseerde lokale gebruikers  
- [ ] Nee — gevoelige configuratiebestanden **zijn toegankelijk** en onthullen credentials of geheimen  

### Kan een aanvaller bestanden of mappen binnen de web root wijzigen?
- [ ] Nee — alle web root-mappen zijn **read-only** voor de webserver-gebruiker  
- [ ] Ja — er bestaan mappen met schrijfrechten, maar scriptuitvoering is **uitgeschakeld**  
- [ ] Ja — er bestaan world-writable mappen die het uploaden en uitvoeren van scripts **toestaan** *(Kritiek)*  

### Zijn metadata van versiebeheer of back-upbestanden blootgesteld door lakse rechten?
- [ ] Nee — `.git`, `.svn`, en back-upbestanden (bijv. `.bak`, `~`) zijn **niet aanwezig** of zijn beschermd  
- [ ] Ja — metadata-mappen bestaan, maar directory listing is **uitgeschakeld**  
- [ ] Ja — gevoelige metadata of back-ups zijn **volledig toegankelijk**, wat herstel van de broncode mogelijk maakt  

### Werkt de webserver-gebruiker volgens het principe van de minste privileges (Least Privilege)?
- [ ] Ja — het webserver-proces draait als een **specifieke gebruiker met lage privileges**  
- [ ] Nee — het webserver-proces draait met **onnodige privileges** (bijv. `root` of `Administrator`)  

### Zijn logbestanden beveiligd tegen ongeautoriseerd lezen of wijzigen?
- [ ] Ja — logbestanden zijn **beperkt** tot administratieve gebruikers  
- [ ] Ja — logbestanden zijn leesbaar, maar bevatten **geen** sessietokens of PII  
- [ ] Nee — logbestanden zijn **publiekelijk leesbaar** en onthullen gevoelige transactiegegevens of gebruikersinformatie  

---