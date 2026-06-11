## WSTG-INFO-06 — Identificeren van applicatie-invoerpunten

Het identificeren van applicatie-invoerpunten omvat het in kaart brengen van het volledige aanvalsoppervlak door alle mogelijke manieren waarop een gebruiker of extern systeem met de applicatie kan communiceren op te sommen. Dit proces omvat het ontdekken van alle URL's, API-endpoints, parameters (GET, POST, cookie-based), HTTP-headers en gespecialiseerde protocollen zoals WebSockets of gRPC. Voor een penetratietester is deze fase van vitaal belang omdat kwetsbaarheden vaak worden gevonden in ongedocumenteerde "shadow" API's, legacy-endpoints of complexe parameterstructuren die tijdens de ontwikkeling over het hoofd zijn gezien. Grondige enumeratie zorgt ervoor dat geen enkel onderdeel van de applicatie een ongeteste black box blijft, waardoor wordt voorkomen dat aanvallers niet-gecontroleerde routes naar het systeem vinden.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Informatief |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Zijn alle GET- en POST-parameters binnen de applicatie geënumereerd?
- [ ] Ja — uitgebreide enumeratie via geautomatiseerde en handmatige crawling **is voltooid**  
- [ ] Ja — alleen algemene parameters zijn geïdentificeerd via standaard crawling  
- [ ] Nee — parameters zijn grotendeels **niet geïdentificeerd** of ongetest  

### Wordt er gezocht naar ongedocumenteerde of verborgen parameters (bijv. debug, admin) met behulp van fuzzing?
- [ ] Nee — het ontdekken van verborgen parameters is **niet noodzakelijk** voor deze specifieke scope  
- [ ] Ja — parameter fuzzing (bijv. met `Arjun`) **is uitgevoerd** zonder bevindingen  
- [ ] Ja — verborgen parameters **zijn ontdekt** en in kaart gebracht voor verdere testen  
- [ ] Nee — het ontdekken van verborgen parameters **is niet** uitgevoerd  

### Zijn alle ondersteunde HTTP-methoden (PUT, DELETE, PATCH, etc.) geïdentificeerd voor elk endpoint?
- [ ] Ja — methode-ontdekking **is toegepast** op alle ontdekte endpoints  
- [ ] Ja — methode-ontdekking **is alleen toegepast** op endpoints met een hoog risico of gevoelige endpoints  
- [ ] Nee — alleen standaard GET- en POST-methoden **zijn geïdentificeerd**  

### Zijn niet-standaard invoerpunten zoals WebSockets, gRPC of aangepaste headers in kaart gebracht?
- [ ] Nee — applicatie **maakt geen gebruik** van niet-standaard protocollen of aangepaste headers  
- [ ] Ja — alle protocolspecifieke invoerpunten en aangepaste headers **zijn geïdentificeerd**  
- [ ] Nee — niet-standaard invoerpunten **bestaan**, maar zijn niet in kaart gebracht  

### Is het API-oppervlak (inclusief geversioneerde endpoints zoals /v1/ of /v2/) volledig in kaart gebracht?
- [ ] Nee — applicatie **stelt geen** API beschikbaar  
- [ ] Ja — alle API-versies en endpoints **zijn geïdentificeerd** en gedocumenteerd  
- [ ] Ja — alleen de huidige productie-API-versie **is geïdentificeerd**  
- [ ] Nee — API-endpoints **blijven niet in kaart gebracht** of zijn slechts gedeeltelijk ontdekt  

---