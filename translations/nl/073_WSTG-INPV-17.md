## WSTG-INPV-17 — Testing for Host Header Injection

Host Header Injection treedt op wanneer een applicatie de door de gebruiker opgegeven `Host`-header vertrouwt en deze zonder voldoende validatie opneemt in de server-side logica, zoals bij het genereren van links of redirects. Aanvallers manipuleren deze header om Web Cache Poisoning te faciliteren, Password Reset Poisoning mogelijk te maken door gevoelige tokens naar een extern domein om te leiden, of om beveiligingscontroles te omzeilen die vertrouwen op routing op basis van headers. Succesvolle exploitatie stelt een aanvaller in staat om sessiegegevens te exfiltreren, account takeovers uit te voeren of nietsvermoedende gebruikers om te leiden naar malafide infrastructuur. Deze kwetsbaarheid wordt het meest aangetroffen in load-balanced omgevingen of applicaties die dynamisch absolute URL's genereren op basis van de inkomende request-context.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst (Severity)** | Medium / High* |

> *De ernst wordt High als de Host-header wordt gebruikt voor het genereren van gevoelige links in e-mails, zoals bij wachtwoordresets.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Tools:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### Reflecteert de applicatie de waarde van de `Host`-header in links, redirects of scripts?
- [ ] Nee — de applicatie gebruikt hardcoded base URL's of een strikt gevalideerde configuratie  
- [ ] Ja — de `Host`-header wordt gereflecteerd maar strikt gevalideerd tegen een whitelist  
- [ ] Ja — de `Host`-header wordt gereflecteerd en een bypass **is mogelijk** via misvormde waarden (bijv. `expected.com:attacker.com`)  
- [ ] Ja — de `Host`-header wordt gereflecteerd **zonder** enige validatie  

### Worden alternatieve host-gerelateerde headers verwerkt door de applicatie of infrastructuur?
- [ ] Nee — headers zoals `X-Forwarded-Host`, `X-Host` of `Forwarded` worden genegeerd  
- [ ] Ja — alternatieve headers worden verwerkt maar overschrijven de interne logica **niet**  
- [ ] Ja — alternatieve headers **kunnen** worden gebruikt om de primaire `Host`-headerwaarde te overschrijven  

### Kan de `Host`-header worden gemanipuleerd om door het systeem gegenereerde e-mails (bijv. wachtwoordresets) te vergiftigen?
- [ ] Nee — e-mail-links gebruiken een statisch, vertrouwd domein uit de serverconfiguratie  
- [ ] Ja — de `Host`-header beïnvloedt e-mail-links maar validatie **kan niet** worden omzeild  
- [ ] Ja — de `Host`-header **kan** worden gemanipuleerd om wachtwoord-reset-tokens om te leiden naar een door de aanvaller beheerd domein *(Critical)*  

### Is de applicatie vatbaar voor Web Cache Poisoning via de `Host`-header?
- [ ] Nee — er is geen caching-mechanisme aanwezig of de `Host`-header maakt deel uit van de cache key  
- [ ] Ja — er is een cache aanwezig maar deze valideert strikt of negeert de `Host`-header voor unkeyed inputs  
- [ ] Ja — er bestaat een cache en deze **kan** worden vergiftigd door een geïnjecteerde `Host`-header om malafide content aan andere gebruikers te serveren  

### Accepteert de server willekeurige `Host`-headers voor virtual host routing?
- [ ] Nee — de server retourneert een foutmelding of een standaardpagina voor niet-herkende `Host`-headers  
- [ ] Ja — de server accepteert elke `Host`-header, wat nuttig **is** voor reconnaissance of enumeratie van interne services  

---