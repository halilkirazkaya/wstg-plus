## WSTG-INFO-01 — Uitvoeren van zoekmachine-onderzoek en verkenningsactiviteiten naar informatielekken

Zoekmachine-onderzoek omvat het gebruik van openbare zoekmachines, gecachte pagina's en indexeringsdiensten om informatie te identificeren die de doelorganisatie onbedoeld heeft blootgesteld. Aanvallers gebruiken geavanceerde zoekoperators (`Google Dorks`) en indexeringsdiensten van derden om gevoelige bestanden, mappen, inlogportalen, foutmeldingen en metadata te ontdekken die niet openbaar toegankelijk zouden mogen zijn. Deze verkenningsfase is cruciaal omdat er geen directe interactie met het doelwit vereist is, waardoor het vrijwel ondetecteerbaar is, terwijl het potentieel waardevolle toegangspunten onthult zoals blootgestelde beheerpanelen, configuratiebestanden, database-dumps en interne documentatie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Worden gevoelige bestanden of mappen geïndexeerd door zoekmachines?
- [ ] Nee — geen gevoelige inhoud gevonden in de resultaten van zoekmachines  
- [ ] Ja — configuratiebestanden, back-ups of interne documenten **worden** geïndexeerd *(Kritiek)*  

### Onthult Google Dorking blootgestelde beheerders- of inloginterfaces?
- [ ] Nee — beheerpanelen en inlogpagina's zijn **niet** geïndexeerd  
- [ ] Ja — beheerpanelen **zijn** geïndexeerd maar vereisen sterke multifactorauthenticatie  
- [ ] Ja — beheerpanelen **zijn** geïndexeerd en openbaar toegankelijk **zonder** voldoende controles  

### Worden via gecachte versies van pagina's gevoelige gegevens blootgesteld die inmiddels zijn verwijderd?
- [ ] Nee — geen gevoelige gegevens gevonden in gecachte snapshots  
- [ ] Ja — gecachte pagina's bevatten inloggegevens, interne IP-adressen of gevoelige informatie  

### Onthult het `robots.txt` bestand onbedoeld gevoelige paden?
- [ ] Nee — `robots.txt` onthult **geen** gevoelige mapstructuren  
- [ ] Ja — `robots.txt` vermeldt gevoelige paden die de verkenning door een aanvaller vergemakkelijken  
- [ ] Er bestaat geen `robots.txt` bestand  

### Onthullen diensten van derden (`Shodan`, `Censys`, `Wayback Machine`) historische of huidige blootstelling?
- [ ] Nee — geen significante bevindingen via indexeringsdiensten van derden  
- [ ] Ja — historische snapshots of scans van diensten onthullen gevoelige metadata of service-banners

---

## WSTG-INFO-02 — Fingerprint Web Server

Webserver-fingerprinting is het proces van het identificeren van het specifieke softwaretype, de versie en het onderliggende besturingssysteem van een doel-webserver. Aanvallers voeren deze reconnaissance uit om bekende kwetsbaarheden te identificeren, zoals ongepatchte CVE's of configuratiezwakheden, die specifiek zijn voor bepaalde versies van Apache, Nginx, IIS of andere servertechnologieën. Dit wordt doorgaans bereikt door het analyseren van HTTP-responskoppen (bijv. `Server`, `X-Powered-By`), het onderzoeken van uniek protocolgedrag of het observeren van informatie die wordt gelekt via standaard foutpagina's en bestanden. Succesvolle fingerprinting stelt een aanvaller in staat om hun exploitatiestrategie aan te passen, overgaand van generiek scannen naar aanvallen met een hoge waarschijnlijkheid op succes tegen bekende architecturale fouten.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Informatief / Laag* |

> *De ernst wordt Hoog als de fingerprinting een verouderde of end-of-life (EOL) serverversie onthult met bekende kritieke kwetsbaarheden.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Zijn er identificerende strings aanwezig in de HTTP-responskoppen?
- [ ] Nee — `Server` en `X-Powered-By` headers zijn **niet aanwezig** of bevatten generieke waarden  
- [ ] Ja — headers onthullen serversoftware maar **geen** specifieke versienummers  
- [ ] Ja — headers onthullen specifieke serversoftware en **exacte** versienummers  

### Lekken standaard foutpagina's of door het systeem gegenereerde responsen servergegevens?
- [ ] Nee — er worden aangepaste foutpagina's gebruikt en deze onthullen **geen** serverinformatie  
- [ ] Ja — standaard foutpagina's lekken de naam van de serversoftware en/of de versie  
- [ ] Ja — foutpagina's lekken serverdetails, versie en het **onderliggende besturingssysteem**  

### Wordt de serverversie onthuld via standaardbestanden of mappenstructuren?
- [ ] Nee — standaard installatiebestanden, handleidingen en testscripts zijn **verwijderd**  
- [ ] Ja — standaardbestanden (bijv. `info.php`, `manual/`, `test.html`) zijn **aanwezig** en onthullen versiegegevens  

### Kan het servertype nauwkeurig worden afgeleid via een analyse van uniek gedrag?
- [ ] Nee — serverresponsen zijn genormaliseerd en op gedrag gebaseerde fingerprinting is **niet mogelijk**  
- [ ] Ja — uniek gedrag (bijv. volgorde van headers, specifieke foutcodes of TCP/IP-stackkenmerken) **maken** serveridentificatie mogelijk  

### Wordt de geïdentificeerde serverversie momenteel ondersteund en gepatcht?
- [ ] Ja — de geïdentificeerde versie is actueel en wordt **ondersteund** door de leverancier  
- [ ] Nee — de geïdentificeerde versie is **verouderd** of **end-of-life (EOL)** met bekende kwetsbaarheden

---

## WSTG-INFO-03 — Review Webserver Metafiles op Information Leakage

Webserver-metafiles zoals `robots.txt`, `sitemap.xml` en `security.txt` zijn bedoeld om crawlers aan te sturen en administratieve metadata te verstrekken, maar ze onthullen vaak onbedoeld gevoelige mappenstructuren of verborgen endpoints. Aanvallers analyseren deze bestanden om het aanvalsoppervlak (Attack Surface) van de applicatie in kaart te brengen, waarbij ze "Disallow"-richtlijnen identificeren die vaak wijzen naar niet-gekoppelde administratieve panels, back-upmappen of ontwikkelomgevingen. Naast instructies voor zoekmachines kunnen bestanden in de `.well-known/` map of bestanden zoals `humans.txt` technologiestacks, ontwikkelaarsinformatie of beveiligingscontacten van de organisatie onthullen. Systematische beoordeling van deze bestanden stelt een tester in staat om structurele en technische inzichten te verkrijgen zonder agressieve Brute Force discovery uit te voeren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Laag / Informatief* |

> *Severity wordt Medium als metafiles niet-geauthenticeerde administratieve interfaces of gevoelige configuratieback-ups onthullen.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Bestaat het `robots.txt` bestand en worden er gevoelige mappen blootgesteld?
- [ ] Nee — `robots.txt` bestaat **niet**  
- [ ] Ja — `robots.txt` bestaat maar bevat alleen standaard publieke paden  
- [ ] Ja — `robots.txt` onthult gevoelige mappaden of administratieve interfaces  
- [ ] Ja — `robots.txt` onthult credentials of specifieke softwareversies in commentaar  

### Is er een `sitemap.xml` bestand aanwezig en onthult dit de verborgen applicatiestructuur?
- [ ] Nee — `sitemap.xml` bestaat **niet**  
- [ ] Ja — `sitemap.xml` is aanwezig maar bevat alleen publiek toegankelijke URL's  
- [ ] Ja — `sitemap.xml` bevat niet-gekoppelde endpoints of interne bronnen die wel toegankelijk **zijn**  

### Onthullen beveiligings- en organisatorische metafiles technische details?
- [ ] Nee — geen metadata-bestanden gevonden in de root of `.well-known/`  
- [ ] Ja — `security.txt` of `humans.txt` zijn aanwezig maar bevatten standaardinformatie  
- [ ] Ja — metafiles onthullen interne hostnamen, identiteiten van ontwikkelaars of details over de technologiestack die kunnen helpen bij social engineering of verdere aanvallen  

### Zijn er legacy of framework-specifieke metafiles aanwezig?
- [ ] Nee — geen framework-specifieke bestanden (bijv. `info.php`, `composer.json`, `package.json`) gevonden  
- [ ] Ja — bestanden zoals `README.md`, `CHANGELOG.txt` of `.DS_Store` zijn aanwezig maar geschoond (sanitized)  
- [ ] Ja — framework- of versiespecifieke bestanden zijn aanwezig en **maken** nauwkeurige technology fingerprinting mogelijk

---

## WSTG-INFO-04 — Applicaties op de webserver enumereren

Applicatie-enumeratie is het proces van het identificeren van alle webapplicaties en diensten die worden gehost op een enkele webserver of IP-adres. Omdat moderne webservers vaak meerdere Virtual Hosts, verouderde staging-omgevingen of administratieve interfaces op niet-standaard poorten en paden hosten, blijft een aanzienlijk deel van het aanvalsoppervlak (attack surface) ongetest als deze verborgen assets niet worden geïdentificeerd. Aanvallers maken gebruik van technieken zoals DNS zone transfers, virtual host brute-forcing en reverse IP lookups om deze vergeten of obscure toegangspunten te ontdekken, die vaak minder veilig zijn dan de primaire productie-applicatie.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Informatief / Laag |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Tools:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Zijn er meerdere IP-adressen of DNS-aliassen gekoppeld aan de doel-infrastructuur?
- [ ] Nee — er bestaat slechts één IP en A-record  
- [ ] Ja — er zijn meerdere IP-adressen of aliassen geïdentificeerd, wat de scope vergroot  

### Host de webserver meerdere Virtual Hosts (vhosts) op hetzelfde IP-adres?
- [ ] Nee — alleen het primaire domein wordt door de server bediend  
- [ ] Ja — aanvullende Virtual Hosts zijn geïdentificeerd, maar toegang is niet mogelijk (bijv. 403 Forbidden)  
- [ ] Ja — verborgen, development- of staging-Virtual Hosts zijn geïdentificeerd en toegankelijk  

### Worden applicaties gehost op niet-standaard poorten?
- [ ] Nee — alleen standaardpoorten (80/443) zijn open  
- [ ] Ja — niet-standaard poorten zijn open, maar hosten geen webapplicaties  
- [ ] Ja — aanvullende webapplicaties zijn geïdentificeerd op niet-standaard poorten (bijv. 8080, 8443, 9000)  

### Zijn er afzonderlijke applicaties toegewezen aan specifieke URI-paden?
- [ ] Nee — één enkele applicatie bedient de volledige root-directory  
- [ ] Ja — er zijn meerdere applicaties ontdekt via directory-enumeratie (bijv. `/api`, `/portal`, `/backup`)  

### Onthullen reconnaissance-diensten van derden historische of secundaire applicaties?
- [ ] Nee — geen significante bevindingen via `Shodan`, `Censys` of `Wayback Machine`  
- [ ] Ja — historische snapshots of scans onthullen applicaties die momenteel actief zijn, maar niet gelinkt

---

## WSTG-INFO-05 — Webpaginacontent controleren op informatielekken

Het controleren van webpaginacontent omvat de handmatige en geautomatiseerde inspectie van applicatierespons, inclusief HTML-, JavaScript- en CSS-bestanden, om gevoelige informatie te identificeren die onbedoeld aan gebruikers wordt blootgesteld. Aanvallers onderzoeken de broncode aan de clientzijde op commentaren van ontwikkelaars, verborgen formuliervelden, interne serverpaden, hardcoded API-sleutels en debugging-informatie die kan helpen bij verdere exploitatiefasen. Dit lekken treedt vaak op wanneer ontwikkelaars vergeten metadata of debugging-code te verwijderen voordat deze naar productie wordt verplaatst, wat mogelijk logische fouten of details over de backend-infrastructuur onthult. Vanuit het perspectief van een aanvaller biedt dit een methode met weinig inspanning om de interne structuur van de applicatie in kaart te brengen en over het hoofd geziene toegangspunten te ontdekken zonder beveiligingswaarschuwingen te activeren of interactie te hebben met de backend-logica van de server.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld* |

> *De ernst wordt Hoog als er gevoelige hardcoded credentials, privésleutels of interne omgevingsvariabelen worden ontdekt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Bevat de broncode commentaren van ontwikkelaars met gevoelige informatie?
- [ ] Nee — geen commentaren of alleen generieke, niet-gevoelige commentaren gevonden  
- [ ] Ja — commentaren bestaan maar bevatten **geen** gevoelige logica of gegevens  
- [ ] Ja — commentaren **onthullen** interne paden, SQL-query's of administratieve instructies  
- [ ] Ja — commentaren **onthullen** credentials of gevoelige aantekeningen van ontwikkelaars *(Hoog)*  

### Bevatten verborgen HTML-invoervelden gevoelige gegevens of statusinformatie?
- [ ] Nee — geen verborgen velden gevonden of ze bevatten alleen niet-gevoelige tokens  
- [ ] Ja — verborgen velden worden gebruikt voor state-beheer maar zijn **versleuteld** of **ondertekend**  
- [ ] Ja — verborgen velden bevatten **plaintext** wachtwoorden, gebruikersrollen of interne ID's  

### Zijn er hardcoded secrets, API-sleutels of interne IP-adressen aanwezig in JavaScript-bestanden?
- [ ] Nee — geen gevoelige strings of secrets gevonden in JS-bestanden  
- [ ] Ja — JS-bestanden bevatten publieke API-sleutels met **juiste** restricties  
- [ ] Ja — JS-bestanden bevatten **onbeveiligde** API-sleutels, interne IP's of private endpoints  
- [ ] Ja — JS-bestanden bevatten **hardcoded** administratieve credentials of privésleutels *(Kritiek)*  

### Onthult de applicatie framework-versies of interne bestandspaden in de broncode?
- [ ] Nee — framework-informatie en bestandspaden zijn **geminificeerd** of **geobfusceerd**  
- [ ] Ja — framework-namen en versies zijn **zichtbaar** maar gepatcht  
- [ ] Ja — interne bestandspaden of absolute serverpaden zijn **blootgesteld** in de broncode  

### Is de broncode gevoelig voor lekken door een gebrek aan minificatie of obfuscatie?
- [ ] Nee — productiecode is **volledig geminificeerd** en ontdaan van commentaren  
- [ ] Ja — code is geminificeerd maar commentaren **blijven aanwezig** in de broncode  
- [ ] Ja — source maps (`.map`-bestanden) zijn **publiekelijk toegankelijk**, wat volledige reconstructie van de broncode mogelijk maakt

---

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

## WSTG-INFO-07 — Uitvoeringspaden door de applicatie in kaart brengen

Het in kaart brengen van uitvoeringspaden omvat de systematische identificatie van alle bereikbare eindpunten, functionele stromen en beslissingsvertakkingen binnen een webapplicatie. Dit proces is essentieel om ervoor te zorgen dat er geen "verborgen" functionaliteit — zoals legacy code, debug-interfaces of niet-gedocumenteerde API-routes — buiten het beveiligingsonderzoek blijft, aangezien deze vaak moderne beveiligingscontroles missen. Aanvallers gebruiken een combinatie van geautomatiseerde spidering en handmatige verkenning om de structuur van de applicatie te visualiseren, waarbij ze vaak kwetsbaarheden ontdekken zoals ongeautoriseerde administratieve toegang of fouten in de business logic tijdens het proces. Door inputs te correleren aan specifieke server-side responses kunnen testers gevoelige verwerkingslogica lokaliseren die gerichte diepgaande tests vereist om een robuuste dekking te garanderen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Informationeel |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### Is de applicatie volledig geïndexeerd met behulp van geautomatiseerd en handmatig crawlen?
- [ ] Ja — een uitgebreide kaart van alle zichtbare en gekoppelde bronnen **is voltooid**  
- [ ] Ja — geautomatiseerd crawlen **is voltooid**, maar handmatige verkenning is in behandeling  
- [ ] Nee — de applicatie is **niet** geïndexeerd  

### Zijn verborgen of niet-gedocumenteerde eindpunten geïdentificeerd via JS-analyse of directory brute-forcing?
- [ ] Nee — geen niet-gedocumenteerde paden ontdekt via side-channel analyse  
- [ ] Ja — verborgen paden ontdekt, maar deze **kunnen niet** worden geopend zonder autorisatie  
- [ ] Ja — niet-gedocumenteerde eindpunten **zijn** toegankelijk en bieden gevoelige functionaliteit *(Kritiek / Hoog)*  

### Kunnen uitvoeringspaden worden gewijzigd via niet-standaard parameters of headers?
- [ ] Nee — parameters zoals `debug`, `test`, of `admin` hebben **geen** invloed op de uitvoeringsstroom  
- [ ] Ja — parameters wijzigen de output maar **omzeilen** de beveiligingscontroles niet  
- [ ] Ja — pad-wijzigende parameters **worden toegepast** en maken het mogelijk om de beoogde logica te omzeilen  

### Is de stroom van multi-step business logic (bijv. multi-factor auth, checkout) volledig in kaart gebracht?
- [ ] Ja — alle statussen en overgangen in processen met meerdere stappen **zijn geïdentificeerd**  
- [ ] Nee — complexe state machine transities **kunnen niet** volledig in kaart worden gebracht  

### Zijn API-documentatiebestanden (Swagger/OpenAPI) of client-side mappen (Source Maps) blootgesteld?
- [ ] Nee — er zijn geen architecturale kaarten of documentatiebestanden publiekelijk blootgesteld  
- [ ] Ja — documentatie is aanwezig maar **is beveiligd** door authenticatie  
- [ ] Ja — Swagger UI, OpenAPI JSON, of JS Source Maps **zijn ingeschakeld** en publiekelijk toegankelijk

---

## WSTG-INFO-08 — Fingerprint Web Application Framework

Het fingerprinten van een webapplicatie-framework houdt in dat de specifieke technologiestack en softwareversies worden geïdentificeerd die zijn gebruikt om de applicatie te bouwen en te hosten. Aanvallers analyseren HTTP-response headers, framework-specifieke cookies, voorspelbare bestandsstructuren en unieke JavaScript-artefacten om te bepalen of het doelwit draait op platforms zoals React, Angular, Django of Spring. Deze verkenningsfase (reconnaissance) is essentieel omdat het een tester in staat stelt het aanvalsoppervlak (attack surface) te verkleinen tot bekende kwetsbaarheden (CVE's) en veelvoorkomende configuratiezwakheden die specifiek zijn voor dat framework. Door de onderliggende architectuur te begrijpen, kan een aanvaller overstappen van generieke testen naar zeer gerichte exploitatiestrategieën.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Informatief |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Onthullen HTTP-response headers de naam of versie van het framework?
- [ ] Nee — headers zoals `X-Powered-By` of `Server` zijn **niet** aanwezig of zijn gesaneerd (sanitized)  
- [ ] Ja — de naam van het framework is aanwezig, maar de versie **is verborgen**  
- [ ] Ja — zowel de naam van het framework als de versie **worden onthuld**  

### Zijn framework-specifieke cookies of directorystructuren identificeerbaar?
- [ ] Nee — veelvoorkomende framework-artefacten (bijv. `.js` paden, cookienamen) zijn geobfusceerd of hernoemd  
- [ ] Ja — framework-specifieke cookies (bijv. `JSESSIONID`, `PHPSESSID`, `csrftoken`) **zijn** identificeerbaar  
- [ ] Ja — directorystructuren (bijv. `/wp-content/`, `_next/static/`) **zijn** zichtbaar en bevestigen het framework  

### Onthullen foutmeldingen of commentaren in de broncode details over het framework?
- [ ] Nee — foutafhandeling (error handling) is generiek en commentaren zijn verwijderd  
- [ ] Ja — framework-specifieke stack traces **staan aan** en zijn zichtbaar in responses  
- [ ] Ja — de HTML-broncode bevat commentaren of metadata-tags die het framework identificeren  

### Zijn er bekende kwetsbaarheden geassocieerd met de geïdentificeerde framework-versie?
- [ ] Nee — het geïdentificeerde framework is up-to-date en er zijn **geen bekende** publieke kwetsbaarheden  
- [ ] Ja — de framework-versie is verouderd en specifieke CVE's **kunnen worden** geïdentificeerd

---

## WSTG-INFO-09 — Fingerprinting van webapplicaties

Het fingerprinten van een webapplicatie is het proces van het identificeren van het specifieke framework, Content Management System (CMS) of de onderliggende technologiestack en de bijbehorende versies. Deze verkenningsfase (reconnaissance) is essentieel omdat het een aanvaler in staat stelt om geïdentificeerde componenten te koppelen aan bekende kwetsbaarheden (CVE's) en publieke exploits, waardoor het aanvalsoppervlak aanzienlijk wordt verkleind. Informatie wordt doorgaans verkregen uit HTTP-responsheaders, unieke bestandspaden, cookienamen, metadata in de HTML-broncode en cryptografische hashes van statische assets. Door de technologiestack nauwkeurig te bepalen, kan een aanvaller overstappen van generieke geautomatiseerde scanning naar zeer gerichte exploitatie van versiespecifieke zwakheden.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Informatief |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Onthullen HTTP-responsheaders de technologiestack of versienummers?
- [ ] Nee — gevoelige headers zijn verwijderd of bevatten generieke waarden  
- [ ] Ja — standaardheaders zoals `X-Powered-By`, `Server` of `X-AspNet-Version` **zijn** aanwezig  
- [ ] Ja — headers onthullen specifieke, verouderde versies van de webserver of het applicatieframework  

### Is het applicatieframework of CMS identificeerbaar via unieke bestandspaden of naamgevingsconventies?
- [ ] Nee — bestandspaden zijn geobfusceerd en onthullen de onderliggende technologie **niet**  
- [ ] Ja — gangbare mappenstructuren (bijv. `/wp-content/`, `/_next/`, `/node_modules/`) **zijn** zichtbaar  
- [ ] Ja — unieke bestanden zoals `robots.txt`, `humans.txt` of `sitemap.xml` bevatten technologiespecifieke metadata  

### Worden specifieke versienummers blootgesteld in de HTML-broncode of statische assets?
- [ ] Nee — er is geen versie-informatie gevonden in commentaren of asset-parameters  
- [ ] Ja — HTML-commentaren of meta-tags (bijv. `<meta name="generator" content="...">`) **zijn** aanwezig  
- [ ] Ja — versiestrings zijn toegevoegd aan CSS/JS-bestandsnamen (bijv. `jquery.js?v=1.12.4`) en **kunnen niet** worden uitgeschakeld  

### Onthullen standaard foutpagina's of beheerinterfaces de softwareomgeving?
- [ ] Nee — de applicatie gebruikt aangepaste, generieke foutpagina's voor alle statuscodes  
- [ ] Ja — standaard foutpagina's voor de webserver of het framework **zijn** ingeschakeld en lekken versiegegevens  
- [ ] Ja — administratieve loginportalen (bijv. `/wp-admin`, `/phpmyadmin`) **zijn** toegankelijk en identificeerbaar  

### Onthullen cookies het framework voor sessiebeheer?
- [ ] Nee — sessiecookies gebruiken generieke of aangepaste namen  
- [ ] Ja — technologiespecifieke cookienamen (bijv. `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) **worden** gebruikt

---

## WSTG-INFO-10 — Applicatiearchitectuur in kaart brengen

Het in kaart brengen van de applicatiearchitectuur omvat het identificeren van de onderliggende technologieën, infrastructuurcomponenten en gegevensstromen die de webapplicatie ondersteunen. Door HTTP-responskoppen (headers), foutmeldingen, cookieformaten en bestandsextensies te analyseren, kunnen testers een schema reconstrueren van de gebruikte webservers, applicatieservers, databases en integraties van derden. Deze kennis is fundamenteel voor het afstemmen van daaropvolgende aanvallen, omdat het de selectie van platformspecifieke exploits en de identificatie van potentiële knelpunten of verkeerd geconfigureerde tussenliggende apparaten zoals Load Balancers en WAFs mogelijk maakt. Een aanvaller gebruikt deze structurele kaart om over te gaan van generiek scannen naar gerichte exploitatie van de specifieke softwarestack en de onderling verbonden componenten.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Informatief / Laag* |

> *De ernst wordt Gemiddeld (Medium) als de interne netwerktopologie of private IP-adressen worden onthuld.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Kunnen de webserver- en applicatieservertechnologieën nauwkeurig worden geïdentificeerd?
- [ ] Nee — geen identificatie **mogelijk** vanwege effectieve hardening of obfuscation  
- [ ] Ja — het technologietype is bekend, maar specifieke versies **kunnen niet** worden vastgesteld  
- [ ] Ja — technologie en specifieke versies **worden** volledig onthuld via headers of bestandsgegevens (file signatures) *(Informatief)*  

### Zijn tussenliggende apparaten (WAFs, Load Balancers, Proxies) detecteerbaar in het communicatiepad?
- [ ] Nee — er zijn geen tussenliggende apparaten gedetecteerd of ze zijn volledig transparant  
- [ ] Ja — het bestaan wordt vermoed via timing of gedrag, maar de identiteit **is niet** bevestigd  
- [ ] Ja — specifieke producten (bijv. Cloudflare, Nginx, F5) **zijn** geïdentificeerd via unieke headers of gedrag  

### Lekt de applicatie informatie over de interne mappenstructuur of netwerktopologie?
- [ ] Nee — interne paden en IP's worden **niet** onthuld  
- [ ] Ja — interne paden worden gelekt in foutmeldingen of stack traces, maar interne IP's **niet**  
- [ ] Ja — zowel interne mappenstructuren als private IP-adressen **worden** onthuld  

### Kan het type en de versie van de backend-database worden afgeleid uit het gedrag van de applicatie?
- [ ] Nee — databasegegevens **kunnen niet** worden vastgesteld  
- [ ] Ja — het databasetype (bijv. PostgreSQL, MSSQL) wordt afgeleid via foutmeldingen of gedrag  
- [ ] Ja — het databasetype en de versie **worden** expliciet onthuld in response bodies of headers  

### Wordt de applicatie gehost bij identificeerbare cloudproviders of specifieke CDN's van derden?
- [ ] Nee — de hostinginfrastructuur is **niet** identificeerbaar  
- [ ] Ja — de cloudprovider (AWS, Azure, GCP) is geïdentificeerd, maar specifieke diensten **niet**  
- [ ] Ja — specifieke clouddiensten (bijv. S3 buckets, Lambda, Azure Functions) **zijn** in kaart gebracht en bereikbaar

---

## WSTG-CONF-01 — Testen van de netwerkinfrastructuurconfiguratie

Het testen van de netwerkinfrastructuurconfiguratie omvat het identificeren van zwakheden in de onderliggende server- en netwerkcomponenten die de webapplicatie ondersteunen. Aanvallers richten zich op verkeerd geconfigureerde services, verouderde protocollen en onnodige open poorten om voet aan de grond te krijgen, zich zijwaarts door het netwerk te bewegen (Lateral Movement) of verkenningen (Reconnaissance) uit te voeren. Veelvoorkomende bevindingen zijn onveilige TLS-versies, uitgebreide (verbose) service banners en blootgestelde administratieve interfaces die beperkt zouden moeten blijven tot interne netwerken. Door deze tekortkomingen op infrastructuurniveau te misbruiken, kan een tegenstander de vertrouwelijkheid van gegevens in transit in gevaar brengen of ongeautoriseerde toegang verkrijgen tot het host-besturingssysteem.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Tools:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Zijn er onnodige open poorten of services blootgesteld op de applicatiehost?
- [ ] Nee — alleen vereiste poorten (bijv. 443) zijn **toegankelijk**  
- [ ] Ja — aanvullende services zijn blootgesteld maar volgen een **veilige** configuratie  
- [ ] Ja — niet-essentiële services (bijv. FTP, Telnet, SMB) zijn publiekelijk **blootgesteld**  

### Lekken service banners of headers gevoelige versie-informatie?
- [ ] Nee — banners zijn verwijderd of generiek  
- [ ] Ja — banners onthullen serversoftware (bijv. Apache/nginx) maar **geen** versienummers  
- [ ] Ja — verbose banners onthullen specifieke softwareversies, wat **Exploit**-pogingen vergemakkelijkt  

### Volgt de TLS-configuratie de moderne security best practices?
- [ ] Ja — alleen TLS 1.2/1.3 is **ingeschakeld** met sterke cipher suites  
- [ ] Ja — verouderde protocollen (TLS 1.0/1.1) zijn **ingeschakeld**  
- [ ] Nee — zwakke ciphers of onveilige protocollen (SSLv2/v3) zijn **ingeschakeld**  

### Zijn administratieve of beheerinterfaces beperkt tot geautoriseerde netwerken?
- [ ] Ja — interfaces (bijv. SSH, RDP, controlepanelen) zijn **niet toegankelijk** vanaf het publieke internet  
- [ ] Nee — interfaces zijn publiekelijk **toegankelijk** maar vereisen multi-factor authenticatie  
- [ ] Nee — interfaces zijn publiekelijk **toegankelijk** met single-factor authenticatie of standaardgegevens (default credentials)

---

## WSTG-CONF-02 — Configuratietesten van het Applicatieplatform

Het testen van de configuratie van het applicatieplatform omvat het auditen van de instellingen van de onderliggende webserver, applicatieserver en het framework om te waarborgen dat deze gehard (hardened) zijn tegen veelvoorkomende exploits. Aanvallers zoeken actief naar standaard inloggegevens (default credentials), niet-gepatchte platformkwetsbaarheden en blootgestelde administratieve interfaces om ongeautoriseerde toegang te verkrijgen of code uit te voeren. Configuratiefouten leiden vaak tot de onthulling van gevoelige omgevingsvariabelen, interne systeempaden of de aanwezigheid van overbodige voorbeeldapplicaties die het aanvalsoppervlak vergroten. Vanuit een professioneel perspectief dient een slecht geconfigureerd platform als een toegangspunt met een hoge waarschijnlijkheid voor het verkrijgen van initiële toegang tot de doelomgeving.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog indien administratieve interfaces toegankelijk zijn met standaard inloggegevens of indien voorbeeldscripts Remote Code Execution mogelijk maken.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Zijn er standaard inloggegevens of accounts actief op het platform?
- [ ] Nee — alle standaardaccounts zijn **uitgeschakeld** of wachtwoorden zijn gewijzigd  
- [ ] Ja — standaardaccounts bestaan, maar zijn **niet toegankelijk** vanaf het externe netwerk  
- [ ] Ja — standaardaccounts zijn **actief** en toegankelijk via het internet *(Kritiek)*  

### Onthult het platform gevoelige versie-informatie via headers of foutpagina's?
- [ ] Nee — versiestrings zijn **verborgen** of generiek  
- [ ] Ja — gedetailleerde versie-informatie **wordt onthuld** in HTTP-headers (bijv. `Server`, `X-Powered-By`)  
- [ ] Ja — volledige stack traces en interne systeempaden **worden blootgesteld** in uitgebreide foutmeldingen  

### Zijn administratieve of beheerinterfaces op de juiste wijze beperkt?
- [ ] Nee — administratieve interfaces zijn **niet blootgesteld**  
- [ ] Ja — interfaces bestaan, maar zijn **beperkt** door IP allowlisting of Multi-Factor Authentication  
- [ ] Ja — interfaces (bijv. `/admin`, `/manager`, `/console`) zijn **publiekelijk toegankelijk** met single-factor authenticatie  

### Zijn er voorbeeldbestanden, scripts of documentatie aanwezig op de productieserver?
- [ ] Nee — alle niet-essentiële bestanden zijn **verwijderd**  
- [ ] Ja — voorbeeldbestanden of documentatie **zijn aanwezig**, maar lekken geen gevoelige informatie  
- [ ] Ja — voorbeeldscripts (bijv. `info.php`, `examples/`) **zijn aanwezig** en bieden een directe aanvalsvector  

### Is de server geconfigureerd om onveilige HTTP-methoden te ondersteunen?
- [ ] Nee — alleen `GET` en `POST` zijn **ingeschakeld**  
- [ ] Ja — potentieel risicovolle methoden zoals `PUT`, `DELETE` of `TRACE` zijn **ingeschakeld**, maar beperkt  
- [ ] Ja — risicovolle methoden zijn **ingeschakeld** en maken ongeautoriseerde bestandsaanpassing of diefstal van inloggegevens (credential theft) mogelijk

---

## WSTG-CONF-03 — Testen van de afhandeling van bestandsextensies voor gevoelige informatie

Het testen van de afhandeling van bestandsextensies omvat het identificeren of de webserver of applicatieserver gevoelige informatie prijsgeeft door bestanden aan te bieden die beperkt of uitgevoerd zouden moeten worden. Aanvallers zoeken regelmatig naar reservekopieën (backup files), configuratiebestanden en broncodefragmenten (bijv. `.bak`, `.old`, `.env`, `.inc`) die in de web root zijn achtergelaten en als platte tekst worden geserveerd vanwege een gebrek aan specifieke handler mappings. Deze kwetsbaarheid is van belang omdat het kan leiden tot de blootstelling van database-inloggegevens, API-sleutels en interne bedrijfslogica, wat diepere exploitatie van de infrastructuur vergemakkelijkt. Deze blootstellingen vinden doorgaans plaats in publieke mappen, uploadmappen of via verkeerd geconfigureerde MIME-type instellingen op de server.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als configuratiebestanden met inloggegevens of volledige broncode toegankelijk zijn.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Zijn gevoelige reservekopieën of tijdelijke bestandsextensies (bijv. `.bak`, `.old`, `.swp`, `~`) toegankelijk?
- [ ] Nee — de server retourneert 403 Forbidden of 404 Not Found voor algemene backup-extensies  
- [ ] Ja — de server staat het downloaden van backup-bestanden toe, maar deze bevatten **geen** gevoelige gegevens  
- [ ] Ja — de server staat het downloaden van backup-bestanden toe die gevoelige gegevens of broncode bevatten *(Hoog)*  

### Stelt de server omgevings- of systeemconfiguratiebestanden bloot (bijv. `.env`, `.config`, `.yml`, `.ini`)?
- [ ] Nee — beperkte configuratiebestanden zijn **niet** toegankelijk en retourneren 403/404  
- [ ] Ja — gevoelige configuratiebestanden **zijn** toegankelijk en onthullen interne geheimen of inloggegevens  

### Kan broncode worden opgevraagd door alternatieve extensies toe te voegen (bijv. `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] Nee — applicatiecode wordt **niet** als platte tekst weergegeven, ongeacht manipulatie van de extensie  
- [ ] Ja — broncode wordt onthuld omdat de server **geen** handler heeft voor de toegevoegde extensie  

### Zijn administratieve of metadata-mappen (bijv. `.git/`, `.svn/`, `.DS_Store`) geblokkeerd voor publieke toegang?
- [ ] Nee — deze mappen/bestanden bestaan **niet** in de web root  
- [ ] Ja — toegang is **niet mogelijk** vanwege server-side rewrite-regels of machtigingen  
- [ ] Ja — metadata-mappen **zijn** toegankelijk en maken volledige reconstructie van de broncode mogelijk  

### Hoe gaat de server om met verzoeken voor bestanden met meerdere extensies (bijv. `file.php.jpg`)?
- [ ] Nee — de server geeft correcte prioriteit aan de beveiliging van de interne extensie of blokkeert het verzoek  
- [ ] Ja — de server verwerkt het bestand als de eerste extensie, maar een bypass voor uitvoering is **niet mogelijk**  
- [ ] Ja — de server negeert de laatste extensie, waardoor uitvoering van code of onthulling van broncode **mogelijk is**

---

## WSTG-CONF-04 — Controle van oude back-upbestanden en niet-gerefereerde bestanden op gevoelige informatie

Het controleren van oude back-upbestanden en niet-gerefereerde bestanden omvat het identificeren van vergeten, tijdelijke of verborgen bestanden op een webserver die niet bedoeld zijn voor publieke toegang. Deze bestanden bevatten vaak broncode-back-ups (`.zip`, `.bak`), swap-bestanden van teksteditors (`.swp`, `~`) of source control metadata (`.git`, `.svn`) die gevoelige informatie kunnen lekken. Aanvallers gebruiken geautomatiseerde wordlists en discovery-tools om via Brute Force veelvoorkomende naamgevingsconventies te raden, met als doel database-credentials, hardcoded API-keys of logica die verdere Exploitatie vergemakkelijkt te exfiltreren. Deze kwetsbaarheid komt doorgaans voort uit handmatig serveronderhoud of gebrekkige CI/CD-pipelines die de productie web root niet correct opschonen.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Gemiddeld / Hoog* |

> *De Severity wordt Hoog wanneer configuratiebestanden, broncode-archieven of credentials worden ontdekt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Zijn directory listings ingeschakeld op de webserver?
- [ ] Nee — directory listing is **uitgeschakeld** en retourneert een 403 Forbidden of een aangepaste foutmelding  
- [ ] Ja — directory listing is **ingeschakeld** op niet-gevoelige mappen  
- [ ] Ja — directory listing is **ingeschakeld** op mappen die broncode of gevoelige bestanden bevatten *(Kritiek)*  

### Zijn back-upbestanden met veelvoorkomende extensies (bijv. .bak, .old, .save) vindbaar?
- [ ] Nee — veelvoorkomende back-up-extensies zijn **niet gevonden** of worden geblokkeerd door de serverconfiguratie  
- [ ] Ja — back-upbestanden bestaan, maar bevatten **geen** gevoelige informatie  
- [ ] Ja — back-upbestanden voor configuratie of broncode **zijn** toegankelijk *(Hoog)*  

### Zijn source control-directories (bijv. .git, .svn) of metadata blootgesteld?
- [ ] Nee — source control metadata is **niet aanwezig** of wordt correct geblokkeerd  
- [ ] Ja — metadata bestaat, maar de volledige repository kan **niet** worden gereconstrueerd  
- [ ] Ja — de volledige broncode-repository **kan** worden geëxfiltreerd via blootgestelde metadata  

### Zijn er gecomprimeerde archieven (bijv. .zip, .tar.gz, .rar) van de applicatie aanwezig in de web root?
- [ ] Nee — geen gevoelige archiefbestanden ontdekt via Brute Force of enumeratie  
- [ ] Ja — archieven gevonden, maar deze zijn beveiligd met een wachtwoord of bevatten publieke bestanden  
- [ ] Ja — onversleutelde archieven met de broncode of gegevens van de applicatie **zijn** publiekelijk toegankelijk  

### Lekt de applicatie tijdelijke bestanden die zijn aangemaakt door teksteditors of IDE's?
- [ ] Nee — tijdelijke bestanden zoals `.swp`, `~` of `.DS_Store` zijn **niet aanwezig**  
- [ ] Ja — niet-gevoelige tijdelijke bestanden zijn **aanwezig**  
- [ ] Ja — tijdelijke bestanden onthullen fragmenten van broncode of interne paden

---

## WSTG-CONF-05 — Inventariseren van infrastructuur- en applicatiebeheerinterfaces

Beheerinterfaces (Administrative interfaces) vormen het beheercontrolepaneel van een applicatie en de onderliggende infrastructuur. Deze interfaces verlenen vaak geprivilegieerde toegang tot systeemconfiguraties, gebruikersgegevens en server-side bewerkingen. Aanvallers geven prioriteit aan het inventariseren van deze interfaces via directory Brute Force, analyse van `robots.txt` of door het observeren van voorspelbare URL-patronen om toegangspunten te vinden die mogelijk geen robuuste authenticatie hebben of vertrouwen op standaard inloggegevens (default credentials). De ontdekking van een blootgesteld beheerderspaneel verhoogt het risico op volledige systeemovername, ongeoorloofde gegevenswijziging of serviceonderbreking aanzienlijk. Deze interfaces worden vaak aangetroffen op niet-standaard poorten of subdomeinen, waardoor ze belangrijke doelwitten zijn voor geautomatiseerde scanners en handmatige verkenning (reconnaissance).


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als de interface systeemwijde configuratiewijzigingen, gebruikersbeheer of data-exfiltratie toestaat zonder multifactorauthenticatie (MFA).

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Zijn beheerinterfaces ontdekbaar via directory fuzzing of verkenning (reconnaissance)?
- [ ] Nee — er zijn geen beheerinterfaces blootgesteld of ontdekbaar  
- [ ] Ja — interfaces zijn ontdekbaar, maar de toegang **is beperkt** tot interne IP-bereiken  
- [ ] Ja — interfaces **zijn** ontdekbaar en publiekelijk toegankelijk  

### Wordt authenticatie afgedwongen op de ontdekte beheerportalen?
- [ ] Ja — multifactorauthenticatie (MFA) **wordt afgedwongen**  
- [ ] Ja — single-factor authenticatie **wordt afgedwongen**  
- [ ] Nee — er is geen authenticatie vereist om toegang te krijgen tot de interface *(Kritiek)*  

### Zijn beheerderspaden verborgen of niet-standaard om ontdekking te voorkomen?
- [ ] Ja — paden maken gebruik van niet-standaard, willekeurige of niet-voorspelbare naamgeving  
- [ ] Nee — paden maken gebruik van gangbare naamgevingsconventies (bijv. `/admin`, `/manager`, `/console`) en **kunnen** gemakkelijk worden geraden  

### Biedt de interface toegang tot gevoelige systeem- of applicatiefunctionaliteit?
- [ ] Nee — de interface is een "read-only" statusdashboard met minimale impact  
- [ ] Ja — de interface **kan** applicatieconfiguraties of gebruikersgegevens wijzigen  
- [ ] Ja — de interface **kan** commando's op systeemniveau uitvoeren of databasebeheer uitvoeren

---

## WSTG-CONF-06 — Test HTTP-methoden

Het testen van HTTP-methoden omvat het identificeren van welke verbs door de webserver en applicatie worden ondersteund, om te waarborgen dat alleen noodzakelijke functionaliteit wordt blootgesteld. Naast de standaard `GET` en `POST` kunnen servers onbedoeld gevaarlijke methoden inschakelen zoals `PUT`, `DELETE`, of `TRACE`. Deze kunnen aanvallers in staat stellen om kwaadaardige bestanden te uploaden, bestaande inhoud te verwijderen of sessiecookies te exfiltreren via Cross-Site Tracing (XST). Pentesters onderzoeken response headers zoals `Allow` of `Public` en proberen beperkingen te omzeilen met behulp van method overriding-technieken of verb tampering om toegang te krijgen tot ongeautoriseerde functionaliteit. Deze test is cruciaal voor het harden van het aanvalsoppervlak en het voorkomen van misconfiguraties die kunnen leiden tot server-compromise of ongeautoriseerde datamanipulatie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld* |

> *De ernst wordt Hoog als `PUT` of `DELETE` ongeautoriseerde bestandsmanipulatie toestaan of als `TRACE` de exfiltratie van sessietokens vergemakkelijkt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Zijn gevaarlijke HTTP-methoden zoals `PUT` of `DELETE` uitgeschakeld in de gehele applicatie?
- [ ] Ja — alleen veilige methoden (GET, POST, HEAD) **zijn ingeschakeld**  
- [ ] Nee — `PUT` of `DELETE` **zijn ingeschakeld** maar vereisen geldige authenticatie  
- [ ] Nee — `PUT` of `DELETE` **zijn ingeschakeld** en toegankelijk zonder authenticatie *(Kritiek)*  

### Is de `TRACE`-methode uitgeschakeld om Cross-Site Tracing (XST) te voorkomen?
- [ ] Ja — `TRACE`-methode **is uitgeschakeld** of retourneert een 405 Method Not Allowed  
- [ ] Nee — `TRACE`-methode **is ingeschakeld** maar reflecteert geen gevoelige headers  
- [ ] Nee — `TRACE`-methode **is ingeschakeld** en reflecteert `Cookie` of `Authorization` headers *(Gemiddeld)*  

### Wordt HTTP Method Overriding (bijv. `X-HTTP-Method-Override`) ondersteund door de server?
- [ ] Nee — server verwerkt **geen** method override headers  
- [ ] Ja — server verwerkt override headers maar access controls **worden afgedwongen**  
- [ ] Ja — server verwerkt override headers en een access control bypass **is mogelijk**  

### Reageert de server veilig op willekeurige of misvormde HTTP-methoden?
- [ ] Ja — server retourneert een 405 Method Not Allowed of 501 Not Implemented  
- [ ] Nee — server retourneert een 200 OK of 500 Error voor willekeurige methoden zoals `JEFF` of `TEST`  
- [ ] Nee — het gebruik van willekeurige methoden maakt het omzeilen van authenticatie- of autorisatiefilters mogelijk  

### Is de `OPTIONS`-methode beperkt of geconfigureerd om informatieblootstelling te minimaliseren?
- [ ] Ja — `OPTIONS` is uitgeschakeld of retourneert een minimale `Allow`-header  
- [ ] Nee — `OPTIONS` onthult een breed scala aan ingeschakelde methoden, inclusief DAV of debugging verbs

---

## WSTG-CONF-07 — Test HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) is een beveiligingsmechanisme waarmee een webserver browsers kan informeren dat deze alleen via HTTPS mag worden benaderd, waardoor protocol downgrade-aanvallen en cookie hijacking worden voorkomen. Door een versleutelde verbinding af te dwingen, beperkt HSTS Man-in-the-Middle (MitM) aanvallen zoals SSLStrip, waarbij een aanvaller een initiële onbeveiligde HTTP-aanvraag onderschept om de overgang naar TLS te voorkomen. Vanuit het perspectief van een aanvaller biedt de afwezigheid van deze header of een korte `max-age` duur een kans om gevoelige sessietokens of inloggegevens (credentials) te onderscheppen tijdens de initiële cleartext handshake. Deze test richt zich op het verifiëren van de aanwezigheid, duur en reikwijdte van de `Strict-Transport-Security` header op alle gevoelige applicatie-endpoints.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld* |

> *De ernst wordt Gemiddeld als de applicatie PII, sessietokens of credentials verwerkt, aangezien het ontbreken van HSTS het succespercentage van MitM-aanvallen verhoogt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### Is de `Strict-Transport-Security` header aanwezig in HTTPS-responses?
- [ ] Ja — header **is aanwezig** op alle gevoelige endpoints  
- [ ] Ja — header **is aanwezig**, maar alleen op de landingpagina  
- [ ] Nee — header **ontbreekt** volledig in de applicatie  

### Is de `max-age` directive ingesteld op een voldoende lange duur?
- [ ] Ja — `max-age` is ingesteld op een jaar of langer (bijv. `31536000`)  
- [ ] Ja — `max-age` is ingesteld, maar is **te kort** (bijv. minder dan 6 maanden)  
- [ ] Nee — `max-age` directive **ontbreekt** of is ingesteld op `0` *(Kritiek)*  

### Dekt de HSTS-policy alle subdomeinen?
- [ ] Ja — `includeSubDomains` directive **is toegepast**  
- [ ] Nee — `includeSubDomains` directive **ontbreekt**, waardoor subdomeinen kwetsbaar zijn voor downgrade  
- [ ] Nee — functie is niet van toepassing omdat er geen subdomeinen bestaan  

### Is de applicatie geregistreerd voor HSTS Preloading?
- [ ] Ja — `preload` directive **is aanwezig** en het domein staat op de HSTS preload-lijst  
- [ ] Ja — `preload` directive **is aanwezig**, maar het domein staat nog **niet** op de preload-lijst  
- [ ] Nee — `preload` directive **ontbreekt**, wat een window voor MitM toestaat bij het eerste bezoek  

### Wordt de HSTS-header onterecht verzonden over plaintext HTTP?
- [ ] Nee — header wordt **alleen** verzonden over beveiligde HTTPS-verbindingen  
- [ ] Ja — header **wordt verzonden** over HTTP, wat door browsers wordt genegeerd en configuratiedetails kan lekken

---

## WSTG-CONF-08 — Test RIA Cross Domain Policy

Rich Internet Application (RIA) cross-domain-beleid omvat bestanden zoals `crossdomain.xml` en `clientaccesspolicy.xml` die definiëren hoe webclients—met name Adobe Flash en Microsoft Silverlight—omgaan met cross-origin-verzoeken. Deze bestanden zijn cruciaal voor de beveiliging omdat ze expliciet de Same-Origin Policy (SOP) overschrijven, waarbij wordt bepaald welke externe domeinen toestemming hebben om te communiceren met de applicatie en de antwoorden te lezen. Te tolerante configuraties, zoals het gebruik van wildcards in het `domain`-attribuut, stellen aanvallers in staat om kwaadaardige RIA-objecten op een site van derden te hosten die gevoelige gegevens kunnen exfiltreren of acties kunnen uitvoeren namens geauthenticeerde gebruikers. Pentesters lokaliseren deze bestanden doorgaans in de web root om het vertrouwensniveau te evalueren dat aan externe bronnen is verleend en om potentiële cross-origin datalekken te identificeren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als de applicatie gevoelige gebruikersgegevens of sessie-identificatoren verwerkt die geëxfiltreerd kunnen worden via een tolerant beleid.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Zijn cross-domain policy-bestanden (`crossdomain.xml` of `clientaccesspolicy.xml`) aanwezig in de web root?
- [ ] Nee — bestanden **kunnen niet** worden gevonden in de root-directory  
- [ ] Ja — `crossdomain.xml` (Flash) **is** aanwezig  
- [ ] Ja — `clientaccesspolicy.xml` (Silverlight) **is** aanwezig  
- [ ] Ja — beide RIA policy-bestanden **zijn** aanwezig  

### Is het `allow-access-from` domein-attribuut beperkt tot vertrouwde origins?
- [ ] Nee — bestanden bestaan, maar bevatten geen `allow-access-from` vermeldingen  
- [ ] Ja — specifieke, vertrouwde domeinen staan expliciet op de whitelist en bypass is **niet mogelijk**  
- [ ] Ja — domeinen staan op de whitelist, maar bevatten onbetrouwbare derde partijen of subdomeinen  
- [ ] Ja — een wildcard `*` **wordt gebruikt**, wat toegang verleent vanaf **elke** origin *(Kritiek)*  

### Staat het beleid onveilige communicatie over HTTP toe voor HTTPS-bronnen?
- [ ] Nee — `secure="true"` is ingesteld (of standaard), wat voorkomt dat HTTP-origins toegang krijgen tot HTTPS-gegevens  
- [ ] Ja — `secure="false"` is expliciet ingesteld, waardoor MITM-aanvallers beveiligde gegevens kunnen exfiltreren  

### Zijn HTTP-headers te tolerant geconfigureerd in de cross-domain configuratie?
- [ ] Nee — `allow-http-request-headers-from` is beperkt of **niet ingeschakeld**  
- [ ] Ja — specifieke headers zijn toegestaan voor vertrouwde domeinen  
- [ ] Ja — wildcard `*` wordt gebruikt voor headers, waardoor aanvallers aangepaste headers kunnen verzenden vanaf elk domein  

### Is de `site-control` (master-policy) configuratie restrictief?
- [ ] Nee — `permitted-cross-domain-policies` is ingesteld op `none` of `master-only`  
- [ ] Ja — het beleid staat toe dat andere bestanden op de server hun eigen regels definiëren (`all`)

---

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

## WSTG-CONF-10 — Subdomain Takeover

Subdomain takeover vindt plaats wanneer een DNS-vermelding (meestal een CNAME, maar soms A- of MX-records) verwijst naar een buiten gebruik gestelde of niet-bestaande resource bij een externe cloudprovider of hostingdienst. Aanvallers identificeren deze "dangling" DNS-records door te zoeken naar specifieke foutmeldingen van providers zoals AWS, GitHub of Azure die aangeven dat een resource niet langer wordt geclaimd. Door de verlaten resourcenaam te registreren op het platform van de provider, krijgt de aanvaller de volledige controle over het subdomein. Hierdoor kunnen zij kwaadaardige inhoud hosten, sessiecookies exfiltreren die gericht zijn op het basisdomein, en Content Security Policy (CSP) of CORS-beveiligingen omzeilen. Deze kwetsbaarheid is bijzonder gevaarlijk omdat het gebruikmaakt van het vertrouwen dat verbonden is aan de legitieme domeinstructuur van de organisatie om phishing en datadiefstal met een hoge impact te faciliteren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Hoog / Kritiek* |

> *De ernst wordt Kritiek als het subdomein kan worden gebruikt om sessiecookies van het basisdomein te kapen (hijack) of SSO/OAuth-omleidingen te omzeilen.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Tools:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### Maakt de applicatie gebruik van externe hosting of clouddiensten via subdomeinen?
- [ ] Nee — alle subdomeinen verwijzen naar IP-ruimte die door de organisatie zelf wordt beheerd  
- [ ] Ja — subdomeinen verwijzen naar externe providers (S3, GitHub Pages, Heroku, etc.)  

### Zijn er "dangling" DNS-records die verwijzen naar niet-bestaande resources?
- [ ] Nee — alle geïdentificeerde DNS-records verwijzen naar actieve, beheerde resources  
- [ ] Ja — er bestaan CNAME- of A-records voor resources die **niet meer bestaan** bij de provider  
- [ ] Ja — DNS-records verwijzen naar NXDOMAIN of providerspecifieke foutpagina's *(bijv. "NoSuchBucket")*  

### Kan de geïdentificeerde "dangling" resource succesvol worden geclaimd?
- [ ] Nee — de provider heeft beveiligingen tegen het claimen van verlaten namen of accountverificatie is vereist  
- [ ] Ja — de resourcenaam **kan** door elke gebruiker worden geregistreerd op het platform van de derde partij  

### Gebruikt het basisdomein cookies met een "Domain"-scope die gecompromitteerd kunnen worden?
- [ ] Nee — cookies zijn strikt beperkt tot specifieke subdomeinen of gebruiken `Host-Only` flags  
- [ ] Ja — gevoelige cookies (sessie-ID's, JWT's) zijn ingesteld voor het bovenliggende domein en **kunnen** worden geëxfiltreerd via een gekaapt subdomein  

### Kan het subdomein worden gebruikt om beveiligingsheaders of authenticatiestromen te omzeilen?
- [ ] Nee — er zijn geen beveiligingsafhankelijkheden op de geïdentificeerde subdomeinen  
- [ ] Ja — het subdomein staat op de whitelist in CSP `script-src` of `connect-src` richtlijnen  
- [ ] Ja — het subdomein is een geldige redirect URI voor OAuth- of SSO-configuraties

---

## WSTG-CONF-11 — Cloudopslag testen

Het testen op misconfiguraties van cloudopslag omvat het identificeren en auditen van publiekelijk toegankelijke object storage services zoals Amazon S3, Azure Blobs, of Google Cloud Storage. Deze bronnen bevatten vaak gevoelige gegevens, waaronder back-ups, broncode, PII of configuratiebestanden die zijn blootgesteld door te tolerante Access Control Lists (ACLs) of Identity and Access Management (IAM) policies. Aanvallers enumereren bucket-namen via reconnaissance van JavaScript-bestanden, HTML-broncode en brute-force technieken om toegangspunten te vinden voor data exfiltratie of ongeautoriseerde bestandsaanpassingen. Exploitatie kan leiden tot aanzienlijke impact, waaronder volledige datalekken of de mogelijkheid om kwaadaardige bestanden te serveren vanaf een vertrouwd domein.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek* |

> *De ernst wordt als Kritiek beschouwd als PII, credentials of productieback-ups toegankelijk zijn, of als ongeauthenticeerde schrijftoegang is ingeschakeld.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Zijn de cloudopslag-endpoints (buckets/containers) geïdentificeerd en geënumereerd?
- [ ] Nee — er worden geen cloudopslag-endpoints gebruikt of deze zijn niet vindbaar  
- [ ] Ja — storage-endpoints zijn geïdentificeerd via code-analyse of verkeersmonitoring  
- [ ] Ja — storage-endpoints zijn ontdekt via brute-force op basis van naamgevingsconventies  

### Kunnen ongeauthenticeerde gebruikers de inhoud van de cloudopslag listen?
- [ ] Nee — bucket-listing is **uitgeschakeld** en retourneert 403 Forbidden of 404 Not Found  
- [ ] Ja — listing is **ingeschakeld**, maar er zijn geen gevoelige bestanden aanwezig  
- [ ] Ja — listing is **ingeschakeld** en gevoelige bestandsnamen/metadata zijn zichtbaar  

### Is het downloaden van gevoelige bestanden uit geïdentificeerde buckets mogelijk?
- [ ] Nee — bestanden zijn beveiligd door IAM-policies of een geldige authenticatie is vereist  
- [ ] Ja — bestanden zijn toegankelijk, maar vereisen een signed URL die **niet** eenvoudig te raden is  
- [ ] Ja — gevoelige bestanden (back-ups, `.env`, PII) zijn **publiekelijk toegankelijk** voor download *(Kritiek)*  

### Is ongeauthenticeerde bestandsupload of -wijziging toegestaan?
- [ ] Nee — ongeauthenticeerde uploads of wijzigingen zijn **niet mogelijk**  
- [ ] Ja — geauthenticeerde upload is vereist, maar een bypass **is mogelijk** via zwakke policy-voorwaarden  
- [ ] Ja — ongeauthenticeerd schrijven, overschrijven of verwijderen van bestanden is **mogelijk** *(Kritiek)*  

### Zijn de Cross-Origin Resource Sharing (CORS) policies op het storage-endpoint beperkend?
- [ ] Ja — CORS is **uitgeschakeld** of beperkt tot specifieke vertrouwde origins  
- [ ] Nee — CORS is **ingeschakeld** met een wildcard `*` origin, wat ongeautoriseerde webgebaseerde toegang mogelijk maakt

---

## WSTG-CONF-12 — Content Security Policy (CSP)

Content Security Policy (CSP) is een cruciaal defense-in-depth-mechanisme dat wordt geïmplementeerd via HTTP-responseheaders om risico's zoals Cross-Site Scripting (XSS), clickjacking en data-injectie-aanvallen te mitigeren. Door te definiëren welke dynamische bronnen mogen worden geladen, beperkt een goed geconfigureerde CSP de mogelijkheden van een aanvaller om ongeautoriseerde scripts uit te voeren of gevoelige gegevens te exfiltreren naar externe domeinen. Pentesters beoordelen het beleid op overmatig permissieve directieven, het gebruik van onveilige trefwoorden zoals `'unsafe-inline'`, en het vertrouwen op onveilige CDN's die bypasses kunnen vergemakkelijken. Een zwakke of ontbrekende CSP vormt niet direct een kwetsbaarheid, maar verhoogt de impact van succesvolle injectie-aanvallen aanzienlijk door het ontbreken van een secundaire beschermingslaag.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Tools:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Is er een Content Security Policy (CSP) header aanwezig in de applicatieresponses?
- [ ] Ja — `Content-Security-Policy` header is **aanwezig** en wordt **afgedwongen**  
- [ ] Ja — `Content-Security-Policy-Report-Only` is **aanwezig** voor testdoeleinden  
- [ ] Nee — er is geen CSP-header **aanwezig** in de responses  

### Zijn de `script-src` of `default-src` directieven correct beperkt?
- [ ] Ja — directieven maken gebruik van strikte whitelisting, nonces of hashes en een bypass is **niet mogelijk**  
- [ ] Ja — directieven zijn **correct geconfigureerd**, maar de whitelist bevat bekende bypassbare CDN's  
- [ ] Nee — directieven gebruiken wildcards (`*`) of `data:` schema's, waardoor een bypass **mogelijk** is  

### Staat het beleid de uitvoering van onveilige inline scripts of eval toe?
- [ ] Nee — `'unsafe-inline'` en `'unsafe-eval'` zijn **niet aanwezig**  
- [ ] Ja — `'unsafe-inline'` is **aanwezig** maar beschermd door een `nonce` of `hash`  
- [ ] Ja — `'unsafe-inline'` of `'unsafe-eval'` zijn **ingeschakeld** zonder aanvullende bescherming *(Kritiek)*  

### Is de applicatie beschermd tegen clickjacking via de `frame-ancestors` directie?
- [ ] Ja — `frame-ancestors` is ingesteld op `'none'` of `'self'`  
- [ ] Ja — `frame-ancestors` is **ingeschakeld** maar staat specifieke vertrouwde domeinen van derden toe  
- [ ] Nee — `frame-ancestors` **ontbreekt**, er wordt vertrouwd op verouderde `X-Frame-Options` of er is geen bescherming  

### Is er een mechanisme om CSP-schendingen te rapporteren?
- [ ] Ja — `report-uri` of `report-to` is **geconfigureerd** en actief  
- [ ] Nee — rapportage van schendingen is **uitgeschakeld** of **niet geconfigureerd**

---

## WSTG-CONF-13 — Path Confusion

Path Confusion vloeit voort uit discrepanties in de manier waarop verschillende webcomponenten, zoals reverse proxies, load balancers en backend applicatieservers, URL-paden parsen en interpreteren. Aanvallers misbruiken deze inconsistenties door specifieke karakters te injecteren, zoals puntkomma's, gecodeerde slashes of dot-segments, om beveiligingsfilters te misleiden en toegang te krijgen tot beperkte endpoints of statische bronnen. Deze kwetsbaarheid kan leiden tot kritieke beveiligingsfouten, waaronder authentication bypass, ongeautoriseerde toegang tot gegevens en web cache poisoning, aangezien de front-end beveiligingsregels kan toepassen op een pad dat door de back-end anders wordt geïnterpreteerd. Succesvolle exploitatie vindt doorgaans plaats op de architecturale grens waar request routing en normalisatielogica uiteenlopen binnen de tech stack.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als path confusion resulteert in een bypass van authenticatie of toegangscontrole voor gevoelige administratieve endpoints.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Interpreteren verschillende architecturale lagen pad-scheidingstekens (bijv. `;`, `#`, `?`) consistent?
- [ ] Ja — alle lagen normaliseren en interpreteren pad-scheidingstekens identiek  
- [ ] Nee — er bestaan inconsistenties, maar deze **kunnen niet** worden gebruikt om toegang te krijgen tot beperkte bronnen  
- [ ] Nee — discrepanties maken het voor een aanvaller **mogelijk** om front-end filters te omzeilen en de back-end logica te bereiken  

### Kunnen toegangscontroles worden omzeild met behulp van path traversal sequenties of gecodeerde karakters?
- [ ] Nee — normalisatie wordt consistent toegepast vóór beveiligingscontroles  
- [ ] Ja — bypass is **mogelijk** via dot-segment sequenties (bijv. `/admin/..;/`)  
- [ ] Ja — bypass is **mogelijk** via URL-encoded karakters (bijv. `%2f`, `%2e%2e%2f`)  

### Is de applicatie vatbaar voor Web Cache Poisoning via path confusion?
- [ ] Nee — caching-logica wordt niet beïnvloed door onduidelijkheid in het pad of extra pad-informatie  
- [ ] Ja — kwaadaardige inhoud **kan** worden gecachet voor legitieme paden als gevolg van mapping-discrepanties  
- [ ] Ja — gevoelige informatie **kan** worden gecachet in publieke mappen via `RCD` (Relative Path Overwrite) technieken  

### Verwerkt de server "extra pad-informatie" (PathInfo) op een veilige manier?
- [ ] Nee — functie is **uitgeschakeld** of bestaat niet  
- [ ] Ja — `PathInfo` wordt afgehandeld en interfereert **niet** met beveiligingsfilters  
- [ ] Nee — `PathInfo` stelt een aanvaller in staat om gegevens toe te voegen die de routing-logica van de applicatie verwarren

---

## WSTG-CONF-14 — Testen van andere HTTP Security Header misconfiguraties

Het testen op HTTP security header misconfiguraties omvat het verifiëren van de aanwezigheid en correcte implementatie van defensieve headers die essentiële bescherming bieden tegen veelvoorkomende webgebaseerde aanvalsvectoren. Hoewel deze headers onderliggende kwetsbaarheden in de code niet repareren, verhoogt de afwezigheid ervan het risico op man-in-the-middle (MITM) aanvallen, exploitatie van MIME-sniffing en onbedoelde cross-origin datalekken via referrers aanzienlijk. Vanuit het perspectief van een aanvaller zijn ontbrekende security headers duidelijke indicatoren van een slecht geharde (hardened) omgeving, wat vaak complexere exploitatieketens zoals session hijacking of informatie-exfiltratie vergemakkelijkt. Deze configuraties worden doorgaans geëvalueerd op serverniveau en moeten consistent worden toegepast op alle response-typen, inclusief API-endpoints en statische bronnen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audit op aanwezigheid van Security Headers
| Header | Aanwezig | Ontbrekend | Aanbevolen configuratie |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` of `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Functie-specifiek, bijv. `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Hoe worden security headers afgedwongen over het gehele applicatiebereik?
- [ ] Ja — headers worden globaal toegepast via een centrale load balancer of reverse proxy en **kunnen niet** worden overschreven  
- [ ] Ja — headers worden toegepast op responses van hoofddocumenten, maar **ontbreken** bij API-responses of statische assets  
- [ ] Nee — headers worden inconsistent toegepast of **ontbreken** over het gehele applicatiedomein  

### Is de Strict-Transport-Security (HSTS) header correct geconfigureerd?
- [ ] Ja — `max-age` is ingesteld op een lange duur (bijv. 2 jaar) en `includeSubDomains` is **ingeschakeld**  
- [ ] Ja — `max-age` is ingesteld, maar `includeSubDomains` is **uitgeschakeld**, waardoor subdomeinen kwetsbaar blijven  
- [ ] Nee — HSTS header is **niet aanwezig** of `max-age` is ingesteld op 0, wat protocol-downgrade-aanvallen mogelijk maakt  

### Voorkomt de Referrer-Policy het lekken van gevoelige URL-parameters?
- [ ] Ja — policy is ingesteld op `no-referrer` of `same-origin` *(Meest veilig)*  
- [ ] Ja — policy is ingesteld op `strict-origin-when-cross-origin`  
- [ ] Nee — policy is **niet ingesteld** of is ingesteld op `unsafe-url`, wat mogelijk tokens of gevoelige PII (Personally Identifiable Information) in referer-headers lekt  

### Voorkomt de X-Content-Type-Options header MIME-sniffing?
- [ ] Ja — `nosniff` directive is **aanwezig** en correct geïmplementeerd  
- [ ] Nee — header **ontbreekt**, wat een browser mogelijk toestaat om niet-uitvoerbare bestanden (bijv. afbeeldingen) als scripts uit te voeren

---

## WSTG-IDNT-01 — Testen van Roldefinities

Het testen van roldefinities omvat het identificeren en documenteren van de verschillende gebruikersrollen en machtigingsniveaus die binnen de applicatie zijn gedefinieerd om ervoor te zorgen dat deze logisch gescheiden zijn en voldoen aan het principe van de minste privileges (principle of least privilege). Een aanvaller analyseert de applicatie om rollen met hoge privileges versus standaard gebruikersrollen in kaart te brengen, op zoek naar onduidelijkheden in machtigingsgrenzen of ongedocumenteerde administratieve functies. Het identificeren van deze rollen is de eerste stap in het ontdekken van privilege-escalatiepaden, aangezien inconsistenties in roldefinities vaak leiden tot ongeoorloofde toegang tot gevoelige gegevens of administratieve functies. Deze fase vindt doorgaans plaats tijdens de initiële verkenningsfase en het in kaart brengen van de business logic, waarbij de focus ligt op gebruikersprofielen, administratieve dashboards en API-machtigingsstructuren.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst (Severity)** | Laag / Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Zijn rollen duidelijk gedefinieerd en gedocumenteerd in de applicatie of de bijbehorende documentatie?
- [ ] Ja — rollen zijn volledig gedocumenteerd en volgen het principe van **least privilege**  
- [ ] Ja — rollen zijn gedocumenteerd maar bevatten **overlappende machtigingen**  
- [ ] Nee — er bestaat geen formele documentatie of definitie van rollen  

### Is er een duidelijke scheiding tussen administratieve en standaard gebruikersfuncties?
- [ ] Ja — administratieve functies zijn **volledig geïsoleerd** van standaard gebruikersinterfaces  
- [ ] Ja — er is scheiding aanwezig, maar administratieve endpoints zijn **voorspelbaar of raadbaar**  
- [ ] Nee — administratieve en standaard functies zijn **vermengd** binnen dezelfde interfaces  

### Maakt de applicatie gebruik van een gecentraliseerd mechanisme voor Role-Based Access Control (RBAC)?
- [ ] Ja — gecentraliseerde RBAC of ABAC is **geïmplementeerd** en wordt consistent afgedwongen  
- [ ] Ja — er bestaan gedecentraliseerde controles, maar deze worden **inconsistent toegepast** over verschillende modules  
- [ ] Nee — de logica voor toegangscontrole is **hardcoded** in individuele pagina's of componenten  

### Zijn er ongedocumenteerde of verborgen rollen aanwezig in het systeem?
- [ ] Nee — alle ontdekte rollen komen overeen met de gedocumenteerde functionaliteit  
- [ ] Ja — verborgen of "shadow" rollen (bijv. `super_admin`, `support`, `test`) **zijn aanwezig**  

### Kan een gebruiker met lage privileges de machtigingen of rollen van andere gebruikers enumereren?
- [ ] Nee — gebruikers kunnen de rollen/machtigingen van andere entiteiten **niet** inzien of enumereren  
- [ ] Ja — gebruikers **kunnen** rollen enumereren via profielpagina's, metadata of API-responses *(Medium)*

---

## WSTG-IDNT-02 — Gebruikersregistratieproces testen

Het testen van het gebruikersregistratieproces omvat het evalueren van de workflow waarmee nieuwe identiteiten worden aangemaakt om te garanderen dat deze niet kan worden misbruikt voor ongeoorloofde toegang of geautomatiseerde exploitatie. Kwetsbaarheden in dit proces manifesteren zich vaak als het ontbreken van identiteitsverificatie (e-mail/SMS), gevoeligheid voor massale accountcreatie via bots, of informatiedisclosure via gedetailleerde foutmeldingen die bestaande gebruikersnamen onthullen. Aanvallers exploiteren deze tekortkomingen om User Enumeration uit te voeren, grootschalige spamcampagnes op te zetten of registratieparameters te manipuleren om verhoogde privileges te verkrijgen tijdens de accountcreatiefase. Deze test is essentieel voor het beschermen van de integriteit van de applicatie en het voorkomen dat aanvallers het systeem vullen met frauduleuze of kwaadaardige accounts.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Is identiteitsverificatie vereist voordat een nieuw account actief wordt?
- [ ] Ja — registratie vereist een uniek, tijdgebonden token verzonden via e-mail of SMS  
- [ ] Ja — verificatie is vereist, maar tokens zijn **zwak** of **voorspelbaar**  
- [ ] Nee — accounts worden **onmiddellijk** geactiveerd zonder enige verificatiestap  

### Voorkomt het registratieproces User Enumeration?
- [ ] Ja — de applicatie retourneert **identieke** reacties voor zowel nieuwe als bestaande e-mailadressen/gebruikersnamen  
- [ ] Nee — foutmeldingen (bijv. "E-mail al in gebruik") **maken het mogelijk** voor een aanvaller om geregistreerde gebruikers in kaart te brengen  
- [ ] Nee — timingverschillen in serverresponsies **onthullen** of een gebruiker bestaat  

### Zijn er anti-automatiseringscontroles geïmplementeerd om massale registratie te voorkomen?
- [ ] Ja — CAPTCHA of proof-of-work mechanismen zijn **aanwezig** en **effectief**  
- [ ] Ja — controles bestaan, maar een bypass **is mogelijk** via API-endpoints of header-manipulatie  
- [ ] Nee — er is geen Rate Limiting of CAPTCHA **toegepast** op het registratie-endpoint  

### Kunnen registratieparameters worden gemanipuleerd om privileges te escaleren?
- [ ] Nee — gebruikersrollen worden **strikt** toegewezen door de server-side logica  
- [ ] Ja — parameters zoals `role`, `admin` of `group_id` **kunnen** in het request worden aangepast om hogere toegang te verkrijgen  

### Wordt het wachtwoordbeleid afgedwongen tijdens de registratiefase?
- [ ] Ja — sterke wachtwoordvereisten worden server-side **afgedwongen**  
- [ ] Nee — zwakke wachtwoorden (bijv. "password123") worden door het systeem **geaccepteerd**

---

## WSTG-IDNT-03 — Test Account Provisioning Process

Het account provisioning proces beheert hoe nieuwe identiteiten worden aangemaakt, geverifieerd en hoe privileges worden toegewezen binnen een applicatieomgeving. Aanvallers richten zich op deze workflow om geautomatiseerde massaregistraties uit te voeren voor spam of Denial-of-Service (DoS), stappen voor identiteitsverificatie te omzeilen om frauduleuze accounts aan te maken, of parameters te manipuleren om zichzelf verhoogde rechten toe te kennen tijdens de registratiefase. Kwetsbaarheden manifesteren zich vaak in self-service registratieformulieren, systemen die enkel op uitnodiging werken, of administratieve provisioning-consoles waar logische fouten (Business Logic Flaws) ongeautoriseerde accountcreatie of privilege-escalatie mogelijk maken. Vanuit het perspectief van een aanvaller biedt het compromitteren van de provisioning-flow een legitieme voet aan de grond die kan worden gebruikt als uitvalsbasis voor verdere exploitatie, data-exfiltratie of Social Engineering-aanvallen.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Kritiek als een aanvaller administrator-accounts kan provisionen of globale registratiebeperkingen kan omzeilen.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### Wordt zelfregistratie strikt gecontroleerd of beperkt tot geautoriseerde gebruikers?
- [ ] Ja — registratie is **uitgeschakeld** of vereist handmatige administratieve goedkeuring  
- [ ] Ja — registratie is open maar beperkt tot **geautoriseerde** e-maildomeinen of uitnodigingstokens  
- [ ] Nee — registratie is **open** voor elke gebruiker zonder domein- of tokenbeperkingen  

### Is een robuust mechanisme voor identiteitsverificatie (bijv. e-mail/SMS) vereist vóór accountactivatie?
- [ ] Ja — verificatie is **vereist** en kan **niet** worden omzeild  
- [ ] Ja — verificatie is **vereist** maar **kan** worden omzeild via parameter tampering of voorspelbare tokens  
- [ ] Nee — accounts zijn **onmiddellijk actief** na indiening zonder enige verificatie  

### Kan de gebruiker rol- of permissieparameters manipuleren tijdens het registratieverzoek?
- [ ] Nee — rollen worden **aan de serverzijde toegewezen** en er bestaan geen parameters aan de clientzijde  
- [ ] Ja — rolparameters bestaan maar manipulatie is **niet mogelijk** vanwege validatie aan de serverzijde  
- [ ] Ja — rol-/permissieparameters (bijv. `role=admin`, `is_staff=true`) **kunnen** worden gewijzigd om verhoogde toegang te verkrijgen *(Kritiek)*  

### Zijn Rate Limiting of anti-automatisering controles geïmplementeerd om massale accountcreatie te voorkomen?
- [ ] Ja — Rate Limiting en CAPTCHA's zijn **actief** en effectief  
- [ ] Ja — controles bestaan maar omzeiling **is mogelijk** via header spoofing of CAPTCHA-oplossingsdiensten  
- [ ] Nee — er zijn **geen** Rate Limiting of anti-automatisering controles toegepast  

### Lekt het provisioning-proces informatie over bestaande accounts (User Enumeration)?
- [ ] Nee — antwoordberichten zijn **identiek** voor bestaande en niet-bestaande gebruikers  
- [ ] Ja — verschillende antwoorden of timingvariaties **maken** de enumeratie van bestaande gebruikers mogelijk

---

## WSTG-IDNT-04 — Testen op Account Enumeration en Raadbare Gebruikersaccounts

Account Enumeration treedt op wanneer een applicatie het bestaan of niet-bestaan van een gebruikersaccount onthult via variaties in HTTP-responses, foutmeldingen of meetbare tijdsverschillen. Aanvallers misbruiken dit gedrag door systematisch lijsten met gebruikersnamen in te dienen om geldige accounts te identificeren, die vervolgens het doelwit worden van Credential Stuffing, Brute Force-aanvallen of Social Engineering. Deze kwetsbaarheid komt vaak voor in inlogportalen, functies voor het herstellen van wachtwoorden, registratieformulieren en API-endpoints die gebruikersspecifieke identifiers verwerken. Vanuit het perspectief van een aanvaller verkleint succesvolle enumeratie het aanvalsoppervlak aanzienlijk door een blinde Brute Force-poging te transformeren in een gerichte exploitatie van bekende, geldige identiteiten.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Zijn de foutmeldingen bij authenticatie consistent in alle scenario's?
- [ ] Ja — er worden generieke foutmeldingen gebruikt voor zowel ongeldige gebruikersnamen als ongeldige wachtwoorden  
- [ ] Ja — de meldingen zijn vergelijkbaar, maar er **zijn** lichte variaties in interpunctie of hoofdlettergebruik aanwezig  
- [ ] Nee — foutmeldingen vermelden expliciet "Gebruiker niet gevonden" of "Onjuist wachtwoord" *(Kwetsbaar)*  

### Lekt de flow voor het herstellen van wachtwoorden of "wachtwoord vergeten" het bestaan van accounts?
- [ ] Nee — de applicatie retourneert een generieke melding, ongeacht of het e-mailadres of de gebruikersnaam bestaat  
- [ ] Ja — er zijn controles aanwezig, maar een bypass **is mogelijk** via de lengte van de response of statuscodes  
- [ ] Ja — de applicatie bevestigt expliciet of er een herstel-e-mail is verzonden of dat het account **niet bestaat**  

### Is het registratieproces beschermd tegen Account Enumeration?
- [ ] Nee — de registratie lekt geen informatie of gebruikt CAPTCHA/Rate Limiting om bulkcontroles te voorkomen  
- [ ] Ja — er zijn controles aanwezig, maar een bypass **is mogelijk** via tijdsverschillen of verschillen in API-responses  
- [ ] Ja — de applicatie stelt de gebruiker onmiddellijk op de hoogte als de gebruikersnaam of het e-mailadres **al in gebruik is**  

### Zijn er meetbare tijdsverschillen bij het vergelijken van geldige versus ongeldige gebruikersnamen?
- [ ] Nee — responstijden zijn consistent ongeacht de geldigheid van het account dankzij constant-time vergelijkingen  
- [ ] Ja — er zijn tijdsverschillen aanwezig, maar deze zijn te klein om betrouwbaar over het netwerk te worden gemeten  
- [ ] Ja — er **zijn** meetbare tijdsverschillen aanwezig (bijv. doordat password hashing alleen wordt uitgevoerd voor geldige gebruikers)  

### Maakt de applicatie gebruik van voorspelbare of raadbare naamgevingsconventies voor accounts?
- [ ] Nee — account-identifiers zijn willekeurig (UUIDs) of door de gebruiker gedefinieerd en complex  
- [ ] Ja — account-identifiers volgen een patroon (bijv. `emp001`, `emp002`) en enumeratie **is mogelijk**  
- [ ] Ja — identifiers zijn opeenvolgende gehele getallen, waardoor volledige database-enumeratie **mogelijk is**

---

## WSTG-IDNT-05 — Testen op zwak of niet-afgedwongen gebruikersnaambeleid

Het testen op zwak of niet-afgedwongen gebruikersnaambeleid richt zich op de regels voor het aanmaken van account-identifiers en hun gevoeligheid voor enumeratie (Enumeration) of spoofing. Zwak beleid staat vaak voorspelbare reeksen, algemene woordenboekwoorden of identifiers die interne werknemers-ID's spiegelen toe, wat de zoekruimte voor Brute Force- en Credential Stuffing-aanvallen aanzienlijk verkleint. Vanuit het perspectief van een aanvaller is het identificeren van deze patronen de eerste stap in grootschalige accountontdekking, wat vaak wordt bereikt door het analyseren van registratiefoutmeldingen, het gedrag van de wachtwoordreset-functionaliteit of metadata in openbare profielen. Het waarborgen van een robuust beleid voorkomt dat aanvallers systematisch het gebruikersbestand in kaart brengen en vergemakkelijkt de verdediging tegen gerichte phishing en geautomatiseerde pogingen tot Authentication bypass.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Zijn gebruikersnamen gebaseerd op zeer voorspelbare of sequentiële patronen?
- [ ] Nee — gebruikersnamen zijn willekeurig, door de gebruiker gedefinieerd of strings met een hoge entropie  
- [ ] Ja — gebruikersnamen volgen een voorspelbaar formaat (bijv. `user1001`, `user1002`), maar de authenticatie is robuust  
- [ ] Ja — gebruikersnamen zijn **strikt sequentieel** of volgen een bekend bedrijfsformaat, wat eenvoudige Enumeration mogelijk maakt  

### Handhaaft de applicatie minimale lengte- en complexiteitseisen voor gebruikersnamen?
- [ ] Ja — strikt beleid voor lengte en tekenset **wordt afgedwongen** tijdens de registratie  
- [ ] Ja — er is beleid, maar dit staat extreem korte (bijv. 1-2 tekens) of te eenvoudige gebruikersnamen toe  
- [ ] Nee — er wordt **geen beleid** afgedwongen met betrekking tot de lengte of het type tekens van de gebruikersnaam  

### Kunnen geldige gebruikersnamen worden geënumereerd via applicatieresponses?
- [ ] Nee — de applicatie retourneert generieke foutmeldingen en vertoont een consistente timing voor alle pogingen  
- [ ] Ja — de applicatie retourneert verschillende foutmeldingen (bijv. "Gebruikersnaam al in gebruik"), maar Rate Limiting **wordt toegepast**  
- [ ] Ja — de applicatie **lekt** geldige gebruikersnamen via registratiefouten, wachtwoordresets of timingverschillen  

### Voorkomt het beleid de registratie van algemene of gereserveerde administratieve gebruikersnamen?
- [ ] Ja — de applicatie blokkeert algemene namen (bijv. `admin`, `root`, `support`) en door het systeem gereserveerde termen  
- [ ] Nee — gebruikers **kunnen** accounts registreren met gevoelige of administratieve namen  

### Is het gebruikersnaambeleid consistent over alle toegangspunten (API, Mobiel, Web)?
- [ ] Ja — beleid **wordt consistent toegepast** over alle interfaces en versies  
- [ ] Nee — legacy-endpoints of specifieke API-versies **dwingen niet** dezelfde gebruikersnaambeperkingen af

---

## WSTG-ATHN-01 — Testen op het transport van inloggegevens via een versleuteld kanaal

Het testen op het transport van inloggegevens via een versleuteld kanaal waarborgt dat gevoelige authenticatiegegevens, zoals gebruikersnamen, wachtwoorden en sessietokens, beschermd zijn tegen onderschepping tijdens verzending. Aanvallers op het netwerk—bijvoorbeeld via Man-in-the-Middle (MitM) aanvallen op openbare Wi-Fi of gecompromitteerde interne netwerken—kunnen packet sniffing tools gebruiken om inloggegevens in cleartext te onderscheppen als TLS/SSL niet correct wordt afgedwongen. Deze kwetsbaarheid doet zich meestal voor bij login-endpoints, wachtwoord-resetformulieren en API-authenticatieheaders waar de applicatie nalaat HTTPS te verplichten of op onjuiste wijze doorverwijst vanaf HTTP. Succesvolle exploitatie geeft de aanvaller volledige toegang tot gebruikersaccounts, wat leidt tot een volledige compromittering van gebruikersgegevens en mogelijke lateral movement binnen de applicatie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Wordt HTTPS afgedwongen voor het gehele authenticatieproces?
- [ ] Ja — HSTS is **ingeschakeld** en al het verkeer wordt strikt over HTTPS gedwongen  
- [ ] Ja — omleiding naar HTTPS vindt plaats, maar HSTS is **niet ingeschakeld**  
- [ ] Nee — inloggegevens **kunnen** via onversleuteld HTTP worden verzonden  

### Worden inlogpagina's en formulieren aangeboden via een versleuteld kanaal?
- [ ] Ja — de pagina met het inlogformulier wordt via HTTPS geleverd  
- [ ] Nee — de inlogpagina wordt via HTTP geleverd, wat form-action hijacking of credential sniffing mogelijk maakt  

### Is de `Secure` flag toegepast op alle gevoelige sessie-cookies?
- [ ] Ja — de `Secure` flag is **toegepast** op alle authenticatie- en sessie-cookies  
- [ ] Ja — de `Secure` flag is slechts op **sommige** cookies toegepast  
- [ ] Nee — de `Secure` flag is **niet toegepast**, waardoor cookies via onversleutelde verzoeken kunnen worden geëxfiltreerd  

### Ondersteunt de server zwakke TLS-versies of onveilige cipher suites?
- [ ] Nee — alleen moderne TLS (1.2+) en sterke ciphers zijn **ingeschakeld**  
- [ ] Ja — legacy versies (TLS 1.0/1.1) of zwakke ciphers (RC4, DES, CBC) worden **ondersteund**  

### Voorkomt de applicatie de verzending van inloggegevens naar domeinen van derden via onversleutelde kanalen?
- [ ] Ja — er worden geen gevoelige gegevens naar externe domeinen verzonden of alle externe aanroepen worden over HTTPS gedwongen  
- [ ] Nee — authenticatieheaders of inloggegevens worden naar endpoints van derden verzonden (bijv. analytics of SSO) via HTTP

---

## WSTG-ATHN-02 — Testen op standaard inloggegevens (Default Credentials)

Het testen op standaard inloggegevens (Default Credentials) omvat het identificeren van administratieve interfaces, diensten of applicatiecomponenten die nog steeds gebruikmaken van door de fabriek ingestelde of door de leverancier verstrekte gebruikersnamen en wachtwoorden. Deze kwetsbaarheid is een doelwit met een hoge prioriteit voor aanvallers, omdat het vaak een directe weg biedt naar een volledige systeemcompromis, administratieve toegang of Remote Code Execution (RCE) met minimale inspanning. Dit komt doorgaans voor in kant-en-klare software (off-the-shelf), CMS-platforms, databasebeheertools en geïntegreerde hardware zoals IoT-apparaten of netwerkapparatuur. Exploitatie wordt meestal uitgevoerd door geïdentificeerde softwareversies te vergelijken met openbare databases voor standaardwachtwoorden of door gebruik te maken van geautomatiseerde `Credential Stuffing` tools tijdens de initiële verkennings- en exploitatiefasen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst / Severity** | Kritiek / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Zijn administratieve of beheerinterfaces blootgesteld aan het internet of aan onbetrouwbare netwerksegmenten?
- [ ] Nee — er zijn geen beheerinterfaces toegankelijk  
- [ ] Ja — interfaces zijn aanwezig, maar beperkt door controles op netwerkniveau (bijv. VPN, IP Allowlist)  
- [ ] Ja — interfaces **zijn** publiek toegankelijk en vereisen authenticatie  

### Zijn de door de leverancier geleverde standaard inloggegevens gewijzigd voor alle geïdentificeerde componenten?
- [ ] Ja — alle standaard inloggegevens **zijn gewijzigd** voor alle componenten *(Meest veilig)*  
- [ ] Ja — inloggegevens **zijn gewijzigd** voor de hoofdapplicatie, maar secundaire componenten (bijv. CMS, DB) maken nog gebruik van de standaardinstellingen  
- [ ] Nee — standaard inloggegevens **zijn actief** op een of meer identificeerbare interfaces *(Kritiek)*  

### Dwingt de applicatie een wachtwoordwijziging af bij de eerste login voor nieuwe of administratieve accounts?
- [ ] Ja — een wachtwoordwijziging **is verplicht** voordat er administratieve acties kunnen worden ondernomen  
- [ ] Nee — de applicatie **staat** het gebruik van standaard inloggegevens voor onbepaalde tijd **toe**  

### Zijn lockout-mechanismen of Rate Limiting-controles actief om geautomatiseerd testen van standaard inloggegevens te voorkomen?
- [ ] Ja — agressieve Rate Limiting of account lockout **wordt toegepast**  
- [ ] Ja — controles zijn aanwezig, maar een bypass **is mogelijk** via header-manipulatie of IP-rotatie  
- [ ] Nee — er zijn geen Rate Limiting- of lockout-mechanismen **ingeschakeld**  

### Maken plug-ins van derden, middleware of administratieve tools (bijv. phpMyAdmin, Tomcat Manager) gebruik van unieke inloggegevens?
- [ ] Nee — er zijn geen componenten van derden aanwezig in de omgeving  
- [ ] Ja — alle componenten van derden gebruiken unieke, niet-standaard inloggegevens  
- [ ] Nee — ten minste één component van derden **is toegankelijk** via bekende standaard inloggegevens

---

## WSTG-ATHN-03 — Testen op zwakke accountvergrendelingsmechanismen

Een accountvergrendelingsmechanisme is een cruciaal defense-in-depth controlemechanisme, ontworpen om Brute Force- en Credential Stuffing-aanvallen te beperken door een account uit te schakelen na een bepaald aantal mislukte authenticatiepogingen. Zonder een robuust vergrendelingsbeleid kunnen aanvallers geautomatiseerde tools gebruiken om systematisch wachtwoorden te raden voor meerdere accounts (Password Spraying) of een specifiek account met een hoge waarde te targeten tot de poging slaagt. Deze kwetsbaarheid manifesteert zich doorgaans op inlogportalen, formulieren voor het opnieuw instellen van wachtwoorden en API-endpoints die geen Rate Limiting of beveiliging op accountniveau hebben. Vanuit het perspectief van een aanvaller maakt een zwak of ontbrekend vergrendelingsmechanisme een oneindig aantal wachtwoordpogingen mogelijk, terwijl een te agressief mechanisme kan worden misbruikt om een distributed Denial of Service (DoS) tegen legitieme gebruikers te verooraken door hun accounts opzettelijk te vergrendelen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt "Hoog" als de applicatie gevoelige PII, financiële gegevens verwerkt, of als beheerdersaccounts vatbaar zijn voor Brute Force zonder secundaire controles.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Wordt er een accountvergrendelingsmechanisme afgedwongen na een vastgesteld aantal mislukte pogingen?
- [ ] Ja — account wordt vergrendeld na een gedefinieerde, redelijke drempelwaarde (bijv. 5-10 pogingen)  
- [ ] Ja — account wordt vergrendeld, maar pas na een buitengewoon hoog aantal pogingen  
- [ ] Nee — account **kan niet** worden vergrendeld en staat onbeperkte Brute Force toe  

### Kan het vergrendelingsmechanisme worden omzeild met gangbare ontwijkingstechnieken?
- [ ] Nee — vergrendeling wordt aan de serverzijde afgedwongen en wordt **niet** beïnvloed door wijzigingen aan de clientzijde  
- [ ] Ja — vergrendeling **kan** worden omzeild door IP-adressen te roteren of een proxy pool te gebruiken  
- [ ] Ja — vergrendeling **kan** worden omzeild door HTTP-headers zoals `X-Forwarded-For` of `User-Agent` te manipuleren  
- [ ] Ja — vergrendeling **kan** worden omzeild door cookies te verwijderen of nieuwe sessies te starten  

### Biedt het vergrendelingsmechanisme een mogelijkheid voor accountherstel of verloop?
- [ ] Ja — account wordt automatisch ontgrendeld na een ingestelde tijdsduur of via e-mailverificatie  
- [ ] Ja — account vereist tussenkomst van een beheerder om te ontgrendelen  
- [ ] Nee — vergrendeling is permanent zonder duidelijk herstelpad, wat het DoS-risico verhoogt  

### Maakt de applicatie tijdens de vergrendeling onderscheid tussen een geldige en ongeldige gebruikersnaam?
- [ ] Nee — de respons van de applicatie is identiek voor zowel bestaande als niet-bestaande gebruikers  
- [ ] Ja — de applicatie onthult of een account is vergrendeld, wat **Username Enumeration** mogelijk maakt  

### Is het vergrendelingsmechanisme vatbaar voor een Denial of Service (DoS)-aanval?
- [ ] Nee — controles zoals CAPTCHA of IP-gebaseerde Rate Limiting voorkomen massale accountvergrendeling  
- [ ] Ja — een aanvaller **kan** systematisch alle bekende gebruikers vergrendelen door onjuiste wachtwoorden op te geven

---

## WSTG-ATHN-04 — Testing for Bypassing Authentication Schema

Het omzeilen van het authenticatieschema omvat het identificeren en exploiteren van fouten in de applicatielogica die een aanvaller in staat stellen toegang te krijgen tot beschermde bronnen zonder geldige credentials te verstrekken. Deze kwetsbaarheid treedt meestal op wanneer de applicatie vertrouwt op client-side controles, nalaat om server-side autorisatie af te dwingen bij elk verzoek, of wanneer administratieve "backdoors" en debug endpoints openstaan in productieomgevingen. Aanvallers exploiteren deze zwakheden door forced browsing uit te voeren naar gevoelige URL's, HTTP-parameters zoals `authenticated=true` te manipuleren, of headers te spoofen om de applicatie te misleiden zodat deze aanneemt dat er al een sessie tot stand is gebracht. Succesvolle exploitatie resulteert in een volledige ineenstorting van de authenticatieperimeter, wat kan leiden tot ongeautoriseerde data-exfiltratie of een volledige systeemcompromis.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Kunnen gevoelige interne pagina's rechtstreeks worden bezocht zonder een actieve sessie?
- [ ] Nee — directe toegang resulteert in een omleiding naar login of een 403/404-respons  
- [ ] Ja — sommige gevoelige pagina's zijn toegankelijk maar verstrekken beperkte of niet-gevoelige gegevens  
- [ ] Ja — gevoelige administratieve of gebruikersspecifieke pagina's **zijn** volledig toegankelijk zonder authenticatie *(Kritiek)*  

### Vertrouwt de applicatie op client-side flags of parameters om de authenticatiestatus te bepalen?
- [ ] Nee — authenticatiestatus wordt strikt beheerd via de server-side sessiestatus  
- [ ] Ja — client-side flags zijn aanwezig, maar modificatie hiervan **verleent geen** toegang tot beschermde gebieden  
- [ ] Ja — het aanpassen van parameters (bijv. `is_authenticated=true`, `user_role=admin`) **maakt** het omzeilen van de authenticatie **mogelijk**  

### Kan authenticatie worden omzeild door het spoofen of manipuleren van specifieke HTTP-headers?
- [ ] Nee — headers zoals `X-Forwarded-For`, `Referer` of aangepaste headers hebben geen invloed op de authenticatie  
- [ ] Ja — het manipuleren van headers (bijv. `X-Custom-IP-Authorization`, `X-Remote-User`) **is mogelijk** en verleent ongeautoriseerde toegang  

### Zijn er alternatieve paden of ontwikkelingsartefacten die de standaard inlogflow omzeilen?
- [ ] Nee — alle beschermde bronnen worden beveiligd door een gecentraliseerde, productie-klare authenticatiecontrole  
- [ ] Ja — legacy endpoints, debug-routes (bijv. `/debug/login`) of vergeten API-versies **zijn** toegankelijk zonder credentials  

### Verzuimt de applicatie om opnieuw te authenticeren of sessies te verifiëren voor acties met hoge privileges?
- [ ] Nee — acties met hoge privileges of gevoelige acties vereisen een vers of geldig sessietoken  
- [ ] Ja — zodra een bypass is gevonden op één endpoint, **kan** deze worden gebruikt om administratieve acties uit te voeren in de gehele applicatie

---

## WSTG-ATHN-05 — Testen op kwetsbare 'Onthoud wachtwoord'-functionaliteit

De "Onthoud mij"- of persistente inlogfunctionaliteit is ontworpen om de geauthenticeerde status van een gebruiker te behouden na het herstarten van de browser door een token of inloggegeven aan de client-zijde op te slaan. Als deze functie slecht is geïmplementeerd — bijvoorbeeld door het opslaan van wachtwoorden in platte tekst, zwakke hashes of tokens met lage entropie in cookies — stelt dit de gebruiker bloot aan accountovername via Cross-Site Scripting (XSS), fysieke toegang of netwerkintersceptie. Pentesters evalueren de entropie van deze persistente tokens, de security flags die op het opslagmechanisme zijn toegepast en de levenscyclus van de sessie om te garanderen dat het gemak van persistente toegang geen kritieke beveiligingscontroles zoals multifactorauthenticatie (MFA) of sessie-invalidatie omzeilt. Exploitatie omvat vaak het verzamelen van deze langdurige tokens om ongeautoriseerde toegang tot accounts te verkrijgen zonder dat de originele inloggegevens vereist zijn.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt 'Hoog' als inloggegevens in platte tekst of hashes zonder salt in de persistente cookie worden opgeslagen, of als het token Multifactorauthenticatie (MFA) omzeilt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### Implementeert de applicatie een "Onthoud mij"- of "Ingelogd blijven"-functie?
- [ ] Nee — functie bestaat **niet** in de applicatie  
- [ ] Ja — functie bestaat en maakt gebruik van cryptografisch sterke, niet-voorspelbare tokens  
- [ ] Ja — functie bestaat maar maakt gebruik van voorspelbare tokens of tokens met lage entropie *(Gemiddeld)*  
- [ ] Ja — functie bestaat en slaat inloggegevens in platte tekst of base64-gecodeerde wachtwoorden op *(Hoog)*  

### Zijn de security flags correct toegepast op de persistente cookie?
- [ ] Ja — `HttpOnly`, `Secure`, en `SameSite` flags zijn **toegepast**  
- [ ] Ja — sommige flags zijn toegepast, maar `HttpOnly` **ontbreekt**  
- [ ] Ja — sommige flags zijn toegepast, maar `Secure` **ontbreekt** over HTTPS  
- [ ] Nee — er zijn **geen** security flags toegepast op de persistente cookie  

### Blijft het "Onthoud mij"-token geldig na een handmatige afmelding?
- [ ] Nee — token wordt onmiddellijk na uitloggen aan de serverzijde geïnvalideerd  
- [ ] Ja — token blijft geldig, maar vereist een wachtwoord voor gevoelige acties  
- [ ] Ja — token blijft geldig en biedt volledige toegang tot het account **zonder** herauthenticatie *(Gemiddeld)*  

### Wordt de persistente sessie beëindigd na een wachtwoordwijziging?
- [ ] Ja — alle persistente tokens worden ingetrokken wanneer de gebruiker zijn wachtwoord wijzigt  
- [ ] Nee — het "Onthoud mij"-token blijft geldig nadat een wachtwoordreset of -wijziging **is uitgevoerd** *(Hoog)*  

### Omzeilt het persistente token de Multifactorauthenticatie (MFA) bij volgende bezoeken?
- [ ] Nee — MFA is vereist, zelfs wanneer een "Onthoud mij"-token aanwezig is  
- [ ] Ja — MFA wordt omzeild, maar het token is gekoppeld aan een specifieke device fingerprint  
- [ ] Ja — MFA wordt **volledig omzeild** met enkel de persistente cookie vanaf elk willekeurig apparaat *(Hoog)*

---

## WSTG-ATHN-06 — Testen op zwakheden in browsercache

Een zwakheid in de browsercache treedt op wanneer gevoelige informatie wordt opgeslagen in de lokale browsercache en kan worden opgehaald door een onbevoegde gebruiker met toegang tot dezelfde fysieke machine. Dit gebrek aan de juiste cache-control headers zorgt ervoor dat potentieel gevoelige gegevens, zoals persoonlijke informatie, accountgegevens of sessie-identificatoren, behouden blijven nadat de gebruiker is uitgelogd of de sessie heeft gesloten. Aanvallers of volgende gebruikers op gedeelde of publieke terminals kunnen hiervan misbruik maken door door de browsergeschiedenis te navigeren, de "Terug"-knop te gebruiken of lokale cachebestanden te inspecteren om gecachte antwoorden te exfiltreren. Het waarborgen van de implementatie van strikte richtlijnen zoals `Cache-Control: no-store` and `Pragma: no-cache` is essentieel voor elke applicatie die geauthenticeerde inhoud verwerkt, om te garanderen dat gevoelige gegevens nooit naar de schijf worden geschreven.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Medium* |

> *De ernst wordt Medium als de applicatie regelmatig wordt geopend vanaf gedeelde/publieke kiosken of zeer gevoelige PII/PHI-gegevens bevat.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Tools:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Zijn er geschikte cache-control headers aanwezig op geauthenticeerde of gevoelige pagina's?
- [ ] Ja — `Cache-Control: no-store, no-cache` en `Pragma: no-cache` zijn **aanwezig** en **correct geconfigureerd**  
- [ ] Ja — sommige headers zijn aanwezig, maar `no-store` **ontbreekt**, wat caching op de schijf toestaat  
- [ ] Nee — er zijn geen cache-gerelateerde headers **toegepast**, waardoor wordt vertrouwd op het standaardgedrag van de browser  

### Gebruikt de applicatie de `private` richtlijn voor gebruikersspecifieke gegevens?
- [ ] Ja — `Cache-Control: private` is **ingeschakeld** voor alle gepersonaliseerde inhoud om proxy-caching te voorkomen  
- [ ] Nee — inhoud is gemarkeerd als `public` of de `private` richtlijn ontbreekt, waardoor het **gecachet** kan worden door tussenliggende proxy's  

### Is gevoelige informatie toegankelijk via de "Terug"-knop van de browser na een succesvolle afmelding?
- [ ] Nee — de browser vraagt om hernieuwde authenticatie of de pagina wordt **niet geladen** vanuit de lokale cache  
- [ ] Ja — gevoelige informatie is **nog steeds zichtbaar** via de terug-knop nadat de sessie is beëindigd  

### Zijn gevoelige niet-HTML-bestanden (bijv. PDF's, CSV-exports, JSON API-antwoorden) beschermd tegen caching?
- [ ] Nee — er worden geen gevoelige niet-HTML-bestanden gegenereerd of verwerkt door de applicatie  
- [ ] Ja — strikte cache-control headers zijn **toegepast** op alle gevoelige bestandsdownloads en API-endpoints  
- [ ] Nee — gevoelige downloads of API-antwoorden worden **opgeslagen** in de lokale browsercache of tijdelijke mappen  

### Gebruikt de applicatie `Expires: 0` of een datum in het verleden om het gebruik van verouderde gegevens te voorkomen?
- [ ] Ja — de `Expires` header is ingesteld op `0` of een historische datum, wat **garandeert** dat de browser de inhoud als onmiddellijk verouderd beschouwt  
- [ ] Nee — de `Expires` header **ontbreekt** of is ingesteld op een datum in de toekomst voor gevoelige pagina's

---

## WSTG-ATHN-07 — Testen op Zwak Wachtwoordbeleid

Het testen op een zwak wachtwoordbeleid omvat het evalueren van de vereisten voor complexiteit, lengte en entropie die door een applicatie worden afgedwongen tijdens het aanmaken van accounts en het bijwerken van inloggegevens. Onvoldoende beleid vormt een directe bedreiging voor de vertrouwelijkheid van gebruikersaccounts, omdat deze hierdoor vatbaar worden voor geautomatiseerde Dictionary-aanvallen, Brute Force-pogingen en Credential Stuffing. Deze kwetsbaarheden bevinden zich doorgaans in registratie-endpoints, wachtwoordreset-workflows en administratieve beheerderspanelen. Vanuit het perspectief van een aanvaller verlaagt een laks beleid aanzienlijk de computationele kosten en de tijd die nodig is om inloggegevens succesvol te compromitteren met behulp van gangbare wordlists of patroongebaseerd kraken.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Gemiddeld / Laag* |

> *De ernst wordt Hoog als de applicatie geen mechanismen voor account lockout of Rate Limiting heeft om geautomatiseerd raden te voorkomen.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Tools:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Wordt een minimale wachtwoordlengte afgedwongen door de applicatie?
- [ ] Ja — minimale lengte is 12+ tekens *(Meest veilig)*  
- [ ] Ja — minimale lengte ligt tussen 8 en 11 tekens  
- [ ] Ja — minimale lengte is **minder dan** 8 tekens  
- [ ] Nee — er wordt geen minimale lengte afgedwongen  

### Worden complexiteitsvereisten (hoofdletters, kleine letters, cijfers, symbolen) afgedwongen?
- [ ] Ja — meerdere tekenklassen zijn verplicht en een bypass is **niet mogelijk**  
- [ ] Ja — tekenklassen worden gesuggereerd maar **niet** afgedwongen  
- [ ] Nee — er worden geen complexiteitsvereisten toegepast  

### Blokkeert de applicatie veelvoorkomende/zwakke wachtwoorden of wachtwoorden die gebruikersspecifieke gegevens bevatten?
- [ ] Ja — bekende zwakke wachtwoorden (bijv. "password123") en op de gebruikersnaam gebaseerde wachtwoorden **kunnen niet** worden gebruikt  
- [ ] Ja — veelvoorkomende wachtwoorden worden geblokkeerd, maar gebruikersspecifieke gegevens (bijv. gebruikersnaam, e-mail) **kunnen** worden gebruikt  
- [ ] Nee — elke string die voldoet aan de lengte-/complexiteitsvereisten wordt geaccepteerd  

### Kunnen complexiteits- of lengtevereisten worden omzeild via client-side aanpassingen?
- [ ] Nee — beleid wordt strikt afgedwongen aan de **server-side**  
- [ ] Ja — beleid wordt afgedwongen via JavaScript en **kan** worden omzeild door het verzoek te onderscheppen  

### Wordt het wachtwoordbeleid duidelijk gecommuniceerd naar de gebruiker zonder implementatiedetails te lekken?
- [ ] Ja — beleid is zichtbaar tijdens de invoer en geeft generieke foutmeldingen  
- [ ] Ja — beleid is zichtbaar, maar foutmeldingen onthullen specifieke regex- of logicagaten  
- [ ] Nee — beleid is **niet** zichtbaar, wat dwingt tot ontdekking via trial-and-error

---

## WSTG-ATHN-08 — Testen op zwakke antwoorden op beveiligingsvragen

Het testen op zwakke antwoorden op beveiligingsvragen omvat het evalueren van de knowledge-based authentication (KBA) mechanismen die worden gebruikt om de identiteit van een gebruiker te verifiëren, meestal tijdens wachtwoordherstel of multi-factor authentication flows. Dit is belangrijk omdat beveiligingsvragen vaak gebaseerd zijn op statische, niet-geheime informatie die eenvoudig kan worden achterhaald via open-source intelligence (OSINT) of social engineering, wat kan leiden tot ongeautoriseerde account takeover. Deze kwetsbaarheden treden meestal op in de "Wachtwoord vergeten" of "Account ontgrendelen" modules waar gebruikers antwoorden geven op vooraf ingestelde vragen. Vanuit het perspectief van een aanvaler is dit een belangrijk doelwit voor geautomatiseerde Brute Force of gericht onderzoek, aangezien veel gebruikers voorspelbare of publiekelijk verifieerbare antwoorden geven op veelvoorkomende vragen zoals "Wat is de meisjesnaam van je moeder?" of "Naar welke middelbare school ging je?".


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als de beveiligingsvraag de enige vereiste is voor een volledige wachtwoordreset en account takeover zonder verdere verificatie.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Tools:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### Implementeert de applicatie beveiligingsvragen voor identiteitsverificatie of wachtwoordherstel?
- [ ] Nee — beveiligingsvragen worden **niet gebruikt** voor authenticatie of herstel  
- [ ] Ja — beveiligingsvragen worden **gebruikt** als een optionele secundaire factor  
- [ ] Ja — beveiligingsvragen worden **gebruikt** als het primaire/enige herstelmechanisme *(Hoog risico)*  

### Worden beveiligingsvragen geselecteerd uit een vaste lijst of zijn ze door de gebruiker gedefinieerd?
- [ ] Nee — vragen zijn volledig door de gebruiker gedefinieerd en vereisen hoge entropie  
- [ ] Ja — vragen zijn vast maar bevatten opties die niet via OSINT te achterhalen zijn  
- [ ] Ja — vragen komen uit een vaste lijst van veelvoorkomende, via OSINT te achterhalen onderwerpen *(Kwetsbaar)*  

### Wordt Rate Limiting of account lockout toegepast op pogingen om de beveiligingsvraag te beantwoorden?
- [ ] Ja — strikte Rate Limiting en account lockout **worden toegepast** om Brute Force te voorkomen  
- [ ] Ja — Rate Limiting **wordt toegepast** maar bypass **is mogelijk** via IP-rotatie of header-manipulatie  
- [ ] Nee — er wordt **geen Rate Limiting** afgedwongen op antwoordpogingen  

### Handhaaft de applicatie complexiteit of validatie op de inhoud van het antwoord?
- [ ] Ja — validatie voorkomt algemene woorden en waarborgt complexiteit/uniciteit van het antwoord  
- [ ] Ja — basisvalidatie is aanwezig maar **kan** worden omzeild met algemene woordenlijsten of dictionary attacks  
- [ ] Nee — er wordt **geen validatie** uitgevoerd; eenvoudige antwoorden of antwoorden van één enkel teken **zijn toegestaan**  

### Kan de beveiligingsvraag-challenge worden omzeild via logische fouten?
- [ ] Nee — de herstelflow vereist strikt een geldig antwoord om door te gaan  
- [ ] Ja — de herstelflow **kan** worden omzeild door het manipuleren van HTTP response codes of sessieparameters  
- [ ] Ja — het antwoord wordt gereflecteerd in de broncode of verborgen velden van de pagina

---

## WSTG-ATHN-09 — Testen op Zwakke Functionaliteiten voor Wachtwoordwijziging of -herstel

Mechanismen voor het wijzigen en herstellen van wachtwoorden zijn cruciale onderdelen van authenticatiebeheer die, indien onjuist geïmplementeerd, aanvallers in staat stellen gebruikersaccounts over te nemen. Deze kwetsbaarheden omvatten doorgaans voorspelbare herstel-tokens, het ontbreken van accountblokkades tijdens brute-force pogingen, onveilige verzending van tokens, of de mogelijkheid om wachtwoorden te wijzigen zonder het huidige wachtwoord te verifiëren. Aanvallers misbruiken deze gebreken door herstellinks te onderscheppen of te raden, Host Header Injection uit te voeren om herstel-e-mails om te leiden, of session fixation te gebruiken om toegang te behouden na een wachtwoordwijziging. Het waarborgen dat deze processen sterke verificatie vereisen en gebruikmaken van cryptografische willekeurigheid is essentieel voor het behoud van accountintegriteit en het voorkomen van ongeautoriseerde toegang.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Is het huidige wachtwoord vereist om een nieuw wachtwoord in te stellen tijdens een standaard wijziging?
- [ ] Ja — het huidige wachtwoord **is vereist** en wordt geverifieerd  
- [ ] Ja — het huidige wachtwoord **is vereist** maar kan worden omzeild via parameter pollution of manipulatie  
- [ ] Nee — het huidige wachtwoord **wordt niet** gevraagd, wat accountovername via session hijacking mogelijk maakt *(Hoog)*  

### Zijn wachtwoordherstel-tokens cryptografisch veilig en onvoorspelbaar?
- [ ] Ja — tokens hebben een hoge entropie, zijn willekeurig en **niet voorspelbaar**  
- [ ] Ja — tokens worden gegenereerd maar vertonen een lage entropie of voorspelbare patronen (bijv. op basis van tijdstempel)  
- [ ] Nee — tokens zijn **voorspelbaar** of gebaseerd op statische gebruikersgegevens zoals Base64-gecodeerde e-mail/ID *(Kritiek)*  

### Kan de wachtwoordherstellink worden omgeleid via Host Header Injection?
- [ ] Nee — de applicatie negeert of valideert de `Host`-header voor het genereren van links  
- [ ] Ja — de applicatie gebruikt de `Host`- of `X-Forwarded-Host`-header om links op te bouwen, maar validatie **is aanwezig**  
- [ ] Ja — links **kunnen** worden omgeleid naar een door de aanvaller beheerd domein via header-manipulatie *(Hoog)*  

### Heeft de herstel-token een beperkte levensduur en onmiddellijke ongeldigverklaring?
- [ ] Ja — tokens verlopen snel en worden **onmiddellijk** ongeldig verklaard na gebruik of wachtwoordwijziging  
- [ ] Ja — tokens verlopen na een lange tijdsduur (bijv. 24+ uur) of staan **meervoudig gebruik** toe  
- [ ] Nee — tokens **verlopen nooit** of blijven geldig nadat het wachtwoord succesvol is gewijzigd  

### Zijn er rate limits of blokkades op de eindpunten voor wachtwoordherstel en -wijziging?
- [ ] Ja — strikte rate limiting en CAPTCHA's **worden toegepast** om enumeratie en brute-force te voorkomen  
- [ ] Ja — er bestaan beperkte controles, maar omzeiling **is mogelijk** via IP-rotatie of header-spoofing  
- [ ] Nee — er wordt geen rate limiting **toegepast**, wat bulk account-enumeratie of token brute-forcing mogelijk maakt

---

## WSTG-ATHN-10 — Testen op Zwakkere Authenticatie in Alternatieve Kanalen

Het testen op zwakkere authenticatie in alternatieve kanalen omvat het identificeren en analyseren van secundaire paden naar accounttoegang — zoals mobiele API's, wachtwoordherstelflows, helpdesk-interfaces of legacy-subdomeinen — die mogelijk niet dezelfde strikte beveiliging hanteren als het primaire webportaal. Deze alternatieve kanalen missen vaak robuuste multi-factor authenticatie (MFA), strikte rate limiting of complexe wachtwoordvereisten, waardoor een "zwakste schakel" ontstaat die het gehele authenticatiesysteem ondermijnt. Aanvallers richten zich specifiek op deze over het hoofd geziene toegangspunten om credential stuffing uit te voeren of MFA te omzeilen door misbruik te maken van discrepanties in de manier waarop verschillende interfaces identiteitsverificatie afhandelen. Het waarborgen van pariteit over alle authenticatie-oppervlakken is essentieel om ongeautoriseerde toegang te voorkomen en de integriteit van de sessie en gegevens van de gebruiker te behouden.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Zijn er alternatieve authenticatiekanalen aanwezig en geïdentificeerd?
- [ ] Nee — er bestaat slechts één enkel authenticatiekanaal  
- [ ] Ja — meerdere kanalen geïdentificeerd (bijv. mobiele app API, desktop-client, legacy-portaal, SOAP-services)  

### Dwingen alternatieve kanalen dezelfde beveiligingspariteit af als het primaire kanaal?
- [ ] Ja — alle kanalen dwingen **identieke** wachtwoordcomplexiteit en MFA-vereisten af  
- [ ] Ja — kanalen dwingen wachtwoordcomplexiteit af, maar MFA is **niet vereist** of **kan** worden omzeild  
- [ ] Nee — alternatieve kanalen hebben **aanzienlijk zwakkere** authenticatievereisten  

### Zijn rate limiting en account lockout consistent over alle kanalen?
- [ ] Ja — rate limiting en lockout worden **consistent afgedwongen** op alle eindpunten  
- [ ] Ja — controles zijn aanwezig, maar omzeiling **is mogelijk** op specifieke kanalen (bijv. mobiele API)  
- [ ] Nee — rate limiting of lockout wordt **niet toegepast** op secundaire authenticatiepaden  

### Kan MFA worden omzeild door over te schakelen naar een alternatief kanaal?
- [ ] Nee — MFA is verplicht en **kan niet** worden omzeild, ongeacht het toegangspunt  
- [ ] Ja — MFA is vereist op het webportaal, maar wordt **niet afgedwongen** op de mobiele of legacy-API  

### Zijn wachtwoordherstel- of "wachtwoord vergeten"-flows zwakker dan de standaard login?
- [ ] Nee — herstelflows maken gebruik van sterke, out-of-band verificatie en lekken geen informatie  
- [ ] Ja — herstelflows maken gebruik van zwakke beveiligingsvragen die eenvoudig kunnen worden geraden of opgezocht  
- [ ] Ja — herstelflows zijn kwetsbaar voor enumeratie of **vereisen niet** hetzelfde verificatieniveau als een standaard login

---

## WSTG-ATHN-11 — Testen van Multi-Factor Authenticatie (MFA)

Het testen van Multi-Factor Authenticatie (MFA) evalueert de robuustheid van de secundaire beveiligingslaag die is ontworpen om ongeautoriseerde toegang te voorkomen, zelfs wanneer primaire inloggegevens zijn gecompromitteerd. Aanvallers proberen regelmatig MFA te omzeilen door eindpunten te identificeren waar dit inconsistent wordt afgedwongen, zoals legacy API-versies, mobiele back-ends of wachtwoordherstel-workflows. Exploitatie omvat vaak het manipuleren van server-responses, het brute-forcen van kortstondige codes (OTP) of het misbruiken van race conditions en session management-fouten waardoor een gebruiker kan overgaan naar een geauthenticeerde status zonder de tweede factor op te geven.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### Wordt MFA consistent afgedwongen op alle authenticatieportalen?
- [ ] Ja — MFA is vereist voor alle web-, mobiele en op API gebaseerde inlogpogingen  
- [ ] Ja — maar MFA wordt **niet afgedwongen** op specifieke eindpunten (bijv. legacy `/api/v1/login` of mobiel-specifieke portalen)  
- [ ] Nee — MFA is **niet geïmplementeerd** in de applicatie *(Kritiek)*  

### Kan de MFA-verificatiestap worden omzeild via directe URL-navigatie of response-manipulatie?
- [ ] Nee — directe navigatie of parameter tampering **kan** de challenge niet omzeilen  
- [ ] Ja — direct navigeren naar interne dashboard-URL's **is mogelijk** zonder de MFA-challenge te voltooien  
- [ ] Ja — het manipuleren van de server-response (bijv. het wijzigen van een `401 Unauthorized` naar `200 OK`) **is mogelijk** om toegang te verkrijgen  

### Is het verificatieproces van de MFA-code beschermd tegen brute-force aanvallen?
- [ ] Ja — strikte Rate Limiting of account-lockout **wordt toegepast** na meerdere mislukte OTP-pogingen  
- [ ] Ja — Rate Limiting bestaat, maar het **is mogelijk** om dit te omzeilen via IP-rotatie of header-manipulatie  
- [ ] Nee — er wordt geen Rate Limiting afgedwongen en geautomatiseerd Brute Force van codes **is mogelijk**  

### Handhaaft de applicatie een veilige sessiestatus tijdens de MFA-transitie?
- [ ] Ja — sessietokens met hoge privileges worden **pas na** succesvolle voltooiing van MFA uitgegeven  
- [ ] Nee — een volledig functionele sessie-cookie wordt uitgegeven **voordat** MFA is voltooid, wat toegang tot sommige functies mogelijk maakt  
- [ ] Nee — de applicatie gebruikt een statische identifier of voorspelbare sessietransitie die **kan** worden overgenomen (hijacked)  

### Zijn alternatieve MFA-factoren (SMS, e-mail, back-upcodes) kwetsbaar voor exploitatie?
- [ ] Nee — alle factoren maken gebruik van veilige, niet-voorspelbare en tijdgebonden waarden  
- [ ] Ja — back-upcodes zijn voorspelbaar of **kunnen** worden geënumereerd  
- [ ] Ja — de "Code opnieuw verzenden"-functionaliteit kan worden misbruikt om SMS/e-mail flooding uit te voeren of gedeeltelijke contactgegevens te onthullen

---

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

## WSTG-ATHZ-02 — Testen op het omzeilen van het autorisatieschema

Het omzeilen van autorisatieschema's vindt plaats wanneer een applicatie er niet in slaagt toegangscontroles af te dwingen, waardoor een aanvaller toegang krijgt tot resources of functies buiten de beoogde permissies. Deze kwetsbaarheid manifesteert zich doorgaans als Horizontal Privilege Escalation, waarbij een aanvaller toegang krijgt tot gegevens van een andere gebruiker met dezelfde rang, of Vertical Privilege Escalation, waarbij een gebruiker met lage privileges administratieve mogelijkheden verkrijgt. Aanvallers misbruiken deze gebreken door verzoekparameters zoals resource IDs te manipuleren, sessietokens te wijzigen of HTTP-headers zoals `X-Original-URL` te gebruiken om padgebaseerde beperkingen te omzeilen. Het waarborgen van robuuste autorisatie is essentieel voor het behoud van de vertrouwelijkheid van gegevens en het voorkomen van ongeautoriseerde administratieve acties die het gehele systeem in gevaar zouden kunnen brengen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Is horizontal privilege escalation voorkomen tussen gebruikers met dezelfde rol?
- [ ] Ja — autorisatiecontroles **worden toegepast** op elk resourceverzoek en omzeiling is **niet mogelijk**  
- [ ] Ja — autorisatiecontroles **worden toegepast**, maar kunnen worden omzeild via IDOR of parameter tampering  
- [ ] Nee — gebruikers **hebben toegang** tot gegevens van andere gebruikers door simpelweg een resource ID te wijzigen *(Hoog)*  

### Is vertical privilege escalation voorkomen voor gebruikers met lage privileges?
- [ ] Nee — de applicatie heeft slechts één privilegeniveau  
- [ ] Ja — administratieve functies zijn **niet toegankelijk** voor gebruikers met lage privileges  
- [ ] Ja — administratieve functies zijn verborgen in de UI, maar **kunnen** worden geraadpleegd via een directe URL  
- [ ] Nee — gebruikers met lage privileges **kunnen** administratieve acties uitvoeren door rollen of permissies te manipuleren *(Kritiek)*  

### Kan autorisatie worden omzeild met behulp van HTTP verb tampering?
- [ ] Nee — toegangscontroles worden afgedwongen ongeacht de gebruikte HTTP-methode  
- [ ] Ja — het wijzigen van de methode (bijv. van `GET` naar `POST` of `PUT`) **omzeilt** de autorisatiecontroles  

### Zijn administratieve of beperkte endpoints toegankelijk zonder enige authenticatie?
- [ ] Nee — alle gevoelige endpoints vereisen een geldige sessie en de juiste permissies  
- [ ] Ja — sommige administratieve endpoints zijn toegankelijk als de aanvaller de directe URL kent  
- [ ] Ja — ongeauthenticeerde toegang tot gevoelige functies **is mogelijk** *(Kritiek)*  

### Maken HTTP-headers het mogelijk om padgebaseerde autorisatie te omzeilen?
- [ ] Nee — de applicatie is **niet** vatbaar voor op headers gebaseerde omzeilingen  
- [ ] Ja — headers zoals `X-Original-URL`, `X-Rewrite-URL`, of `X-Forwarded-For` **kunnen** worden gebruikt om toegangscontroles te omzeilen

---

## WSTG-ATHZ-03 — Testen op Privilege Escalation

Privilege escalation vindt plaats wanneer een aanvaller kwetsbaarheden misbruikt om toegang te krijgen tot resources of functionaliteit die gereserveerd zijn voor gebruikers met hogere autorisatieniveaus of andere identiteiten. Bij verticale escalatie probeert een standaardgebruiker toegang te krijgen tot administratieve functies, terwijl horizontale escalatie het benaderen van gegevens van een andere gebruiker met hetzelfde privilegieniveau betreft. Deze fouten manifesteren zich doorgaans in slecht geïmplementeerde access control lists (ACLs), insecure direct object references (IDOR), of parameter tampering binnen sessie- of identiteitstokens. Vanuit het perspectief van een aanvaller wordt dit vaak bereikt door het manipuleren van numerieke ID's, het wijzigen van rol-gebaseerde parameters in cookies of JWTs, of door direct verborgen API-endpoints aan te roepen die server-side autorisatiecontroles missen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Implementeert de applicatie meerdere privilegieniveaus of multi-tenancy?
- [ ] Nee — de applicatie is een systeem voor één gebruiker of één rol  
- [ ] Ja — er zijn meerdere rollen of tenants gedefinieerd die autorisatietests vereisen  

### Is horizontale privilege escalation (toegang op hetzelfde niveau) mogelijk?
- [ ] Nee — autorisatiecontroles worden toegepast op alle resource-identifiers en sessie-eigenaren  
- [ ] Ja — toegang tot gegevens van andere gebruikers is mogelijk via IDOR of parameter tampering  
- [ ] Ja — datalekken zijn mogelijk, maar het wijzigen van gegevens van andere gebruikers is niet mogelijk  

### Is verticale privilege escalation (laag naar hoog) mogelijk?
- [ ] Nee — administratieve endpoints dwingen strikt role-based access controls (RBAC) af  
- [ ] Ja — administratieve functies zijn toegankelijk voor gebruikers met weinig privileges via directe URL-toegang  
- [ ] Ja — administratieve functies zijn toegankelijk door het manipuleren van rol-gerelateerde headers, cookies of JWT-claims  

### Kunnen gebruikersrollen of machtigingen worden gewijzigd via Mass Assignment of Parameter Pollution?
- [ ] Nee — rol-gebaseerde velden zijn strikt alleen-lezen en kunnen niet door gebruikers worden gewijzigd  
- [ ] Ja — gebruikersrechten kunnen worden verhoogd door verborgen parameters (bijv. `role=admin`) op te nemen in profielupdates of registratie  

### Worden autorisatiecontroles consistent afgedwongen in alle API-versies en HTTP-methoden?
- [ ] Ja — controles worden consistent toegepast op alle versies en methoden  
- [ ] Nee — legacy API-versies (bijv. `/v1/`) of specifieke HTTP-methoden (bijv. `PUT`, `DELETE`, `PATCH`) omzeilen autorisatie  
- [ ] Nee — administratieve functies zijn verborgen in de UI, maar ingeschakeld op API-niveau voor alle gebruikers

---

## WSTG-ATHZ-04 — Testing for Insecure Direct Object References

Insecure Direct Object References (IDOR) treden op wanneer een applicatie directe toegang biedt tot objecten op basis van door de gebruiker verstrekte invoer, zonder een autorisatiecontrole uit te voeren om de machtigingen van de aanvrager te verifiëren. Deze kwetsbaarheid heeft een aanzienlijke impact op de vertrouwelijkheid en integriteit, aangezien het aanvallers in staat stelt om gegevens van andere gebruikers of het systeem in te zien, te wijzigen of te verwijderen. Het manifesteert zich doorgaans in URL-parameters, POST body data of JSON keys die verwijzen naar interne database keys, bestandsnamen of account-identifiers. Vanuit het perspectief van een aanvaller houdt exploitatie het manipuleren van deze identifiers in — vaak door middel van incrementele integers of het fuzzen van UUIDs — om toegang te krijgen tot bronnen buiten hun geautoriseerde scope.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### Gebruikt de applicatie directe object-identifiers in request-parameters?
- [ ] Nee — applicatie gebruikt indirecte referenties of versleutelde maps *(Meest veilig)*  
- [ ] Ja — applicatie gebruikt niet-voorspelbare identifiers (bijv. lange UUIDs/hashes)  
- [ ] Ja — applicatie gebruikt voorspelbare identifiers (bijv. incrementele integers of gebruikersnamen)  

### Wordt server-side autorisatie uitgevoerd voor elk verzoek waarbij een object-identifier betrokken is?
- [ ] Ja — server-side controles kunnen voor geen enkele identifier worden omzeild  
- [ ] Ja — server-side controles zijn aanwezig, maar omzeiling is mogelijk via parameter pollution of header-manipulatie  
- [ ] Nee — er wordt geen autorisatiecontrole toegepast om het eigendom van het opgevraagde object te verifiëren  

### Kan een aanvaller objecten van andere gebruikers benaderen of wijzigen (Horizontale IDOR)?
- [ ] Nee — toegang tot gegevens van andere gebruikers is niet mogelijk  
- [ ] Ja — het inzien van gegevens van andere gebruikers is mogelijk, maar wijziging is niet mogelijk  
- [ ] Ja — zowel het inzien als het wijzigen van gegevens van andere gebruikers is mogelijk *(Kritiek)*  

### Is het mogelijk om objecten op beheerders- of systeemniveau te benaderen (Verticale IDOR)?
- [ ] Nee — toegang tot objecten met hogere privileges is niet mogelijk  
- [ ] Ja — toegang tot beheerdersconfiguraties of systeembestanden is mogelijk  

### Lekt de applicatie object-identifiers via zoekfuncties, metadata of andere endpoints?
- [ ] Nee — identifiers worden niet onbedoeld blootgesteld  
- [ ] Ja — identifiers voor andere gebruikers/objecten kunnen worden geënumereerd (enumeration) via secundaire endpoints of publieke profielen

---

## WSTG-ATHZ-05 — Testen op OAuth-kwetsbaarheden

OAuth-kwetsbaarheden omvatten diverse tekortkomingen in de implementatie van de OAuth 2.0- of OpenID Connect (OIDC)-protocollen, wat vaak resulteert in een volledige account takeover of ongeoorloofde toegang tot gegevens. Deze kwetsbaarheden manifesteren zich doorgaans tijdens de autorisatie-flow, specifiek met betrekking tot de validatie van de `redirect_uri`, de entropie en verificatie van de `state`-parameter, en de veilige afhandeling van access of ID tokens. Aanvallers maken hier misbruik van door autorisatiecodes te onderscheppen, tokens om te leiden naar door de aanvaller beheerde domeinen via open redirects, of door Cross-Site Request Forgery (CSRF) uit te voeren om de sessie van een slachtoffer te koppelen aan het account van een aanvaller. Omdat OAuth vaak dient als primair authenticatiemechanisme, kan een enkele configuratiefout de integriteit van het gehele gebruikersidentiteitssysteem in gevaar brengen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Tools:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Is de `state`-parameter geïmplementeerd en gevalideerd om CSRF te voorkomen?
- [ ] Ja — `state` is verplicht, uniek per sessie en wordt strikt gevalideerd op de server  
- [ ] Ja — `state` is aanwezig maar is statisch of voorspelbaar over verschillende sessies  
- [ ] Ja — `state` is aanwezig maar de applicatie valideert deze **niet** bij de callback  
- [ ] Nee — de `state`-parameter wordt **niet** gebruikt in het autorisatieverzoek *(Kritiek)*  

### Wordt de `redirect_uri` strikt gevalideerd tegen een whitelist?
- [ ] Ja — alleen exacte overeenkomsten met een vooraf geregistreerde whitelist worden **geaccepteerd**  
- [ ] Ja — validatie wordt toegepast maar omzeiling **is mogelijk** via path traversal of subdomeinmanipulatie  
- [ ] Ja — validatie wordt toegepast maar omzeiling **is mogelijk** via parameter pollution of fragment-injectie  
- [ ] Nee — de applicatie **staat willekeurige URL's toe** in de `redirect_uri`-parameter *(Kritiek)*  

### Kan de `response_type` worden gemanipuleerd om de flow te wijzigen?
- [ ] Nee — de applicatie accepteert alleen de beoogde flow (bijv. `code`) en weigert andere  
- [ ] Ja — overschakelen van `code` naar `token` (Implicit flow) **is mogelijk** en lekt tokens in de URL  
- [ ] Ja — hybrid flows of ongeautoriseerde `response_type`-waarden zijn **ingeschakeld** en worden verwerkt  

### Lekken access tokens of autorisatiecodes via Referer-headers of browsergeschiedenis?
- [ ] Nee — tokens/codes worden veilig afgehandeld en gevoelige pagina's gebruiken een passend `Referrer-Policy`  
- [ ] Ja — tokens/codes **lekken** naar domeinen van derden via de `Referer`-header  
- [ ] Ja — tokens/codes **zijn zichtbaar** in de browsergeschiedenis als gevolg van onveilige caching of GET-verzoeken  

### Handhaaft de applicatie het "Least Privilege"-principe voor OAuth-scopes?
- [ ] Ja — de applicatie vraagt alleen de minimale scopes aan die nodig zijn voor de functionaliteit  
- [ ] Nee — de applicatie vraagt standaard overmatige of administratieve scopes aan  
- [ ] Nee — de gebruiker **kan geen** specifieke scopes beoordelen of weigeren tijdens de toestemmingsfase

---

## WSTG-SESS-01 — Testen van het Sessiebeheer-schema

Het testen van het sessiebeheer-schema richt zich op het analyseren van hoe de applicatie de levenscyclus en structuur van sessie-identificatoren afhandelt om ervoor te zorgen dat deze robuust zijn tegen voorspelling en overname (hijacking). Aanvallers onderscheppen meerdere sessietokens om statistische analyses uit te voeren, waarbij ze zoeken naar patronen, lage entropie (entropy) of voorspelbare toenames die hen in staat stellen om geldige sessies voor andere gebruikers te vervalsen. Zwakheden in het schema uiten zich vaak als sessiefixatie (Session Fixation) kwetsbaarheden of het gebruik van onvoldoende willekeurige waarden, die doorgaans worden aangetroffen in HTTP-cookies, URL-parameters of aangepaste header-implementaties. Het compromitteren van het sessieschema is een aanval met een hoge impact die een aanvaller volledige toegang geeft tot de geauthenticeerde status van een slachtoffer zonder dat de primaire inloggegevens nodig zijn.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Is de sessie-identificator voldoende complex en willekeurig?
- [ ] Ja — token slaagt voor statistische willekeurigheidstests (hoge entropie) en een bypass **is niet mogelijk**  
- [ ] Ja — token is lang maar bevat voorspelbare patronen of statische segmenten  
- [ ] Nee — token is sequentieel, gebaseerd op voorspelbare timestamps, of heeft een **kritiek lage** entropie *(Kritiek)*  

### Waar wordt de sessie-identificator verzonden en opgeslagen?
- [ ] Identificator wordt opgeslagen in een `Secure` en `HttpOnly` cookie *(Meest veilig)*  
- [ ] Identificator wordt opgeslagen in een cookie maar mist de `Secure` of `HttpOnly` flags  
- [ ] Identificator wordt **alleen** verzonden via URL-parameters of `GET`-verzoeken *(Hoog risico)*  

### Genereert de applicatie een nieuwe sessie-identificator bij een succesvolle authenticatie?
- [ ] Ja — een nieuw, uniek sessie-ID **wordt gegenereerd** onmiddellijk na het inloggen  
- [ ] Nee — het sessie-ID blijft hetzelfde voor en na het inloggen *(Sessiefixatie/Session Fixation mogelijk)*  

### Is de sessie-identificator gekoppeld aan een specifiek IP-adres of User-Agent om hergebruik te voorkomen?
- [ ] Ja — sessie wordt ongeldig gemaakt als de vingerafdruk (fingerprint) van de client aanzienlijk verandert  
- [ ] Nee — sessie **kan** worden hergebruikt over verschillende IP-adressen of apparaten zonder aanvullende verificatie  

### Worden sessie-identificatoren aan de serverzijde correct ongeldig gemaakt na uitloggen of time-out?
- [ ] Ja — sessiestatus aan de serverzijde wordt **volledig vernietigd** bij het uitloggen  
- [ ] Nee — sessie blijft geldig op de server, zelfs als de cookie aan de clientzijde wordt verwijderd  
- [ ] Nee — sessie **verloopt nooit** of heeft een buitensporig lange time-outperiode

---

## WSTG-SESS-02 — Testing for Cookies Attributes

Sessiebeheer is vaak afhankelijk van HTTP-cookies om de status (state) te behouden, waardoor de beveiligingsattributen van deze cookies een primair doelwit zijn voor aanvallers. Door attributen zoals `HttpOnly`, `Secure` en `SameSite` weg te laten of onjuist te configureren, stellen applicaties sessietokens bloot aan diefstal via Cross-Site Scripting (XSS), interceptie via onversleutelde kanalen of Cross-Site Request Forgery (CSRF) aanvallen. Pentesters onderzoeken deze flags om te bepalen of een aanvaller sessie-identificatoren kan exfiltreren uit het document object model (DOM) van de browser of de browser kan dwingen deze te verzenden in cross-origin verzoeken. Een juiste configuratie van attributen is een fundamentele defense-in-depth maatregel om te waarborgen dat gevoelige tokens beperkt blijven tot geautoriseerde, veilige contexten.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analyse van de implementatie van cookie-flags
| Attribuut | Aanwezig | Ontbrekend |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Is de `HttpOnly`-flag toegepast op gevoelige sessie-cookies?
- [ ] Ja — toegepast op **alle** gevoelige sessie-cookies *(Meest veilig)*  
- [ ] Ja — alleen toegepast op sommige sessie-cookies  
- [ ] Nee — flag is **niet toegepast**, wat toegang via client-side scripts mogelijk maakt *(Kritiek)*  

### Wordt de `Secure`-flag afgedwongen voor cookies die via HTTPS worden verzonden?
- [ ] Ja — toegepast op **alle** cookies die via TLS worden verzonden  
- [ ] Nee — flag is **niet toegepast**, waardoor cookies via onversleutelde HTTP kunnen worden verzonden  

### Wordt het `SameSite`-attribuut gebruikt om CSRF-risico's te beperken?
- [ ] Ja — ingesteld op `Strict` of `Lax` for sessie-cookies  
- [ ] Ja — ingesteld op `None` maar met de `Secure`-flag aanwezig  
- [ ] Nee — attribuut **ontbreekt** of is ingesteld op `None` **zonder** `Secure`  

### Zijn de `Domain`- en `Path`-attributen beperkt tot de minimaal noodzakelijke scope?
- [ ] Ja — beperkt tot het **specifieke** subdomein en applicatiepad  
- [ ] Nee — scope is te **breed** (bijv. ingesteld op een hoofddomein of root `/`), wat het aanvalsoppervlak vergroot  

### Verlopen gevoelige sessie-cookies correct via de `Expires`- of `Max-Age`-attributen?
- [ ] Ja — passende verloopdata zijn ingesteld  
- [ ] Nee — cookies zijn persistent met een buitengewoon **lange** levensduur  
- [ ] Nee — cookies zijn alleen voor de sessie maar **missen** afdwinging van een server-side timeout

---

## WSTG-SESS-03 — Session Fixation

Session fixation treedt op wanneer een applicatie nalaat de sessie-identificator te invalideren of te roteren nadat een gebruiker succesvol is geauthenticeerd, waardoor een aanvaller een bekend sessietoken kan afdwingen bij een slachtoffer. Als de applicatie de pre-authenticatie sessie-ID behoudt na de login, kan een aanvaller een slachtoffer voorzien van een specifiek samengestelde link die een vast sessie-ID bevat en vervolgens de geauthenticeerde sessie overnemen (hijacken). Deze kwetsbaarheid manifesteert zich doorgaans in inlogformulieren of via sessie-identificatoren die worden geaccepteerd via URL-parameters en cookies. Vanuit het perspectief van de aanvaller omvat exploitatie het "fixeren" van de sessie via een kwaadaardige link of een header injection kwetsbaarheid en wachten tot het slachtoffer inloggegevens verstrekt, waardoor de noodzaak om een live token te stelen effectief wordt omzeild.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Verandert de sessie-identificator na een succesvolle authenticatie?
- [ ] Ja — er wordt een nieuwe sessie-identificator **uitgegeven** en de oude wordt geïnvalideerd *(Meest veilig)*  
- [ ] Ja — er wordt een nieuwe sessie-identificator **uitgegeven** maar de oude **blijft geldig**  
- [ ] Nee — de sessie-identificator **blijft hetzelfde** voor en na authenticatie *(Kritiek)*  

### Accepteert de applicatie sessie-identificatoren die via URL-parameters worden aangeboden?
- [ ] Nee — sessie-ID's worden **uitsluitend** via cookies beheerd  
- [ ] Ja — sessie-ID's worden geaccepteerd in de URL maar worden **genegeerd** of **geroteerd** bij het inloggen  
- [ ] Ja — sessie-ID's in de URL worden **geaccepteerd** en **behouden** na authenticatie  

### Kan een door de aanvaller gedefinieerde sessie-ID worden afgedwongen bij de applicatie?
- [ ] Nee — de applicatie **weigert** ongeldige of niet-bestaande sessie-ID's die door de gebruiker worden verstrekt  
- [ ] Ja — de applicatie **accepteert** en initialiseert een sessie met een willekeurige ID die in een cookie wordt verstrekt  
- [ ] Ja — de applicatie **accepteert** en initialiseert een sessie met een willekeurige ID die in een URL-parameter wordt verstrekt  

### Worden pre-authenticatie sessies correct beëindigd?
- [ ] Ja — de anonieme sessie wordt **volledig vernietigd** bij de login-gebeurtenis  
- [ ] Nee — de anonieme sessie wordt **niet vernietigd**, wat mogelijke sessie-harvesting toestaat  

### Is de sessie-cookie beschermd tegen client-side injection?
- [ ] Ja — `HttpOnly` and `Secure` flags zijn **toegepast** om op scripts gebaseerde fixation te voorkomen  
- [ ] Nee — flags zijn **niet toegepast**, waardoor sessie-ID's kunnen worden ingesteld via Cross-Site Scripting (XSS)

---

## WSTG-SESS-04 — Testen op Blootgestelde Sessievariabelen

Blootgestelde sessievariabelen treden op wanneer gevoelige sessie-identifiers of statusgerelateerde gegevens worden verzonden via onveilige kanalen, zoals URL-querystrings, Referer-headers of systeemlogs. Deze blootstelling verhoogt het risico op Session Hijacking aanzienlijk, aangezien identifiers kunnen worden opgeslagen in de browsergeschiedenis, gelogd door tussenliggende proxy's of gelekt naar websites van derden via de Referer-header. Pentesters identificeren deze blootstellingen door alle uitgaande verzoeken te monitoren en applicatielogs of serverconfiguraties te inspecteren op onbedoelde datalekken. In de praktijk maken aanvallers misbruik van deze lekken door geldige sessietokens te verzamelen van gedeelde systemen of door verkeerspatronen te analyseren om authenticatie te omzeilen en ongeautoriseerde toegang tot gebruikersaccounts te verkrijgen.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Teststatus** | Niet Uitgevoerd |
| **Severity** | Gemiddeld / Hoog* |

> *Severity wordt Hoog indien de blootgestelde variabele een sessietoken is dat directe account takeover mogelijk maakt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### Wordt de sessie-identifier verzonden in de URL-querystring?
- [ ] Nee — sessie-identifiers zijn **niet** aanwezig in de URL-querystring  
- [ ] Ja — identifiers zijn aanwezig, maar de sessie is kortstondig of van laag risico  
- [ ] Ja — unieke sessie-ID's **zijn** aanwezig in de URL-querystring *(Kritiek)*  

### Lekt de applicatie sessietokens via de Referer-header?
- [ ] Nee — `Referrer-Policy` voorkomt lekkage of er bestaan geen links naar derden  
- [ ] Ja — sessietokens **worden** verzonden naar domeinen van derden via de Referer-header  

### Worden sessievariabelen opgeslagen in server-side of applicatielogs?
- [ ] Nee — sessievariabelen zijn gemaskeerd of uitgesloten van logs  
- [ ] Ja — sessievariabelen **worden** in plain text opgenomen in applicatie- of webserverlogs  

### Gebruikt de applicatie GET-verzoeken voor statuswijzigende operaties?
- [ ] Nee — de applicatie gebruikt `POST` of andere niet-idempotente methoden voor gevoelige operaties  
- [ ] Ja — `GET` wordt gebruikt, waardoor sessiegegevens worden gecached in de browsergeschiedenis en tussenliggende proxy's  

### Is de `Cache-Control` header geconfigureerd om caching van sessiegerelateerde gegevens te voorkomen?
- [ ] Ja — `Cache-Control: no-store` (of vergelijkbaar) **is toegepast** op gevoelige antwoorden  
- [ ] Ja — controles zijn aanwezig, maar een bypass **is mogelijk** via browser edge cases  
- [ ] Nee — gevoelige antwoorden die sessiegegevens bevatten **kunnen** door de browser worden gecached

---

## WSTG-SESS-05 — Testen op Cross-Site Request Forgery

Cross-Site Request Forgery (CSRF) is een kwetsbaarheid waarbij een aanvaller de browser van een slachtoffer misleidt om een ongewenste actie uit te voeren op een andere website waar het slachtoffer momenteel is geauthenticeerd. Deze exploit maakt gebruik van het gedrag van de browser om automatisch "ambient" inloggegevens, zoals sessiecookies of Authorization-headers, toe te voegen aan uitgaande verzoeken. Aanvallers richten zich doorgaans op statusveranderende operaties zoals het wijzigen van wachtwoorden, het bijwerken van e-mailadressen of het uitvoeren van financiële overschrijvingen door kwaadaardige scripts of verborgen formulieren op een externe site te hosten. Succesvolle exploitatie kan leiden tot volledige accountovername of ongeautoriseerde gegevenswijziging zonder medeweten of toestemming van de gebruiker.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt "Hoog" als de kwetsbare actie accountovername, privilege-escalatie of ongeautoriseerde financiële transacties mogelijk maakt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Tools:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Zijn anti-CSRF tokens geïmplementeerd voor statusveranderende verzoeken?
- [ ] Ja — unieke, cryptografisch sterke tokens zijn vereist voor alle statusveranderende acties  
- [ ] Ja — tokens zijn aanwezig, maar zijn **niet** uniek per sessie of zijn voorspelbaar  
- [ ] Nee — anti-CSRF tokens zijn **niet** geïmplementeerd  

### Is de server-side validatie van het anti-CSRF token robuust?
- [ ] Ja — de server valideert het token strikt en bypass is **niet mogelijk**  
- [ ] Ja — validatie wordt uitgevoerd, maar **kan** worden omzeild door de tokenparameter te verwijderen  
- [ ] Ja — validatie wordt uitgevoerd, maar **kan** worden omzeild door een leeg of dummy token op te geven  
- [ ] Ja — validatie wordt uitgevoerd, maar het token is **niet** gekoppeld aan de sessie van de gebruiker  

### Vertrouwt de applicatie op eenvoudig te omzeilen methoden voor CSRF-bescherming?
- [ ] Nee — de applicatie vertrouwt niet alleen op zwakke headers of origin-controles  
- [ ] Ja — de bescherming vertrouwt uitsluitend op de `Referer`- of `Origin`-header, die **gespooft** of verwijderd kan worden  
- [ ] Ja — de bescherming vertrouwt op het controleren van de `X-Requested-With`-header, wat **omzeild kan worden** via CORS-misconfiguraties  

### Zijn sessiecookies geconfigureerd met het `SameSite`-attribuut?
- [ ] Ja — `SameSite` is ingesteld op `Strict` of `Lax` voor alle sessiegerelateerde cookies  
- [ ] Nee — het `SameSite`-attribuut **ontbreekt**, waardoor wordt teruggevallen op browserspecifiek gedrag  
- [ ] Nee — `SameSite` is expliciet ingesteld op `None` zonder de `Secure`-vlag of aanvullende mitigerende maatregelen  

### Kan de CSRF-bescherming worden omzeild door de HTTP-verzoekmethode te wijzigen?
- [ ] Nee — bescherming wordt afgedwongen ongeacht de gebruikte HTTP-methode  
- [ ] Ja — tokens worden alleen gevalideerd bij `POST`-verzoeken, maar de actie **kan** worden uitgevoerd via `GET`  
- [ ] Ja — het wijzigen van de methode (bijv. naar `PUT` of `DELETE`) omzeilt de tokencontrole

---

## WSTG-SESS-06 — Testen op Uitlogfunctionaliteit

De uitlogfunctionaliteit is een kritieke beveiligingsmaatregel die ontworpen is om de sessie van een gebruiker te beëindigen en de bijbehorende sessie-identificatoren zowel aan de client- als aan de serverzijde ongeldig te maken. Het niet correct implementeren van de uitlogfunctionaliteit maakt Session Fixation of Hijacking aanvallen mogelijk, aangezien een aanvaller die toegang krijgt tot een systeem nadat een gebruiker is "uitgelogd", potentieel het nog steeds actieve Session Token kan hergebruiken. Pentesters evalueren dit door te controleren of de sessiecookie uit de browser wordt verwijderd, of de server-side sessiestatus expliciet wordt vernietigd, en of navigatie via de 'Terug'-knop toegang geeft tot gecachte gevoelige gegevens. Een veilige implementatie van uitloggen zorgt ervoor dat zodra de actie is geactiveerd, geen enkel volgend verzoek met het oude Session Token meer door de applicatie wordt geautoriseerd.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Is er een functioneel uitlogmechanisme aanwezig en toegankelijk?
- [ ] Ja — uitlogknop is aanwezig en triggert een verzoek tot beëindiging  
- [ ] Nee — uitlogknop **ontbreekt** of triggert **geen** beëindigingsevent  

### Wordt de sessie-identificator aan de serverzijde ongeldig gemaakt?
- [ ] Ja — de server weigert alle volgende verzoeken die het oude Session Token gebruiken  
- [ ] Nee — de server **blijft** het oude Session Token **accepteren** na het uitloggen  

### Verwijdert de applicatie sessiecookies in de browser bij het uitloggen?
- [ ] Ja — cookies worden overschreven met een verlopen datum of een lege waarde  
- [ ] Ja — cookies blijven aanwezig maar zijn **niet langer geldig** op de server  
- [ ] Nee — cookies blijven in de browser aanwezig en **behouden** hun oorspronkelijke waarden  

### Is gevoelige geauthenticeerde content toegankelijk via de 'Terug'-knop van de browser na het uitloggen?
- [ ] Nee — `Cache-Control: no-store` of vergelijkbare headers voorkomen het bekijken van gevoelige pagina's na het uitloggen  
- [ ] Ja — gevoelige pagina's worden gecacht en **kunnen** worden bekeken via navigatie nadat de sessie is beëindigd

---

## WSTG-SESS-07 — Testen van Sessie-timeout

Sessie-timeout-testen evalueren of een applicatie de sessie van een gebruiker effectief beëindigt na een vooraf gedefinieerde periode van inactiviteit of totale duur. Deze controle is essentieel voor het beperken van het risico op Session Hijacking, met name op gedeelde werkstations of in scenario's waarin een aanvaller een sessie-identifier bemachtigt via netwerkinterceptie of XSS. Pentesters analyseren zowel idle timeouts, die worden geactiveerd na een periode van inactiviteit, als absolute timeouts, die de totale levensduur van een sessie beperken, ongeacht de activiteit. Vanuit het perspectief van een aanvaller bieden onbeperkte of extreem lange sessies een groter venster voor het behouden van ongeautoriseerde toegang en het omzeilen van de noodzaak voor herauthenticatie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Implementeert de applicatie een idle sessie-timeout?
- [ ] Ja — sessie verloopt na een korte periode (bijv. 15-30 minuten) van inactiviteit  
- [ ] Ja — sessie verloopt, maar de timeout-duur is **excessief lang** (bijv. > 24 uur)  
- [ ] Nee — sessie **blijft onbeperkt actief** tijdens inactiviteit  

### Wordt de sessie-timeout aan de serverzijde afgedwongen?
- [ ] Ja — server weigert verzoeken met verlopen tokens, ongeacht de status aan de clientzijde  
- [ ] Nee — timeout wordt **alleen** afgedwongen via client-side JavaScript of meta-refresh  

### Implementeert de applicatie een absolute sessie-timeout?
- [ ] Ja — sessie wordt beëindigd na een vaste maximale duur, ongeacht activiteit  
- [ ] Nee — sessie **kan** onbeperkt worden verlengd zolang er voortdurende activiteit is  

### Wordt de sessie-identifier op de server ongeldig gemaakt bij een timeout?
- [ ] Ja — sessietoken **kan niet** opnieuw worden gebruikt zodra de timeout-drempel is bereikt  
- [ ] Nee — sessietoken **is nog steeds geldig** op de server, zelfs nadat de client is omgeleid naar de inlogpagina  

### Kan de sessie onbeperkt in leven worden gehouden met behulp van geautomatiseerde heartbeat-verzoeken?
- [ ] Nee — absolute timeout of secundaire controles **voorkomen** oneindige sessieverlenging  
- [ ] Ja — periodieke achtergrondverzoeken (bijv. AJAX) **kunnen** de sessie onbeperkt behouden

---

## WSTG-SESS-08 — Session Puzzling

Session Puzzling, ook wel bekend als Session Variable Overloading, vindt plaats wanneer een applicatie dezelfde sessievariabele voor meerdere doeleinden gebruikt in verschillende modules, of nalaat om sessiestatussen correct te wissen tijdens workflow-overgangen. Aanvallers maken misbruik van dit gedrag door sessievariabelen in te vullen in de ene context — zoals een niet-geauthenticeerde profielweergave of een registratieproces met meerdere stappen — en vervolgens naar een beveiligd gedeelte te navigeren waar de applicatie deze reeds bestaande variabelen ten onrechte vertrouwt. Dit kan leiden tot kritieke gevolgen, waaronder authentication bypass, privilege escalation of ongeautoriseerde toegang tot gevoelige gegevens, door de applicatie te misleiden en te laten geloven dat een sessie zich in een hoger geprivilegieerde status bevindt dan in werkelijkheid het geval is. De kwetsbaarheid komt het meest voor in complexe, stateful applicaties waar de sessiebeheerlogica inconsistent wordt toegepast over verschillende functionele componenten.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### Gebruikt de applicatie dezelfde namen voor sessievariabelen voor verschillende doeleinden in afzonderlijke modules?
- [ ] Nee — variabelenamen zijn uniek of scopes zijn geïsoleerd  
- [ ] Ja — er bestaan gedeelde variabelenamen, maar de status wordt gewist tussen modules  
- [ ] Ja — er bestaan gedeelde variabelenamen en de status **wordt niet** gewist  

### Kunnen niet-geauthenticeerde acties sessievariabelen beïnvloeden die in geauthenticeerde contexten worden gebruikt?
- [ ] Nee — sessievariabelen worden pas geïnitialiseerd **na** een succesvolle authenticatie  
- [ ] Ja — sessievariabelen kunnen vóór het inloggen worden ingesteld, maar hebben **geen** invloed op de status na het inloggen  
- [ ] Ja — sessievariabelen die tijdens pre-authenticatie zijn ingesteld, **worden** vertrouwd na de authenticatie *(Kritiek)*  

### Herinitialiseert de applicatie de sessie-identifier en de bijbehorende variabelen bij een wijziging in het privilegeniveau?
- [ ] Ja — de sessie wordt volledig gereset en er **wordt** een nieuw ID uitgegeven  
- [ ] Gedeeltelijk — er wordt een nieuw ID uitgegeven, maar sommige sessievariabelen **blijven behouden**  
- [ ] Nee — het sessie-ID en alle variabelen **blijven** ongewijzigd  

### Kunnen beheerdersrechten worden verkregen door sessievariabelen te manipuleren via een account zonder privileges?
- [ ] Nee — privilege-variabelen **kunnen niet** door gebruikers worden gewijzigd  
- [ ] Ja — variabelen kunnen worden gewijzigd, maar de applicatie **valideert** deze tegen de backend-database  
- [ ] Ja — variabelen kunnen worden gewijzigd en de applicatie **vertrouwt** de sessiestatus impliciet *(Kritiek)*  

### Wordt de sessiestatus onmiddellijk gewist na het voltooien van een specifieke workflow (bijv. wachtwoord resetten, afrekenen)?
- [ ] Ja — sessievariabelen voor de workflow worden bij voltooiing vernietigd  
- [ ] Nee — workflow-variabelen **blijven behouden** en kunnen in andere delen van de applicatie worden hergebruikt

---

## WSTG-SESS-09 — Testen op Session Hijacking

Session hijacking vindt plaats wanneer een aanvaller een geldige sessie-identificatie (SID) onderschept, voorspelt of vastlegt om ongeautoriseerde toegang te krijgen tot de actieve sessie van een gebruiker. Deze kwetsbaarheid komt doorgaans voort uit onvoldoende beveiliging van de transportlaag, het ontbreken van cookie-beveiligingsvlaggen of voorspelbare SID-generatie-algoritmen die een aanvaller in staat stellen om authenticatie volledig te omzeilen. Vanuit het perspectief van een aanvaller biedt succesvolle exploitatie de mogelijkheid om het slachtoffer volledig te imiteren zonder dat de inloggegevens nodig zijn, wat vaak wordt bereikt via network sniffing, Cross-Site Scripting (XSS) of session fixation-aanvallen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Worden sessie-identificaties beschermd tijdens transport?
- [ ] Ja — `Secure` flag is **ingeschakeld** en HSTS wordt strikt afgedwongen  
- [ ] Ja — `Secure` flag is **ingeschakeld** maar HSTS is **uitgeschakeld**  
- [ ] Nee — `Secure` flag is **niet toegepast**, waardoor SID-interceptie via onversleutelde kanalen mogelijk is *(Hoog)*  

### Is de sessie-identificatie beschermd tegen toegang door client-side scripts?
- [ ] Ja — `HttpOnly` flag is **toegepast** op alle sessie-gerelateerde cookies  
- [ ] Nee — `HttpOnly` flag is **niet toegepast**, waardoor SID-exfiltratie via XSS mogelijk is *(Kritiek)*  

### Implementeert de applicatie sessiebinding aan clientkenmerken?
- [ ] Ja — sessie is gebonden aan clientkenmerken (IP/User-Agent) en hergebruik is **niet mogelijk**  
- [ ] Ja — sessiebinding is aanwezig maar omzeiling **is mogelijk** via header spoofing  
- [ ] Nee — geen sessiebinding aanwezig; SID **is geldig** bij gebruik vanaf elke bron  

### Is de sessie-identificatie voldoende willekeurig om voorspelling te voorkomen?
- [ ] Ja — SID maakt gebruik van een cryptografisch veilige pseudo-random getallengenerator (CSPRNG)  
- [ ] Nee — SID-lengte is onvoldoende of volgt een **voorspelbare** reeks/patroon  

### Worden gelijktijdige sessies beheerd om de aanvalsperiode te beperken?
- [ ] Nee — applicatie dwingt strikt één enkele actieve sessie per gebruiker af  
- [ ] Ja — meerdere gelijktijdige sessies zijn **ingeschakeld** maar worden gemonitord  
- [ ] Ja — onbeperkte gelijktijdige sessies **zijn mogelijk** over verschillende apparaten/locaties

---

## WSTG-SESS-10 — Testen van JSON Web Tokens

JSON Web Tokens (JWTs) worden veelvuldig gebruikt voor stateless session management en identity propagation, maar de beveiliging hiervan is volledig afhankelijk van de correcte implementatie van signing algorithms en strikte claim-verificatie. Aanvallers proberen regelmatig authenticatie te omzeilen door misbruik te maken van "none" algorithm kwetsbaarheden, het brute-forcen van zwakke HS256 secret keys, of het uitvoeren van algorithm-switching attacks waarbij een asymmetrische public key wordt gebruikt als een symmetrisch HMAC secret. Deze kwetsbaarheden manifesteren zich doorgaans in session cookies of Authorization headers, wat een aanvaller potentieel in staat stelt om rechten te escaleren (privilege escalation) of een willekeurige gebruiker te imiteren door claims zoals `role` of `user_id` aan te passen zonder de cryptografische integriteit van het token ongeldig te maken.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Tools:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### Wordt het "none" algoritme geaccepteerd door de server?
- [ ] Nee — de server **weigert** tokens die `alg: none` gebruiken in de header  
- [ ] Ja — de server **accepteert** tokens met `alg: none` en een lege signature *(Kritiek)*  

### Hoe wordt de JWT-signature geverifieerd en beschermd tegen brute-force?
- [ ] Ja — er worden sterke asymmetrische keys (RS256/ES256) of high-entropy HMAC secrets gebruikt  
- [ ] Ja — HS256 wordt gebruikt, maar het secret **kan** worden ge-brute-forced vanwege lage entropie of standaardwaarden  
- [ ] Nee — signature-verificatie **wordt niet toegepast** of **kan** worden omzeild via algorithm switching (RS256 naar HS256)  

### Worden standaard claims (exp, nbf, iat) strikt gehandhaafd door de backend?
- [ ] Ja — `exp` (expiration) is aanwezig en **kan niet** worden omzeild  
- [ ] Ja — `exp` is aanwezig, maar de server **handhaaft** deze niet tijdens de validatie  
- [ ] Nee — expiration claims **ontbreken** of worden genegeerd, wat onbeperkte session replay mogelijk maakt  

### Voorkomt de implementatie header-gebaseerde key injection (kid, jku, x5u)?
- [ ] Ja — headers worden gesaneerd en alleen vertrouwde interne keys/bronnen zijn **toegestaan**  
- [ ] Nee — de `kid` (Key ID) header is kwetsbaar voor directory traversal of SQL Injection om naar een bekend bestand te verwijzen  
- [ ] Nee — de `jku` of `x5u` headers **kunnen** worden verwezen naar door de aanvaller beheerde URL's om kwaadaardige keys te laden

---

## WSTG-SESS-11 — Testen op Gelijktijdige Sessies

Testen op gelijktijdige sessies (Concurrent session testing) evalueert of een applicatie een enkel gebruikersaccount toestaat om meerdere gelijktijdige actieve sessies te onderhouden via verschillende browsers, apparaten of IP-adressen. Het niet beperken van gelijktijdige sessies vergroot de kans voor aanvallers om misbruik te maken van gekaapte sessietokens (session tokens) of gecompromitteerde inloggegevens zonder de legitieme gebruiker te waarschuwen of bestaande sessies te beëindigen. Dit gedrag wordt doorgaans beoordeeld door te authenticeren vanuit meerdere bronnen en de sessiestabiliteit te monitoren, wat vaak zwakheden in de sessiebeheerlogica (session management logic) of een gebrek aan veiligheidsgerichte bedrijfsregels aan het licht brengt. Vanuit het perspectief van een aanvaller maakt het ontbreken van concurrency control heimelijke persistentie mogelijk na een initiële inbreuk en vergemakkelijkt het parallelle geautomatiseerde aanvallen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Worden gelijktijdige sessies beperkt door het applicatiebeleid?
- [ ] Ja — slechts één sessie **is toegestaan** per gebruiker; nieuwe logins maken oude sessies ongeldig *(Meest veilig)*  
- [ ] Ja — een vast aantal gelijktijdige sessies **wordt afgedwongen** en kan niet worden overschreden  
- [ ] Nee — een onbeperkt aantal gelijktijdige sessies **is mogelijk**  

### Hoe reageert de applicatie wanneer de sessielimiet is bereikt?
- [ ] De oudste actieve sessie **wordt automatisch beëindigd** (mitigatie tegen session fixation/hijacking)  
- [ ] De gebruiker **wordt gevraagd** om bestaande sessies te beëindigen voordat de nieuwe sessie wordt geautoriseerd  
- [ ] De nieuwe inlogpoging **wordt geblokkeerd** totdat een handmatige afmelding plaatsvindt vanaf een ander apparaat  
- [ ] Er wordt geen actie ondernomen — meerdere sessies blijven gelijktijdig **ingeschakeld**  

### Biedt de applicatie een interface voor sessiebeheer aan de gebruiker?
- [ ] Ja — gebruikers **kunnen** actieve sessies bekijken (IP, apparaat, tijd) en deze op afstand beëindigen  
- [ ] Ja — gebruikers **kunnen** actieve sessies bekijken maar deze **niet** op afstand beëindigen  
- [ ] Nee — gebruikers **kunnen** geen andere actieve sessies zien of beheren die aan hun account zijn gekoppeld  

### Wordt de gebruiker op de hoogte gesteld wanneer gelijktijdige inlogactiviteit wordt gedetecteerd?
- [ ] Ja — een waarschuwing (e-mail, sms of in-app) **wordt geactiveerd** bij logins vanaf nieuwe apparaten of locaties  
- [ ] Nee — er **wordt geen melding gegeven** wanneer gelijktijdige sessies tot stand worden gebracht

---

## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Reflected Cross Site Scripting (XSS) treedt op wanneer een applicatie onvertrouwde gegevens opneemt in een HTTP-respons zonder voldoende validatie of encoding, waardoor de payload wordt uitgevoerd in de context van de browser van het slachtoffer. Aanvallers leveren kwaadaardige payloads via social engineering, doorgaans via gemanipuleerde URL's of formulieren, om gebruikerssessies over te nemen, gevoelige cookies te exfiltreren of ongeautoriseerde acties uit te voeren namens de gebruiker. Deze kwetsbaarheid wordt vaak aangetroffen in zoekparameters, foutmeldingen en elk endpoint dat invoer direct teruggeeft aan de gebruikersinterface. Exploitatie is afhankelijk van de interactie van het slachtoffer met een kwaadaardige link, waardoor het een niet-persistente maar zeer effectieve aanvalsvector is voor het targeten van specifieke beheerders of geauthenticeerde gebruikers.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Worden door de gebruiker verstrekte invoergegevens gereflecteerd in de response body?
- [ ] Nee — invoer wordt **nooit** teruggekoppeld aan de gebruiker  
- [ ] Ja — invoer wordt gereflecteerd maar is correct geëncodeerd en een bypass is **niet mogelijk**  
- [ ] Ja — invoer wordt gereflecteerd **zonder** enige encoding of sanitization  

### Is contextbewuste output encoding geïmplementeerd?
- [ ] Ja — de juiste encoding (HTML, Attribute, JavaScript) wordt **toegepast** op basis van de specifieke reflectiecontext  
- [ ] Ja — encoding wordt **toegepast** maar is onvoldoende voor de specifieke context (bijv. HTML-encoding binnen een `<script>` tag)  
- [ ] Nee — encoding wordt **niet toegepast**  

### Kunnen inputvalidatie of Web Application Firewall (WAF) filters worden omzeild?
- [ ] Nee — filters blokkeren effectief alle algemene en geobfusceerde XSS payloads  
- [ ] Ja — filters zijn aanwezig maar een bypass **is mogelijk** door gebruik te maken van character encoding, niet-standaard tags of polyglots  
- [ ] Nee — er zijn geen filters of validatiemechanismen aanwezig  

### Wat is de context van uitvoering van de gereflecteerde invoer?
- [ ] Veilig — invoer wordt gereflecteerd op een niet-uitvoerbare locatie (bijv. binnen standaard `<div>` of `<span>` tags)  
- [ ] Riskant — invoer wordt gereflecteerd binnen HTML-attributen of binnen de `src`/`href` van tags  
- [ ] Kritiek — invoer wordt direct gereflecteerd binnen `<script>` blokken, event handlers of template literals  

### Zijn er moderne browser security headers aanwezig om de impact van XSS te beperken?
- [ ] Ja — `Content-Security-Policy` (CSP) is **ingeschakeld** met een beperkende script-src  
- [ ] Ja — `Content-Security-Policy` (CSP) is **ingeschakeld** maar bevat `unsafe-inline` of zwakke whitelists  
- [ ] Nee — er zijn geen CSP of legacy `X-XSS-Protection` headers aanwezig

---

## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Stored Cross Site Scripting (XSS), ook bekend als Persistent XSS, vindt plaats wanneer een applicatie gegevens van een gebruiker ontvangt en deze opslaat in een persistente database, bestandssysteem of ander opslagmedium zonder adequate validatie of encoding. Wanneer een slachtoffer vervolgens naar een pagina navigeert die deze niet-gesaneerde gegevens ophaalt en weergeeft, wordt het kwaadaardige script uitgevoerd binnen hun browsercontext onder de origin van de applicatie. Deze kwetsbaarheid is bijzonder gevaarlijk omdat het niet vereist dat een slachtoffer op een speciaal samengestelde link klikt; elke gebruiker die de betreffende pagina bekijkt, wordt een doelwit, wat kan leiden tot session hijacking, ongeautoriseerde acties of credential harvesting. Aanvallers richten zich doorgaans op commentaarsecties, gebruikersprofielen, message boards en administratieve logs waar invoer consistent wordt gerenderd voor andere gebruikers of beheerders.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Tools:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Zijn er invoerpunten die gebruikersinvoer opslaan voor latere weergave aan andere gebruikers?
- [ ] Nee — de applicatie slaat geen door de gebruiker controleerbare invoer op voor latere weergave  
- [ ] Ja — invoer wordt opgeslagen (bijv. profielen, reacties) maar wordt **niet** gerenderd in een HTML-context  
- [ ] Ja — invoer wordt opgeslagen en gerenderd voor andere gebruikers of beheerders  

### Wordt output encoding of sanitization toegepast wanneer opgeslagen gegevens worden gerenderd?
- [ ] Ja — context-aware output encoding **is toegepast** en bypass is **niet mogelijk** *(Meest veilig)*  
- [ ] Ja — sanitization (bijv. DOMPurify) **is toegepast** maar bypass **is mogelijk** via configuratiefouten  
- [ ] Nee — gegevens worden direct in de DOM gerenderd **zonder** encoding of sanitization *(Hoog)*  

### Kan de inputvalidatie of sanitization worden omzeild met alternatieve payloads?
- [ ] Nee — filters verwerken verschillende encodings, niet-standaard tags en event handlers op veilige wijze  
- [ ] Ja — bypass **is mogelijk** met behulp van HTML-entity encoding of specifieke tags (bijv. `<svg>`, `<img>`)  
- [ ] Ja — bypass **is mogelijk** via polyglot payloads of mutation-based XSS (mXSS)  

### Biedt een Content Security Policy (CSP) een secundaire verdedigingslaag?
- [ ] Ja — een strikt CSP is **ingeschakeld** en voorkomt de uitvoering van inline scripts en ongeautoriseerde bronnen  
- [ ] Ja — een CSP is **ingeschakeld** maar is verkeerd geconfigureerd (bijv. `unsafe-inline` of een brede `script-src`)  
- [ ] Nee — er is geen CSP **ingeschakeld** voor de applicatie  

### Is het mogelijk om "Stored XSS naar RCE" of andere high-impact ketens (Blind XSS) te bereiken?
- [ ] Nee — impact is beperkt tot de browsercontext aan de clientzijde  
- [ ] Ja — stored XSS **kan** worden gebruikt om beheerders te targeten (Blind XSS) in interne panelen  
- [ ] Ja — stored XSS **kan** worden gecombineerd met andere kwetsbaarheden (bijv. CSRF, exfiltratie van gevoelige gegevens)

---

## WSTG-INPV-03 — Testing for HTTP Verb Tampering

HTTP Verb Tampering maakt misbruik van zwakheden in de manier waarop webservers en applicatieframeworks de toegang tot specifieke resources autoriseren op basis van de gebruikte HTTP-methode. Aanvallers proberen beveiligingsrestricties te omzeilen door standaardmethoden zoals `GET` of `POST` te vervangen door alternatieven zoals `HEAD`, `PUT`, `OPTIONS`, of zelfs willekeurige, niet-standaard strings die de backend mogelijk inconsistent verwerkt. Deze kwetsbaarheid treedt doorgaans op wanneer beveiligingsconfiguraties—zoals Java EE web.xml filters of .NET authorization regels—expliciet toegestane methoden vermelden maar geen rekening houden met andere, of wanneer een "deny-by-method" benadering wordt gebruikt in plaats van "deny-all." Succesvolle exploitatie kan resulteren in ongeautoriseerde toegang tot administratieve functies, gegevenswijziging of informatieonthulling over de interne configuratie van de server.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Medium / High* |

> *Severity wordt High als administratieve functies of gegevenswijziging (PUT/DELETE) kunnen worden uitgevoerd zonder authenticatie.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### Reageert de applicatie op niet-standaard of willekeurige HTTP-methoden?
- [ ] Nee — de applicatie weigert alle methoden behalve degene die expliciet vereist zijn  
- [ ] Ja — de applicatie reageert op standaardmethoden (OPTIONS, TRACE) maar kan de beveiliging **niet** omzeilen  
- [ ] Ja — de applicatie accepteert willekeurige strings (bijv. FOO, TEST) en verwerkt deze als `GET` of `POST` verzoeken  

### Kan authenticatie/autorisatie worden omzeild door de HTTP-methode te wijzigen?
- [ ] Nee — beveiligingscontroles worden globaal toegepast, ongeacht het HTTP-verb  
- [ ] Ja — controles zijn aanwezig, maar omzeiling **is mogelijk** met de `HEAD`-methode  
- [ ] Ja — beveiligingscontroles worden **niet toegepast** op alternatieve methoden, wat ongeautoriseerde toegang tot beveiligde endpoints mogelijk maakt  

### Zijn gevaarlijke methoden zoals `PUT` of `DELETE` ingeschakeld op de server?
- [ ] Nee — gevaarlijke methoden zijn **uitgeschakeld** of retourneren een 405 Method Not Allowed  
- [ ] Ja — methoden zijn ingeschakeld maar vereisen geldige authenticatie met hoge privileges  
- [ ] Ja — methoden zijn **ingeschakeld** en maken ongeautoriseerde bestandsuploads of resource-verwijdering mogelijk *(Kritiek)*  

### Onthult de `OPTIONS`-methode gevoelige informatie over toegestane verbs?
- [ ] Nee — `OPTIONS` is **uitgeschakeld** of retourneert een generieke respons  
- [ ] Ja — `OPTIONS` retourneert de `Allow`-header maar geeft geen beperkte interne methoden prijs  
- [ ] Ja — `OPTIONS` onthult interne of administratieve methoden die helpen bij verdere exploitatie  

### Is de `TRACE`-methode ingeschakeld, wat mogelijk Cross-Site Tracing (XST) toestaat?
- [ ] Nee — `TRACE`- en `TRACK`-methoden zijn **uitgeschakeld**  
- [ ] Ja — `TRACE` is **ingeschakeld** en reflecteert HTTP-headers, inclusief gevoelige cookies/tokens

---

## WSTG-INPV-04 — Testen op HTTP Parameter Pollution

HTTP Parameter Pollution (HPP) treedt op wanneer een applicatie meerdere HTTP-parameters met dezelfde naam ontvangt en deze op een inconsistente of onveilige manier verwerkt. Door dubbele parameters op te geven, kan een aanvaller de interne logica van de applicatie manipuleren, waardoor Web Application Firewalls (WAF), inputvalidatiefilters of mechanismen voor toegangscontrole (Access Control) mogelijk worden omzeild. Deze kwetsbaarheid is sterk afhankelijk van de onderliggende webserver of het applicatie-framework, dat kan kiezen voor de eerste, de laatste of een samengevoegde (geconcateneerde) versie van de herhaalde parameters. Uitbuiting richt zich doorgaans op endpoints die parameters doorgeven aan back-end API's, databasequery's of URL-redirection-mechanismen, wat kan leiden tot impacts variërend van Cross-Site Scripting (XSS) tot ongeoorloofde data-exfiltratie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Hoe gaat de applicatie om met meerdere HTTP-parameters met dezelfde naam?
- [ ] Nee — dubbele parameters worden **geweigerd** of genegeerd door de server  
- [ ] Ja — de server selecteert consequent alleen de **eerste** of **laatste** instantie  
- [ ] Ja — de server **voegt waarden samen** (concatenatie), wat mogelijk injectie toestaat  

### Kan HPP worden ingezet om beveiligingsmaatregelen zoals een WAF of inputfilter te omzeilen?
- [ ] Nee — beveiligingsmaatregelen inspecteren correct **alle** instanties van een parameter  
- [ ] Ja — een WAF of filter inspecteert alleen de **eerste** instantie, waardoor een malafide **tweede** instantie kan passeren  
- [ ] Ja — door samenvoeging kan een aanvaller een **Payload splitsen** over meerdere parameters om detectie te ontwijken  

### Reflecteert de applicatie vervuilde parameters in de respons zonder de juiste sanitization?
- [ ] Nee — parameters worden gesaneerd of gecodeerd voordat ze in de UI worden gereflecteerd  
- [ ] Ja — vervuiling resulteert in gereflecteerde inhoud, maar sanitization **wordt toegepast**  
- [ ] Ja — vervuiling resulteert in gereflecteerde inhoud en sanitization **wordt niet toegepast**, wat leidt tot XSS of logische fouten  

### Zijn parameters die worden doorgegeven aan interne API-calls of back-end-systemen kwetsbaar voor vervuiling?
- [ ] Nee — back-end-requests maken gebruik van veilige, niet-dynamische constructiemethoden  
- [ ] Ja — HPP stelt een aanvaller in staat om parameters in de back-end-requeststructuur te **injecteren** of te **overschrijven**  

### Vertoont de applicatie ander gedrag wanneer parameters worden vervuild in GET- versus POST-requests?
- [ ] Nee — het gedrag is consistent over alle HTTP-methoden  
- [ ] Ja — alleen GET-parameters zijn kwetsbaar voor vervuiling  
- [ ] Ja — alleen POST (body) parameters zijn kwetsbaar voor vervuiling

---

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

## WSTG-INPV-06 — Testen op LDAP Injection

LDAP Injection treedt op wanneer een applicatie door de gebruiker verstrekte gegevens opneemt in LDAP (Lightweight Directory Access Protocol) filters zonder de juiste opschoning (sanitization) of escaping, waardoor een aanvaller de query-logica kan manipuleren. Door speciale tekens zoals sterretjes, haakjes en logische operatoren te injecteren, kunnen aanvallers het zoekfilter aanpassen om authenticatiemechanismen te omzeilen of gevoelige informatie uit de directory te extraheren, waaronder gebruikersnamen, groepslidmaatschappen en organisatorische attributen. Deze kwetsbaarheid komt meestal voor in functies zoals bedrijvengidszoekopdrachten, werknemersportalen of single sign-on (SSO) systemen die communiceren met Active Directory of OpenLDAP. Vanuit het perspectief van een aanvaller houdt een succesvolle exploitatie vaak blinde technieken (blind techniques) in om de directorystructuur of attribuutwaarden bit-voor-bit te enumereren wanneer directe query-output door de applicatie wordt onderdrukt.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### Maakt de applicatie gebruik van LDAP voor authenticatie of zoekopdrachten in de directory?
- [ ] Nee — LDAP wordt **niet gebruikt** in de applicatie-architectuur  
- [ ] Ja — LDAP wordt gebruikt en alle invoer wordt strikt opgeschoond of geparametriseerd  
- [ ] Ja — LDAP wordt gebruikt en gebruikersinvoer wordt geconcateneerd in filters **zonder** de juiste escaping  

### Worden LDAP-metatekens correct geëscaped of gefilterd in de gebruikersinvoer?
- [ ] Ja — tekens zoals `(`, `)`, `&`, `|`, `*`, en `\` zijn **niet toegestaan** of worden correct geëscaped  
- [ ] Nee — metatekens **kunnen** worden geïnjecteerd om de structuur van het LDAP-filter te wijzigen  

### Kan het authenticatiemechanisme worden omzeild via injectie?
- [ ] Nee — de inloglogica is **niet vatbaar** voor query-manipulatie  
- [ ] Ja — authenticatie **kan** worden omzeild door gebruik te maken van logische OR (`|`) of wildcard (`*`) injectie in de gebruikersnaam- of wachtwoordvelden  

### Is blinde data-exfiltratie mogelijk vanuit de directoryservice?
- [ ] Nee — zoekresultaten zijn beperkt en er worden geen timing- of boolean-verschillen waargenomen  
- [ ] Ja — directory-attributen **kunnen** bit-voor-bit worden geënumeerd via boolean-based blind injection technieken  
- [ ] Ja — volledige directoryrecords **worden** weerspiegeld in de applicatierespons als gevolg van query-manipulatie  

### Zijn secundaire applicatiecomponenten (bijv. mailers, SSO) kwetsbaar voor LDAP-gebaseerde attribuut-injectie?
- [ ] Nee — LDAP-attributen worden veilig afgehandeld in alle componenten  
- [ ] Ja — een aanvaller **kan** zijn eigen attributen (bijv. e-mail, groepslidmaatschap) wijzigen via injectie om privileges te escaleren of communicatie om te leiden

---

## WSTG-INPV-07 — XML Injection

XML Injection treedt op wanneer een applicatie door de gebruiker verstrekte gegevens opneemt in een XML-document of -stream zonder de juiste validatie, sanitization of escaping van XML meta-characters. Door specifieke XML-tags of structurele elementen te injecteren, kan een aanvaller de logica van het document manipuleren, authenticatiemechanismen omzeilen of de integriteit van de door de backend verwerkte gegevens verstoren. Deze kwetsbaarheid wordt vaak aangetroffen in op SOAP gebaseerde webdiensten, REST API's die XML accepteren, SAML-assertions en applicaties die dynamisch XML-configuratiebestanden of rapporten genereren. Vanuit het perspectief van een aanvaller is het doel om uit de beoogde gegevenscontext te breken om de documentstructuur te wijzigen, wat mogelijk kan leiden tot ongeautoriseerde privilege escalation of de uitvoering van onbedoelde business logic.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Tools:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### Accepteert en verwerkt de applicatie door XML geformatteerde input van de gebruiker?
- [ ] Nee — applicatie verwerkt **geen** XML-input  
- [ ] Ja — XML-input wordt verwerkt via API's (SOAP/REST)  
- [ ] Ja — XML wordt verwerkt via bestandsuploads (SVG, DOCX, etc.)  

### Worden XML meta-characters (bijv. `<`, `>`, `&`, `'`, `"`) op de juiste manier ge-escaped of gesanitize?
- [ ] Ja — alle meta-characters worden **correct** ge-escaped of gesanitize *(Meest veilig)*  
- [ ] Ja — er wordt enige filtering toegepast, maar een bypass **is mogelijk** met behulp van encoding  
- [ ] Nee — ruwe input wordt direct in XML-structuren ingebed **zonder** sanitization  

### Wordt server-side schema-validatie (XSD/DTD) afgedwongen voor alle XML-inputs?
- [ ] Ja — strikte schema-validatie is **ingeschakeld** en voorkomt structurele wijzigingen  
- [ ] Ja — validatie is **ingeschakeld**, maar voorkomt **geen** tag-injectie binnen datavelden  
- [ ] Nee — schema-validatie is **uitgeschakeld** of niet geïmplementeerd  

### Kan een aanvaller nieuwe XML-tags injecteren om de documentstructuur of -logica te wijzigen?
- [ ] Nee — structuur blijft intact ongeacht de input  
- [ ] Ja — tags kunnen worden geïnjecteerd, maar kunnen de business logic **niet** wijzigen  
- [ ] Ja — tag-injectie **is mogelijk** en staat logica-bypass of gegevensmodificatie toe *(Kritiek)*  

### Kan de XML-parser worden gemanipuleerd met behulp van CDATA-secties of XML-commentaren?
- [ ] Nee — parser verwerkt of weigert CDATA/commentaar-markers correct  
- [ ] Ja — injectie van `<![CDATA[...]]>` of `<!-- ... -->` **is mogelijk** om filters te verbergen of te omzeilen

---

## WSTG-INPV-08 — Testing for SSI Injection

Server-Side Includes (SSI) Injection vindt plaats wanneer een applicatie er niet in slaagt gebruikersinvoer op te schonen voordat deze wordt opgenomen in HTML-bestanden die worden verwerkt door de SSI-engine van de server. Aanvallers misbruiken dit door SSI-directives te injecteren, zoals `<!--#exec cmd="id" -->`, om willekeurige shell-commando's uit te voeren, lokale bestanden te lezen of omgevingsvariabelen te exfiltreren. Deze kwetsbaarheid wordt doorgaans aangetroffen in applicaties die verouderde bestandsextensies gebruiken zoals `.shtml`, `.shtm`, of `.stm`, of wanneer de webserver expliciet is geconfigureerd om SSI te parsen binnen standaard HTML-bestanden. Succesvolle exploitatie verleent de aanvaller dezelfde rechten als het webserverproces, wat vaak leidt tot een volledig systeemcompromis of de blootstelling van gevoelige gegevens.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Ondersteunt en verwerkt de webserver SSI-directives?
- [ ] Nee — server ondersteunt **geen** SSI of bestandsextensies zoals `.shtml` zijn **uitgeschakeld**  
- [ ] Ja — SSI **is ingeschakeld** maar gebruikersinvoer wordt **niet** gereflecteerd in verwerkte pagina's  
- [ ] Ja — SSI **is ingeschakeld** en gebruikersinvoer wordt **wel** gereflecteerd in verwerkte pagina's  

### Kunnen willekeurige systeemcommando's worden uitgevoerd via de `#exec` directive?
- [ ] Nee — `#exec` directive is **uitgeschakeld** of beperkt door de serverconfiguratie  
- [ ] Ja — uitvoering van commando's **is mogelijk** via `#exec cmd` of `#exec cgi` payloads  

### Is de exfiltratie van gevoelige bestanden of omgevingsvariabelen mogelijk?
- [ ] Nee — directives zoals `#include` of `#config` zijn **uitgeschakeld**  
- [ ] Ja — het lezen van lokale bestanden (bijv. `/etc/passwd`) **is mogelijk** via `#include virtual`  
- [ ] Ja — server-omgevingsvariabelen **kunnen** worden geëxfiltreerd via `#printenv` of `#echo`  

### Zijn invoervalidatie of WAF-controles effectief tegen SSI-payloads?
- [ ] Ja — strikte invoervalidatie **is toegepast** en omzeiling is **niet mogelijk**  
- [ ] Ja — validatie/WAF **is toegepast** maar omzeiling **is mogelijk** door gebruik te maken van karakter-encoding of afwijkende SSI-syntaxis  
- [ ] Nee — er is geen validatie of WAF-beveiliging aanwezig voor SSI-karakters (`<`, `!`, `#`, `-`, `"`)

---

## WSTG-INPV-09 — Testen op XPath Injection

XPath Injection vindt plaats wanneer een applicatie door de gebruiker verstrekte informatie gebruikt om een XPath-query voor XML-gegevens samen te stellen, waardoor een aanvaller de logica van de query kan verstoren. Door specifieke syntaxis te injecteren, zoals enkele aanhalingstekens, haakjes en logische operatoren, kan een aanvaller door de XML-documentstructuur navigeren, authenticatie omzeilen of gevoelige gegevensnodes exfiltreren. Deze kwetsbaarheid wordt vaak aangetroffen in webservices, configuratiebeheerinterfaces en legacy-systemen die vertrouwen op XML-gebaseerde gegevensopslag in plaats van traditionele SQL-databases. Vanuit het perspectief van een aanvaller wordt dit vaak misbruikt met behulp van boolean-based of error-based technieken om systematisch de XML-boom in kaart te brengen en verborgen waarden uit de backend op te halen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Tools:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Worden gebruikersinputs die worden gebruikt voor het samenstellen van XPath-queries op de juiste wijze gesaneerd of geparametriseerd?
- [ ] Ja — inputs worden **niet** gebruikt in XPath-queries of zijn strikt geparametriseerd  
- [ ] Ja — robuuste input-validatie/encoding **is toegepast** en bypass is **niet mogelijk**  
- [ ] Ja — enige validatie **is toegepast**, maar bypass **is mogelijk** met behulp van geëncodeerde tekens  
- [ ] Nee — gebruikersinput wordt direct samengevoegd (concatenated) in XPath-queries *(Kritiek)*  

### Onthult de applicatie structurele XML/XPath-details via foutmeldingen?
- [ ] Nee — generieke foutpagina's worden getoond en er worden **geen** XML-details gelekt  
- [ ] Ja — foutmeldingen onthullen de aanwezigheid van XML-verwerking, maar **geen** details over het pad  
- [ ] Ja — uitgebreide foutmeldingen onthullen XPath-syntaxis, nodenamen of bestandspaden  

### Is het mogelijk om authenticatie- of autorisatielogica te omzeilen met behulp van XPath-logica?
- [ ] Nee — authenticatielogica vertrouwt **niet** op XML/XPath of is veilig geïmplementeerd  
- [ ] Ja — login- of permissiecontroles kunnen worden omzeild met behulp van op logica gebaseerde payloads zoals `' or 1=1 or '`  

### Kan de XML-documentstructuur of data worden geënumereerd via blind injection?
- [ ] Nee — de applicatie is **niet** vatbaar voor gevolgtrekking (inference) op basis van booleaanse waarden of tijd  
- [ ] Ja — nodenamen en gegevens **kunnen** worden geëxfiltreerd met behulp van boolean-based inference  
- [ ] Ja — volledige XML-tree traversal en data-extractie **is mogelijk**

---

## WSTG-INPV-10 — IMAP SMTP Injection

IMAP- en SMTP-injectiekwetsbaarheden ontstaan wanneer een webapplicatie door de gebruiker verstrekte gegevens onvoldoende filtert voordat deze worden opgenomen in commando's die naar een mailserver worden verzonden. Door Carriage Return en Line Feed (CRLF) karakters te injecteren, kan een aanvaller het beoogde commando beëindigen en willekeurige mailinstructies toevoegen, zoals het toevoegen van extra ontvangers (CC/BCC) of het wijzigen van de berichtinhoud. Deze kwetsbaarheden worden het vaakst aangetroffen in web-to-mail gateways, contactformulieren en e-mailclients die rechtstreeks met mailservers communiceren via protocollen zoals IMAP4 of SMTP. Succesvolle exploitatie maakt het ongeautoriseerd doorsturen (relaying) van spam mogelijk, evenals de exfiltratie van gevoelige e-mailinhoud en, in sommige gevallen, de volledige compromittering van de integriteit van de mailserver.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Verwerkt de applicatie door de gebruiker verstrekte invoer om IMAP- of SMTP-commando's op te stellen?
- [ ] Nee — de applicatie communiceert **niet** met mailservers via gebruikersinvoer  
- [ ] Ja — de applicatie communiceert met mailservers maar gebruikt een veilige API of library  
- [ ] Ja — de applicatie stelt ruwe (raw) mailcommando's op met parameters die door de gebruiker worden gecontroleerd  

### Wordt invoer gevalideerd om de injectie van Carriage Return (`\r`) en Line Feed (`\n`) sequenties te voorkomen?
- [ ] Ja — CRLF-karakters worden **strikt** gefilterd, gecodeerd of geweigerd  
- [ ] Ja — invoer wordt gevalideerd tegen een strikte allowlist van karakters  
- [ ] Nee — CRLF-karakters worden **niet** gefilterd, wat commandobeëindiging en injectie mogelijk maakt  

### Kan een aanvaller aanvullende mailheaders zoals `Bcc:` of `Cc:` injecteren?
- [ ] Nee — het wijzigen van headers is **niet mogelijk** vanwege strikte invoerbeperkingen  
- [ ] Ja — injectie van aanvullende headers **is mogelijk** via CRLF-sequenties  
- [ ] Ja — mail relaying naar externe domeinen **is ingeschakeld** en exploiteerbaar *(Kritiek)*  

### Staat de applicatie de manipulatie van IMAP-commando's toe om toegang te krijgen tot ongeautoriseerde mailboxen?
- [ ] Nee — de applicatie gebruikt **geen** IMAP of beperkt de toegang tot de mailbox strikt tot de geauthenticeerde gebruiker  
- [ ] Ja — IMAP-commando-injectie **is mogelijk** maar beperkt tot metadata-enumeratie  
- [ ] Ja — IMAP-commando-injectie maakt het lezen of verwijderen van e-mails van andere gebruikers mogelijk  

### Is de backend mailserver kwetsbaar voor command pipelining?
- [ ] Nee — de mailserver weigert gebatchte commando's of meerdere commando's in een enkele sessie  
- [ ] Ja — de mailserver verwerkt meerdere commando's die via een enkel invoerveld zijn geïnjecteerd

---

## WSTG-INPV-11 — Code Injection

Code-injectie treedt op wanneer een applicatie door de gebruiker aangeleverde codefragmenten evalueert binnen de eigen runtime-omgeving, meestal via onveilige taalspecifieke functies zoals `eval()`, `exec()`, of `system()`. Deze kwetsbaarheid verschilt van Command Injection omdat het zich richt op de executiecontext van de programmeertaal (bijv. PHP, Python, Node.js) in plaats van de shell van het onderliggende besturingssysteem. Aanvallers misbruiken deze punten om willekeurige logica uit te voeren, omgevingsvariabelen te exfiltreren of volledige Remote Code Execution (RCE) op de server te bewerkstelligen. Pentesters moeten functies onderzoeken die wiskundige expressies, server-side templates of dynamische configuratieparameters verwerken waarbij invoer geïnterpreteerd zou kunnen worden als uitvoerbare code.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Critical |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Tools:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Evalueren applicatiefuncties dynamische invoer via taalspecifieke evaluatiefuncties?
- [ ] Nee — dynamische evaluatiefuncties **zijn niet aanwezig** in de codebase  
- [ ] Ja — functies zoals `eval()`, `exec()`, of `include()` worden gebruikt, maar accepteren **geen** gebruikersinvoer  
- [ ] Ja — functies worden gebruikt en verwerken door de gebruiker aangeleverde invoer  

### Worden er mechanismen voor input-validatie of sanitisatie toegepast op de invoer vóór evaluatie?
- [ ] Ja — invoer wordt strikt gevalideerd tegen een **hardcoded whitelist**  
- [ ] Ja — invoer wordt gesaneerd via blacklisting, maar een bypass **is mogelijk**  
- [ ] Nee — invoer wordt direct doorgegeven aan de evaluatiefunctie en er worden **geen controles** toegepast  

### Is de executie-omgeving gesandboxed of beperkt om toegang op systeemniveau te voorkomen?
- [ ] Ja — de executie vindt plaats in een strikt beperkte sandbox waar **geen** system calls kunnen worden uitgevoerd  
- [ ] Ja — er bestaan enkele beperkingen, maar een sandbox-escape **is mogelijk**  
- [ ] Nee — de omgeving biedt volledige toegang tot de onderliggende taal-API's en OS-functies *(Critical)*  

### Kan de code-injectie worden gebruikt om gevoelige informatie te exfiltreren?
- [ ] Nee — de executie is blind en er is **geen** out-of-band communicatie mogelijk  
- [ ] Ja — omgevingsvariabelen of lokale bestanden **kunnen** worden geëxfiltreerd via HTTP/DNS-verzoeken  
- [ ] Ja — resultaten van de uitgevoerde code worden **direct gereflecteerd** in het antwoord van de applicatie  

### Staat de applicatie Remote File Inclusion of het dynamisch laden van bibliotheken toe?
- [ ] Nee — alleen lokale, vooraf gedefinieerde codepaden kunnen worden uitgevoerd  
- [ ] Ja — de applicatie **kan** worden gedwongen om code te laden en uit te voeren vanaf een externe of door de aanvaller beheerde bron

---

## WSTG-INPV-12 — Command Injection

Command Injection treedt op wanneer een applicatie onveilige, door de gebruiker verstrekte gegevens, zoals formulierinvoer, HTTP-headers of cookies, doorgeeft aan een system shell. Deze kwetsbaarheid stelt een aanvaller in staat om willekeurige besturingssysteemcommando's uit te voeren op de server, doorgaans met de rechten van het webapplicatieproces. Vanuit het perspectief van een aanvaller wordt dit misbruikt door shell-metatekens zoals puntkomma's, pipes of backticks te gebruiken om kwaadaardige commando's te koppelen aan de beoogde uitvoeringsstroom. De impact is vaak catastrofaal en leidt tot volledige systeemcompromittering, ongeautoriseerde gegevensexfiltratie of lateral movement binnen het interne netwerk.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Tools:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### Roept de applicatie commando's op systeemniveau of shell-executables aan?
- [ ] Nee — applicatie heeft **geen** interactie met de OS shell  
- [ ] Ja — applicatie maakt gebruik van interne API's of high-level libraries voor systeemtaken  
- [ ] Ja — applicatie roept rechtstreeks shell-commando's aan via system calls zoals `system()`, `exec()`, of `popen()`  

### Wordt gebruikersinvoer gevalideerd of gesaneerd voordat deze aan system calls wordt doorgegeven?
- [ ] Ja — strikte allow-listing en invoervalidatie **wordt toegepast**  
- [ ] Ja — sanering of escaping wordt gebruikt, maar een bypass **is mogelijk**  
- [ ] Nee — gebruikersinvoer wordt rechtstreeks naar de shell doorgegeven **zonder** validatie  

### Kunnen shell-metatekens worden gebruikt om de stroom van de commando-uitvoering te manipuleren?
- [ ] Nee — metatekens worden correct gefilterd of ge-escaped  
- [ ] Ja — in-band uitvoering waarbij de commando-output wordt gereflecteerd in de respons **is mogelijk**  
- [ ] Ja — blind uitvoering waarbij geen output wordt gereflecteerd **is mogelijk**  

### Is out-of-band (OOB) exfiltratie mogelijk vanaf de host?
- [ ] Nee — server heeft geen uitgaand verkeer (egress) of OOB-signalen (DNS/HTTP) falen  
- [ ] Ja — gegevensexfiltratie via DNS of HTTP-gebaseerde OOB-signalen **is mogelijk**  

### Draait het applicatieproces met verhoogde systeemprivileges?
- [ ] Nee — applicatie draait met een specifiek service-account met lage privileges  
- [ ] Ja — applicatie draait als `root`, `Administrator`, of een gebruiker met hoge privileges *(Kritiek)*

---

## WSTG-INPV-13 — Format String Injection

Format string injection vindt plaats wanneer een webapplicatie door de gebruiker gecontroleerde invoer rechtstreeks doorgeeft aan het format string-argument van een variadische functie, zoals de `printf`-familie in C of low-level logging-wrappers. Hoewel deze kwetsbaarheid minder vaak voorkomt in moderne managed-code webframeworks, blijft deze kritiek in verouderde (legacy) CGI-applicaties, native extensies of specifieke logging-systemen die interageren met low-level systeembibliotheken. Aanvallers misbruiken dit door format specifiers zoals `%x` of `%p` aan te leveren om gevoelige geheugenadressen en gegevens van de stack te lekken, of de `%n` specifier om willekeurige geheugenschrijfacties uit te voeren. Succesvolle exploitatie resulteert doorgaal in volledige systeemcompromitering via Remote Code Execution (RCE) of, minimaal, een impactvolle Denial-of-Service (DoS) conditie door procescrashes.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Verwerkt de applicatie gebruikersinvoer via native code of legacy CGI-interfaces?
- [ ] Nee — applicatie is volledig gebouwd op managed-code frameworks (bijv. JVM, CLR)  
- [ ] Ja — applicatie maakt gebruik van JNI, native C/C++ extensies of legacy binaire CGI's waar format string kwetsbaarheden **kunnen** bestaan  

### Wordt door de gebruiker verstrekte invoer doorgegeven als het primaire argument aan format-aware functies?
- [ ] Nee — invoer wordt doorgegeven als data-argumenten, en format strings zijn **statisch** en **hardcoded**  
- [ ] Ja — gebruikersinvoer wordt direct geconcatenereerd in het format string-argument, maar opschoning (sanitization) **wordt toegepast**  
- [ ] Ja — gebruikersinvoer wordt direct gebruikt als de format string en er **wordt geen** opschoning toegepast  

### Is het mogelijk om geheugeninhoud te lekken met behulp van format specifiers?
- [ ] Nee — invoer wordt gevalideerd en specifiers zoals `%p`, `%x` of `%s` **worden niet verwerkt**  
- [ ] Ja — het aanleveren van `%p` of `%x` specifiers resulteert in de reflectie van stack- of heap-geheugenadressen  
- [ ] Ja — gevoelige gegevens (bijv. pointers, stack cookies) **kunnen** worden geëxfiltreerd via format specifiers  

### Kan de applicatie worden gecrasht of gemanipuleerd met behulp van de `%n` specifier?
- [ ] Nee — specifiers die geheugenschrijfacties toestaan zijn **uitgeschakeld** of geblokkeerd door de compiler/omgeving  
- [ ] Ja — de applicatie crasht (DoS) wanneer `%s%s%s%s` of vergelijkbare strings worden opgegeven  
- [ ] Ja — willekeurige geheugenschrijfacties via `%n` zijn **mogelijk**, wat potentieel kan leiden tot Remote Code Execution (RCE)  

### Zijn moderne binaire beveiligingen (ASLR, DEP, Stack Canaries) te omzeilen via deze injectie?
- [ ] Nee — de omgeving is gehard (hardened) en geheugenlekken **kunnen niet** worden gebruikt om beveiligingen te omzeilen  
- [ ] Ja — format string injection **kan** worden gebruikt om basisadressen te lekken, waardoor het omzeilen van ASLR/DEP **mogelijk** wordt gemaakt

---

## WSTG-INPV-14 — Testing for Incubated Vulnerabilities

Incubated vulnerabilities, ook wel persistente of second-order kwetsbaarheden genoemd, treden op wanneer een kwaadaardige payload door de applicatie in een "cold" state wordt opgeslagen en later wordt uitgevoerd of verwerkt in een andere context. Dit aanvalspatroon richt zich doorgaans op back-end systemen, administratieve consoles of geautomatiseerde rapportagetools die gegevens ophalen uit een gedeelde database of bestandssysteem zonder adequate her-validatie of output encoding. Pentesters moeten de levenscyclus van gebruikersinvoer volgen vanaf het punt van binnenkomst (de "incubator") naar elke locatie waar die gegevens uiteindelijk worden gerenderd of gebruikt door andere componenten of secundaire applicaties. Succesvolle exploitatie leidt vaak tot resultaten met een hoge impact, zoals administratieve session hijacking via Stored XSS of data-exfiltratie via second-order SQL Injection in interne rapportagemodules.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Tools:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Zijn er endpoints waar door de gebruiker gecontroleerde invoer wordt opgeslagen voor latere ophaling door andere gebruikers of processen?
- [ ] Nee — de applicatie slaat geen gebruikersinvoer op voor later gebruik  
- [ ] Ja — invoer wordt opgeslagen in databasevelden, logbestanden of configuratie-instellingen  

### Wordt invoervalidatie of sanitization toegepast voordat gegevens naar de persistente opslaglaag worden geschreven?
- [ ] Ja — strikte allow-list validatie wordt toegepast op het punt van binnenkomst  
- [ ] Ja — generieke sanitization wordt toegepast, maar een bypass is mogelijk via encoding of niet-standaard karakters  
- [ ] Nee — invoer wordt opgeslagen in zijn ruwe, niet-gevalideerde vorm  

### Worden opgeslagen gegevens correct ge-encodeerd of gesaneerd wanneer ze worden gerenderd in administratieve of secundaire interfaces?
- [ ] Ja — contextbewuste output encoding wordt overal toegepast waar de gegevens worden gerenderd  
- [ ] Ja — encoding wordt op sommige locaties toegepast, maar ontbreekt op andere (bijv. intern admin-dashboard)  
- [ ] Nee — gegevens worden gerenderd zonder enige encoding of sanitization, wat de uitvoering van payloads mogelijk maakt  

### Kunnen payloads die in de database zijn opgeslagen invloed hebben op back-end batchprocessen of out-of-band monitoringsystemen?
- [ ] Nee — back-end processen maken gebruik van veilige parsing-methoden of geparametriseerde queries  
- [ ] Ja — opgeslagen payloads kunnen interacties in de back-end logica triggeren (bijv. CSV injection, command injection in logs)  
- [ ] Ja — opgeslagen payloads kunnen out-of-band (OOB) verzoeken naar door de aanvaller gecontroleerde servers triggeren

---

## WSTG-INPV-15 — HTTP Splitting Smuggling

HTTP Splitting- en Smuggling-kwetsbaarheden ontstaan door discrepanties in de manier waarop frontend-proxy's en backend-servers HTTP-verzoekgrenzen interpreteren en verwerken, met name met betrekking tot de `Content-Length` en `Transfer-Encoding` headers. Door dubbelzinnige verzoeken te formuleren, kan een aanvaller een verborgen verzoek naar de backend "smokkelen" of CRLF-sequenties injecteren om een respons te splitsen, wat leidt tot cache poisoning, request hijacking of het omzeilen van security controls. Deze gebreken manifesteren zich doorgaans in complexe omgevingen die gebruikmaken van reverse proxies, load balancers of CDN's met inconsistente parsing-logica. Vanuit het perspectief van een aanvaller maakt dit het omleiden van gebruikersverkeer, diefstal van gevoelige sessietokens en het uitvoeren van ongeautoriseerde acties binnen de context van sessies van andere gebruikers mogelijk.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Is de omgeving vatbaar voor request smuggling via CL.TE- of TE.CL-discrepanties?
- [ ] Nee — frontend- en backend-servers verwerken verzoekgrenzen consistent  
- [ ] Ja — er bestaan discrepanties, maar exploitatie is **niet mogelijk** vanwege infrastructuur-mitigaties  
- [ ] Ja — CL.TE- of TE.CL-smuggling **is mogelijk**, waardoor verborgen verzoeken de backend kunnen bereiken *(Hoog)*  
- [ ] Ja — TE.TE (dubbele codering/obfuscatie) **is mogelijk** om frontend-filters te omzeilen *(Kritiek)*  

### Kan HTTP Response Splitting worden bereikt via CRLF-injectie in headers?
- [ ] Nee — de applicatie saneert CRLF-sequenties correct in alle header-inputs  
- [ ] Ja — CRLF-sequenties worden gereflecteerd in headers, maar response splitting is **niet mogelijk**  
- [ ] Ja — CRLF-injectie **is mogelijk**, wat header-injectie of cache poisoning toestaat  

### Worden security controls (WAF/ACLs) omzeild met behulp van gesmokkelde verzoeken?
- [ ] Nee — security controls zijn van toepassing op zowel de buitenste als de gesmokkelde verzoeken  
- [ ] Ja — gesmokkelde verzoeken **kunnen** frontend WAF-regels of op IP gebaseerde ACL's omzeilen  

### Is het mogelijk om sessies van andere gebruikers te kapen of hun verkeer om te leiden?
- [ ] Nee — verzoek/respons-stromen zijn geïsoleerd en kunnen niet worden gekruist  
- [ ] Ja — request smuggling **is mogelijk** en maakt het vastleggen van verzoeken van andere gebruikers mogelijk *(Kritiek)*  
- [ ] Ja — response splitting **is mogelijk** en maakt browser-side cache poisoning of XSS mogelijk

---

## WSTG-INPV-16 — HTTP Request Smuggling

HTTP Request Smuggling (HRS) treedt op wanneer een keten van HTTP-servers (zoals een load balancer en een back-end webserver) de grenzen van een verzoek verschillend interpreteert, meestal als gevolg van conflicterende `Content-Length` en `Transfer-Encoding` headers. Een aanvaller exploiteert deze desynchronisatie om een item in de request buffer van de back-end server te "smokkelen", waardoor het verzoek van de volgende legitieme gebruiker effectief wordt voorafgegaan door een segment dat door de aanvaller wordt beheerd. Deze techniek maakt ernstige aanvallen mogelijk, waaronder session hijacking, het omzeilen van beveiligingscontroles en cache poisoning. Exploitatie wordt doorgaans uitgevoerd door middel van timing-gebaseerde analyse of door het observeren van onverwachte antwoorden van de back-end wanneer opeenvolgende verzoeken naar de server worden verzonden.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Kritiek / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Verwerken de front-end en back-end servers de `Transfer-Encoding: chunked` header op consistente wijze?
- [ ] Ja — beide servers verwerken chunked encoding op identieke wijze en desynchronisatie is **niet mogelijk**  
- [ ] Nee — servers interpreteren headers verschillend, maar opschoning/normalisatie **wordt toegepast**  
- [ ] Nee — CL.TE (Content-Length/Transfer-Encoding) desynchronisatie is **mogelijk** *(Kritiek)*  
- [ ] Nee — TE.CL (Transfer-Encoding/Content-Length) desynchronisatie is **mogelijk** *(Kritiek)*  

### Kan de aanvaller de server- of client-side cache vergiftigen met een gesmokkeld verzoek?
- [ ] Nee — caching is **uitgeschakeld** of niet vatbaar voor poisoning  
- [ ] Ja — gesmokkelde verzoeken **kunnen** gebruikers omleiden naar kwaadaardige domeinen of scripts injecteren via cache poisoning  

### Is het mogelijk om verzoeken of sessietokens van andere gebruikers te onderscheppen via request concatenation?
- [ ] Nee — smuggling staat blootstelling van gegevens tussen gebruikers **niet** toe  
- [ ] Ja — smuggling maakt het mogelijk om het verzoek van de volgende gebruiker toe te voegen aan een door de aanvaller beheerde `POST` body, waardoor exfiltratie **mogelijk** is  

### Maakt de infrastructuur gebruik van HTTP/2 en is deze vatbaar voor H2.CL of H2.TE desynchronisatie?
- [ ] Nee — de applicatie gebruikt alleen HTTP/1.1 of HTTP/2 is veilig geïmplementeerd zonder downgrading  
- [ ] Ja — er vinden HTTP/2 naar HTTP/1.1 cleartext downgrades plaats en desynchronisatie is **mogelijk**  

### Worden onjuist geformatteerde headers (bijv. `Transfer-Encoding:  chunked` met een spatie) veilig afgehandeld?
- [ ] Ja — onjuist geformatteerde headers worden geweigerd of genormaliseerd in alle lagen  
- [ ] Nee — TE.TE desynchronisatie is **mogelijk** vanwege inconsistente afhandeling van onjuist geformatteerde of verhulde headers

---

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

## WSTG-INPV-18 — Testen op Server-Side Template Injection

Server-Side Template Injection (SSTI) vindt plaats wanneer een applicatie door de gebruiker gecontroleerde input invoegt in een server-side template engine zonder voldoende sanitization of escaping. Dit stelt een aanvaller in staat om kwaadaardige template-directives te injecteren, die vervolgens op de server worden uitgevoerd, wat potentieel kan leiden tot een volledige systeemcompromis via remote code execution (RCE). SSTI manifesteert zich doorgaans in functionaliteiten zoals dynamische e-mailgeneratie, profielpagina's en notificatiesystemen die engines gebruiken zoals Jinja2, Twig of FreeMarker. Vanuit het perspectief van een aanvaller wordt deze kwetsbaarheid vaak misbruikt om gevoelige omgevingsvariabelen te lekken, interne bestanden te lezen of systeemcommando's uit te voeren door gebruik te maken van de native objecten en methoden van de template engine.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Tools:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### Maakt de applicatie gebruik van een server-side template engine om gebruikersinput te verwerken?
- [ ] Nee — applicatie gebruikt geen server-side templates voor dynamische content  
- [ ] Ja — templates worden gebruikt, maar gebruikersinput wordt **niet** ingebed in template-directives  
- [ ] Ja — gebruikersinput wordt ingebed in templates **zonder** de juiste sanitization  

### Worden basis rekenkundige expressies geëvalueerd wanneer ze in invoervelden worden geïnjecteerd?
- [ ] Nee — payloads zoals `{{7*7}}` of `${7*7}` worden weergegeven als letterlijke strings  
- [ ] Ja — expressies worden geëvalueerd (bijv. resultaat `49`), wat bevestigt dat de template engine **kwetsbaar is**  

### Kunnen de ingebouwde objecten of methoden van de template engine worden benaderd?
- [ ] Nee — toegang tot gevaarlijke objecten, methoden of configuraties is **beperkt** of vindt plaats in een sandbox  
- [ ] Ja — ingebouwde objecten (bijv. `self`, `config`, `__class__`) **kunnen** worden benaderd om gevoelige gegevens te lekken  
- [ ] Ja — remote code execution (RCE) via system calls of OS-libraries **is mogelijk**  

### Is er een sandbox- of allowlist-mechanisme aanwezig dat de uitvoering van gevaarlijke directives voorkomt?
- [ ] Ja — een strikte allowlist of beveiligde sandbox is aanwezig en een bypass is **niet mogelijk**  
- [ ] Ja — er is een sandbox aanwezig, maar een bypass **is mogelijk** via specifieke engine-afhankelijke payloads  
- [ ] Nee — er is **geen** sandbox of allowlist toegepast op de template engine

---

## WSTG-INPV-19 — Testen op Server-Side Request Forgery (SSRF)

Server-Side Request Forgery vindt plaats wanneer een aanvaller een webapplicatie kan beïnvloeden om ongeautoriseerde verzoeken te doen vanaf de server naar interne of externe bronnen. Door parameters te manipuleren die URL's, hostnamen of IP-adressen verwachten, kan een aanvaller controles op netwerkniveau, zoals firewalls en ACL's, omzeilen om interne netwerksegmenten te scannen, toegang te krijgen tot cloud metadata services (IMDS) of interactie te hebben met kwetsbare interne API's en databases. Deze kwetsbaarheid komt doorgaans voor in functionaliteiten voor het ophalen van afbeeldingen, PDF-generatie, webhooks of bestandsuploads via URL. Vanuit het perspectief van een aanvaller is SSRF een krachtig primitief dat wordt gebruikt om te pivoten naar geïsoleerde omgevingen, gevoelige configuratiegegevens te exfiltreren of mogelijk remote code execution te bereiken via interactie met interne services zoals Redis of Memcached.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Tools:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### Verwerkt de applicatie door de gebruiker opgegeven URL's of hostnamen?
- [ ] Nee — geen enkele functionaliteit accepteert externe URL's of hostnamen  
- [ ] Ja — functionaliteit bestaat, maar invoer **wordt strikt gevalideerd** tegen een allow-list  
- [ ] Ja — functionaliteit bestaat en accepteert door de gebruiker opgegeven URL's  

### Worden er validatiecontroles of zwarte lijsten toegepast op de opgevraagde bestemming?
- [ ] Ja — een strikte **allow-list** van vertrouwde domeinen/IP's wordt afgedwongen  
- [ ] Ja — een **deny-list** wordt gebruikt (bijv. het blokkeren van `127.0.0.1`, `169.254.169.254`)  
- [ ] Nee — er wordt **geen validatie** of filtering toegepast op de invoer  

### Is het mogelijk om filters te omzeilen via encoding, redirects of DNS rebinding?
- [ ] Nee — filters zijn robuust en verwerken redirects/DNS-resolutie op een veilige manier  
- [ ] Ja — bypass **is mogelijk** met behulp van URL-encoding of alternatieve IP-formaten (hex, octaal)  
- [ ] Ja — bypass **is mogelijk** via 3xx HTTP-redirects naar interne doelen  
- [ ] Ja — bypass **is mogelijk** via DNS rebinding-aanvallen om te herleiden naar interne IP-adressen  

### Kan de applicatie interne metadata services of lokale interfaces bereiken?
- [ ] Nee — lokaal netwerk en cloud metadata endpoints zijn **niet bereikbaar**  
- [ ] Ja — toegang tot localhost (`127.0.0.1`) of loopback-services **is mogelijk**  
- [ ] Ja — toegang tot cloud metadata services (bijv. AWS/GCP/Azure IMDS) **is mogelijk** *(Kritiek)*  

### Wat is de aard van de SSRF-respons?
- [ ] Blind — er wordt geen respons of metadata teruggestuurd naar de gebruiker  
- [ ] Gedeeltelijk — responsmetadata (headers, grootte, timing) wordt teruggestuurd naar de gebruiker  
- [ ] Volledig — de volledige response body van de interne bron **wordt gereflecteerd** naar de gebruiker

---

## WSTG-INPV-20 — Mass Assignment

Mass Assignment, ook bekend als Overposting of Insecure Deserialization van parameters, treedt op wanneer een webapplicatie inkomende HTTP-verzoekparameters automatisch koppelt aan interne objecteigenschappen zonder de juiste filtering van attributen. Deze kwetsbaarheid komt veel voor in moderne MVC-frameworks en RESTful API's waar JSON, XML, of formuliergecodeerde gegevens rechtstreeks worden gedeserialiseerd naar datamodellen aan de applicatiezijde. Aanvallers misbruiken dit gedrag door extra, niet-vermelde parameters in een verzoek te injecteren — zoals `isAdmin`, `role`, of `account_balance` — om gevoelige velden te wijzigen die de ontwikkelaars niet bedoeld hadden bloot te stellen aan gebruikerscontrole. Succesvolle exploitatie resulteert doorgaans in privilege-escalatie, ongeautoriseerde gegevenswijziging of het omzeilen van kritieke business logic-workflows.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Hoog / Gemiddeld* |

> *De ernst wordt Hoog als administratieve attributen, permissieniveaus of factureringsvelden kunnen worden gewijzigd.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### Gebruikt de applicatie automatische binding voor inkomende verzoekparameters?
- [ ] Nee — parameters worden handmatig toegewezen aan specifieke variabelen of veilige objecten  
- [ ] Ja — automatische binding wordt gebruikt, maar strikte **whitelists** of DTO's (Data Transfer Objects) worden afgedwongen  
- [ ] Ja — automatische binding wordt gebruikt en gevoelige interne attributen **kunnen** worden gewijzigd  

### Kunnen administratieve of permissie-gerelateerde attributen succesvol worden geïnjecteerd?
- [ ] Nee — geïnjecteerde parameters worden genegeerd, verwijderd of veroorzaken een validatiefout  
- [ ] Ja — extra parameters worden geaccepteerd, maar hebben **geen** invloed op de status van het backend-object  
- [ ] Ja — het injecteren van parameters zoals `is_admin`, `role`, of `permissions` leidt **succesvol** tot privilege-escalatie *(Kritiek)*  

### Is het mogelijk om gevoelige business logic of financiële velden te wijzigen via overposting?
- [ ] Nee — velden zoals `price`, `balance`, of `status` zijn beschermd tegen mass assignment  
- [ ] Ja — gevoelige zakelijke attributen **kunnen** worden gewijzigd door ze toe te voegen aan de JSON of de body van het formulierverzoek  

### Onthult de applicatie interne objectstructuren die het raden van parameters vergemakkelijken?
- [ ] Nee — antwoorden bevatten alleen de beoogde publieke velden en documentatie is beperkt  
- [ ] Ja — interne objectvelden zijn zichtbaar in GET-responses, waardoor parameter-discovery **mogelijk** is  
- [ ] Ja — API-documentatie (Swagger/OpenAPI) vermeldt expliciet interne eigenschappen die **toegewezen** kunnen worden

---

## WSTG-ERRH-01 — Testen op Onjuiste Foutafhandeling

Onjuiste foutafhandeling treedt op wanneer een webapplicatie gevoelige technische details—zoals stack traces, database schema-informatie of interne bestandspaden—vrijgeeft via foutmeldingen. Aanvallers lokken deze fouten uit door het indienen van malformed input, het opvragen van niet-bestaande bronnen of het forceren van server-side exceptions om de onderliggende architectuur van de applicatie in kaart te brengen en potentiële vectoren voor verdere exploitatie te identificeren. Dit lekken van informatie dient vaak als voorloper van ernstigere aanvallen, waaronder SQL Injection of path traversal, door de aanvaller te voorzien van nauwkeurige omgevingsspecificaties. Een veilige implementatie moet generieke, gebruiksvriendelijke meldingen tonen, terwijl gedetailleerde diagnostische gegevens alleen in beveiligde, server-side logs worden vastgelegd.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Laag / Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### Gebruikt de applicatie een generieke, globale error handler voor niet-afgehandelde exceptions?
- [ ] Ja — generieke foutpagina's zijn **ingeschakeld** en onthullen **geen** technische details  
- [ ] Ja — aangepaste foutpagina's worden gebruikt, maar lekkage **is mogelijk** via specifieke headers  
- [ ] Nee — standaard server-foutpagina's (bijv. Tomcat, IIS, Nginx) zijn **zichtbaar**  

### Kan technische informatie worden geënumereerd via malformed input of randgevallen?
- [ ] Nee — fouten worden netjes afgehandeld met unieke referentie-ID's voor logging  
- [ ] Ja — stack traces of database-queries **worden vrijgegeven** in responses  
- [ ] Ja — interne bestandspaden, omgevingsvariabelen of serverversie-strings **worden vrijgegeven**  

### Lekken API-responses uitgebreide error-objecten of debug-informatie?
- [ ] Nee — API's retourneren gestandaardiseerde foutcodes en geschoonde JSON-berichten  
- [ ] Ja — API's retourneren verbose debug-objecten of volledige exception-details in de response body  
- [ ] Ja — stack traces zijn opgenomen in de `details` of `exception` velden van de JSON-response  

### Gedraagt de applicatie zich anders (Time-based of Content-based) wanneer er fouten optreden?
- [ ] Nee — response-signatures en timings zijn consistent, ongeacht het type fout  
- [ ] Ja — differentiële responses **kunnen** worden gebruikt om geldige versus ongeldige toestanden te enumereren (bijv. User Enumeration)

---

## WSTG-ERRH-02 — Testen op Stack Traces

Stack traces worden gegenereerd wanneer een applicatie een exceptie niet op een correcte manier afhandelt, wat resulteert in het onthullen van de interne uitvoeringsstatus en de aanroep-hiërarchie aan de eindgebruiker. Voor een penetratietester zijn deze traces van onschatbare waarde, omdat ze het applicatieframework, bibliotheekversies, interne bestandssysteempaden en de logische flow onthullen, wat de benodigde inspanning voor reconnaissance aanzienlijk vermindert. Exploitatie omvat het opzettelijk veroorzaken van fouten via misvormde invoer, onverwachte datatypen of schendingen van grensvoorwaarden om de applicatie in een onstabiele staat te dwingen en informatie over de onderliggende technologiestack te exfiltreren. Dit lekken van informatie biedt vaak de noodzakelijke context om kwetsbare componenten van derden te identificeren of om specifieke exploits te ontwerpen voor logische fouten die zijn ontdekt in de onthulde codepaden.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld* |

> *De ernst stijgt naar Gemiddeld als de stack trace gevoelige architecturale details, databasequery's of interne configuratieparameters onthult.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Worden volledige stack traces teruggestuurd naar de client bij applicatiefouten?
- [ ] Nee — de applicatie retourneert generieke foutpagina's of aangepaste foutmeldingen  
- [ ] Ja — stack traces zijn **ingeschakeld**, maar alleen voor specifieke, niet-gevoelige modules  
- [ ] Ja — volledige stack traces zijn **ingeschakeld** en zichtbaar in de HTTP response body  

### Onthullen foutmeldingen gevoelige omgevingsinformatie?
- [ ] Nee — foutmeldingen bevatten geen technische of omgevingsdetails  
- [ ] Ja — meldingen onthullen **interne bestandspaden** of server-side **mapstructuren**  
- [ ] Ja — meldingen onthullen **databaseschema's**, **SQL-query's** of **versies van bibliotheken van derden**  

### Kunnen stack traces worden getriggerd door het manipuleren van invoerparameters?
- [ ] Nee — misvormde invoer wordt correct afgehandeld of geweigerd door validatie  
- [ ] Ja — traces kunnen worden getriggerd door **onverwachte datatypen** (bijv. het indienen van een array waar een string wordt verwacht)  
- [ ] Ja — traces worden getriggerd door **null bytes**, **speciale tekens** of **grenswaarden**  

### Wordt er consistent een globale error handler toegepast in de gehele applicatie?
- [ ] Ja — een globale exception handler wordt **consistent toegepast** op alle geteste endpoints  
- [ ] Ja — er is een handler aanwezig, maar deze is **niet toegepast** op legacy, API of nieuw toegevoegde endpoints  
- [ ] Nee — er is geen globaal mechanisme voor foutafhandeling **gedetecteerd**, wat resulteert in standaard serverfouten

---

## WSTG-CRYP-01 — Testen op zwakke Transport Layer Security

Het testen op zwakke transport layer security omvat het analyseren van de implementatie en configuratie van SSL/TLS om de vertrouwelijkheid en integriteit van gegevens tijdens het transport te waarborgen. Aanvallers richten zich op misconfiguraties zoals verouderde protocollen (SSLv2, TLS 1.0), zwakke cipher suites (NULL, RC4 of anoniem) en ongeldige of zwak ondertekende certificaten om Man-in-the-Middle (MitM) aanvallen uit te voeren. Deze test richt zich doorgaans op de webserver of load balancer-endpoints van de applicatie waar versleutelde communicatie tot stand wordt gebracht. Succesvolle exploitatie stelt een tegenstander in staat om gevoelige gegevens te onderscheppen, gebruikerssessies te kapen of verbindingen te downgraden naar onveilige staten via protocol downgrade of traffic decryption.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Gemiddeld* |

> *De ernst is Hoog als gevoelige gegevens (PII, inloggegevens, session tokens) worden verzonden; Gemiddeld voor algemeen informatief verkeer.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Worden verouderde of onveilige TLS/SSL-protocollen ondersteund?
- [ ] Nee — alleen TLS 1.2 en TLS 1.3 zijn **ingeschakeld**  
- [ ] Ja — TLS 1.0 of TLS 1.1 zijn **ingeschakeld**  
- [ ] Ja — SSLv2 of SSLv3 zijn **ingeschakeld** *(Kritiek)*  

### Worden zwakke of onveilige cipher suites geaccepteerd door de server?
- [ ] Nee — alleen sterke, moderne ciphers (bijv. AES-GCM, ChaCha20) worden **ondersteund**  
- [ ] Ja — ciphers met zwakke versleuteling (bijv. 3DES, RC4) of lage bitlengte worden **ondersteund**  
- [ ] Ja — NULL, anonieme (ADH) of export ciphers worden **ondersteund** *(Kritiek)*  

### Is het servercertificaat geldig en vertrouwd?
- [ ] Ja — certificaat is geldig, komt overeen met het domein en is ondertekend door een **vertrouwde CA**  
- [ ] Nee — certificaat is **verlopen** of gebruikt een **zwak signature-algoritme** (bijv. SHA-1)  
- [ ] Nee — certificaat is **self-signed** of er is sprake van een **domeinnaam-mismatch**  

### Ondersteunt de server Secure Renegotiation en voorkomt deze Downgrade-aanvallen?
- [ ] Ja — Secure Renegotiation wordt **ondersteund** en TLS Fallback SCSV is **ingeschakeld**  
- [ ] Nee — Secure Renegotiation wordt **niet ondersteund**, wat mogelijk MitM-injectie toestaat  
- [ ] Nee — Server is kwetsbaar voor **POODLE** of **ROBOT** aanvallen  

### Is HTTP Strict Transport Security (HSTS) correct geïmplementeerd?

| Header | Aanwezig | Ontbrekend |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Ja — HSTS-header is aanwezig met een lange `max-age` en `includeSubDomains` is **ingeschakeld**  
- [ ] Ja — HSTS-header is aanwezig maar `includeSubDomains` ontbreekt of heeft een **korte max-age**  
- [ ] Nee — HSTS-header is **niet aanwezig**

---

## WSTG-CRYP-02 — Testen op Padding Oracle

Padding Oracle-kwetsbaarheden treden op wanneer het decryptieproces van een applicatie informatie lekt over de vraag of de padding van een gedecrypteerde cijfertekst (ciphertext) valide of invalide is. Door deze side-channel-responsen te observeren — zoals afwijkende HTTP-statuscodes, specifieke foutmeldingen of variaties in responstijd — kan een aanvaller gegevens ontsleutelen zonder de encryptiesleutel te kennen en potentieel willekeurige klare tekst (plaintext) opnieuw versleutelen. Deze kwetsbaarheid manifesteert zich doorgaans in versleutelde sessiecookies, authenticatietokens of URL-parameters die gebruikmaken van blokcijfers (block ciphers) in Cipher Block Chaining (CBC)-modus met PKCS#7-padding. Exploitatie leidt tot een volledig verlies van vertrouwelijkheid en kan leiden tot integriteitsschendingen door de constructie van vervalste versleutelde blokken.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst (Severity)** | Hoog / Kritiek* |

> *De ernst is Kritiek als de kwetsbaarheid zich bevindt in sessiebeheer, identiteitstokens of PII-encryptie.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Wordt er een blokcijfer in CBC-modus gebruikt voor gevoelige gegevens?
- [ ] Nee — de applicatie gebruikt geauthenticeerde encryptie (AES-GCM/ChaCha20-Poly1305)  
- [ ] Ja — de applicatie gebruikt blokcijfers, maar padding **is niet** vereist (bijv. stroomcijfers of CTR-modus)  
- [ ] Ja — de applicatie gebruikt CBC-modus en padding **wordt toegepast**  

### Maakt de server de geldigheid van de padding bekend via side-channels?
- [ ] Nee — de server retourneert uniforme foutmeldingen en consistente responstijden  
- [ ] Ja — de server retourneert verschillende HTTP-statuscodes (bijv. 200 vs. 500) voor padding-fouten  
- [ ] Ja — de server retourneert specifieke foutmeldingen (bijv. "Invalid Padding", "Decryption failed")  
- [ ] Ja — de server vertoont meetbare tijdsverschillen tijdens een mislukte decryptie  

### Is geautomatiseerde decryptie van de cijfertekst mogelijk?
- [ ] Nee — side-channels zijn **niet** stabiel genoeg voor exploitatie  
- [ ] Ja — cijfertekst **kan** gedeeltelijk worden gedecrypteerd (de laatste paar blokken)  
- [ ] Ja — volledige herstel van de klare tekst (plaintext) **is mogelijk** met tools zoals `PadBuster`  

### Kan de aanvaller bit-flipping uitvoeren of valide cijferteksten vervalsen?
- [ ] Nee — integriteitscontroles (HMAC) worden geverifieerd **vóór** de decryptie  
- [ ] Ja — er is geen integriteitscontrole aanwezig, maar payload-constructie **is niet** haalbaar  
- [ ] Ja — de constructie van willekeurige cijferteksten **is mogelijk**, wat privilege-escalatie of data-injectie toestaat

---

## WSTG-CRYP-03 — Testen op gevoelige informatie verzonden via niet-versleutelde kanalen

Het verzenden van gevoelige gegevens via niet-versleutelde kanalen, zoals onversleuteld HTTP, stelt informatie bloot aan onderschepping en wijziging via Man-in-the-Middle (MITM) aanvallen. Deze kwetsbaarheid treedt doorgaans op wanneer webapplicaties nalaten Transport Layer Security (TLS) af te dwingen voor alle eindpunten, met name in legacy-componenten, inlogformulieren of tijdens de initiële overgang van HTTP naar HTTPS. Vanuit het perspectief van een aanvaller maakt dit het passief sniffen van authenticatiegegevens, sessietokens en persoonlijk identificeerbare informatie (PII) mogelijk op gecompromitteerde of onbetrouwbare netwerksegmenten zoals openbare wifi. Het waarborgen dat alle gevoelige gegevens zijn ingekapseld binnen een robuuste TLS-tunnel is fundamenteel voor het behoud van de vertrouwelijkheid en integriteit van gebruikersinteracties en het voorkomen van session hijacking.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Gemiddeld |

> *De ernst wordt Hoog als authenticatiegegevens, sessie-identificatoren of PII in cleartext worden verzonden.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### Staat de applicatie communicatie via niet-versleuteld HTTP toe voor gevoelige eindpunten?
- [ ] Nee — al het verkeer van de applicatie wordt strikt gedwongen over HTTPS *(Meest veilig)*  
- [ ] Ja — niet-gevoelige pagina's zijn toegankelijk via HTTP, maar gevoelige eindpunten **niet**  
- [ ] Ja — gevoelige eindpunten (login, checkout, profiel) **zijn** toegankelijk via niet-versleuteld HTTP *(Hoog risico)*  

### Is er een globale omleiding van HTTP naar HTTPS?
- [ ] Ja — de applicatie voert een 301/302 redirect uit naar HTTPS voor alle HTTP-verzoeken  
- [ ] Ja — omleiding wordt alleen uitgevoerd voor specifieke gevoelige eindpunten  
- [ ] Nee — er wordt **geen** omleiding van HTTP naar HTTPS toegepast  

### Is HTTP Strict Transport Security (HSTS) geïmplementeerd?
- [ ] Ja — HSTS is **ingeschakeld** met een lange `max-age` en bevat de `includeSubDomains` en `preload` directives  
- [ ] Ja — HSTS is **ingeschakeld** maar mist de `includeSubDomains` of `preload` directives  
- [ ] Nee — HSTS is **niet ingeschakeld** op de applicatieserver  

### Zijn gevoelige cookies beschermd tegen verzending in cleartext?

| Cookienaam | Secure Flag aanwezig | Secure Flag ontbreekt |
|------------|:--------------------:|:---------------------:|
| `SessionID` |                      |                       |
| `AuthToken` |                      |                       |
| `CSRF-Token`|                      |                       |

### Verzendt de applicatie gevoelige gegevens naar API's van derden via niet-versleutelde kanalen?
- [ ] Nee — alle externe API-calls worden uitgevoerd via versleutelde (HTTPS) tunnels  
- [ ] Ja — sommige niet-gevoelige telemetrie wordt verzonden via HTTP  
- [ ] Ja — API-keys, PII of inloggegevens **worden** naar derden verzonden via niet-versleuteld HTTP

---

## WSTG-CRYP-04 — Testen op Zwakke Cryptografische Primitives

Zwakke cryptografische primitives omvatten het gebruik van verouderde of onveilige algoritmen, zoals MD5, SHA-1, DES of RC4, die vatbaar zijn voor collision attacks, frequentieanalyse of brute-force decryptie. Deze zwakheden treden doorgaans op bij password hashing, data encryption at rest, generatie van session tokens of verificatie van digitale handtekeningen binnen de backend van de applicatie. Vanuit het perspectief van een aanvaller maakt het identificeren van deze primitives de inbreuk op data-integriteit en vertrouwelijkheid mogelijk, zoals het kraken van onderschepte hashes om cleartext wachtwoorden te achterhalen of het vervalsen van authenticatie-tokens. Moderne applicaties moeten in plaats daarvan gebruikmaken van industriestandaard, collision-resistant en high-entropy primitives zoals Argon2, Bcrypt of AES-GCM om robuuste bescherming tegen cryptanalyse te garanderen.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Worden verouderde hashing-algoritmen zoals MD5 of SHA-1 gebruikt voor de opslag van gevoelige gegevens?
- [ ] Nee — applicatie maakt gebruik van moderne collision-resistant algoritmen zoals Argon2 of Bcrypt  
- [ ] Ja — verouderde algoritmen worden gebruikt, maar **alleen** voor niet-beveiligingsgevoelige integriteitscontroles  
- [ ] Ja — verouderde algoritmen **worden gebruikt** voor wachtwoorden of gevoelige tokens *(Hoog)*  

### Worden er momenteel zwakke symmetrische encryptie-ciphers zoals DES, 3DES of RC4 toegepast?
- [ ] Nee — AES (128-bit of hoger) in een veilige modus zoals GCM of CBC wordt gebruikt  
- [ ] Ja — legacy ciphers zijn **ingeschakeld** voor achterwaartse compatibiliteit, maar zijn **niet** de standaard  
- [ ] Ja — legacy ciphers zijn de primaire methode voor het beschermen van data at rest of in transit  

### Implementeert de applicatie "roll-your-own" of niet-standaard cryptografische logica?
- [ ] Nee — applicatie houdt zich strikt aan standaard, peer-reviewed cryptografische libraries  
- [ ] Ja — er bestaan aangepaste wrappers, maar de onderliggende primitives **zijn** industriestandaard  
- [ ] Ja — eigen cryptografische algoritmen of aangepaste logica **wordt toegepast** op gevoelige gegevens *(Kritiek)*  

### Worden cryptografische salts en initialization vectors (IVs) beheerd volgens best practices?
- [ ] Ja — salts zijn uniek per gebruiker en IVs zijn onvoorspelbaar voor elke operatie  
- [ ] Ja — salts worden gebruikt, maar zijn **niet** uniek over alle records  
- [ ] Nee — salts zijn statisch, hardcoded of afwezig, en IVs **worden hergebruikt** over meerdere sessies  

### Is de bit-lengte van asymmetrische sleutels (RSA, Diffie-Hellman) voldoende voor moderne standaarden?
- [ ] Ja — RSA/DH-sleutels zijn 2048 bits of hoger, of Elliptic Curve (ECC) wordt gebruikt  
- [ ] Nee — sleutels zijn minder dan 2048 bits, maar bypass **is niet** triviaal  
- [ ] Nee — sleutels zijn 1024 bits of kleiner, waardoor ze **kwetsbaar** zijn voor factorisatie of computationele aanvallen

---

## WSTG-BUSL-01 — Testen van Validatie van Bedrijfslogica-gegevens

Het testen van validatie van bedrijfslogica-gegevens richt zich op het vermogen van de applicatie om gegevens te identificeren en te weigeren die syntactisch correct zijn, maar logisch ongeldig binnen de context van het bedrijfsdomein. Aanvallers maken misbruik van deze gebreken door onverwachte waarden in te dienen—zoals negatieve hoeveelheden in een winkelmandje, datums in het verleden voor toekomstige evenementen, of extreem grote gehele getallen—om beperkingen te omzeilen en de status van de applicatie te manipuleren. Deze kwetsbaarheden bevinden zich vaak in workflows met meerdere stappen, afrekenprocessen en accountbeheermodules waar de server-side logica nalaat de contextuele integriteit van door de gebruiker verstrekte gegevens te verifiëren. Succesvolle exploitatie kan leiden tot financieel verlies, aantasting van de integriteit en ongeoorloofde toegang tot beperkte functies of gegevens.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Dwingt de applicatie logische bereiklimieten af op numerieke invoer?
- [ ] Ja — de applicatie weigert correct negatieve, nul- of excessief grote waarden waar dit ongepast is *(Meest veilig)*  
- [ ] Ja — de applicatie weigert sommige ongeldige bereiken maar **is** kwetsbaar voor integer overflow of boundary bypass  
- [ ] Nee — de applicatie accepteert elke numerieke waarde ongeacht de zakelijke context *(Kritiek)*  

### Zijn server-side validatiecontroles consistent in alle lagen van de applicatie?
- [ ] Ja — validatie wordt consistent toegepast op zowel de API/service- als de databaselagen  
- [ ] Ja — validatie **wordt toegepast** in de UI/Frontend, maar bypass via een direct API-verzoek **is mogelijk**  
- [ ] Nee — validatie wordt **niet toegepast** of is inconsistent, wat leidt tot logische gegevenscorruptie  

### Kan de bedrijfslogica worden ondermijnd door gegevens in workflows met meerdere stappen te manipuleren?
- [ ] Nee — de status wordt veilig op de server bijgehouden en er **kan niet** worden geknoeid met gegevens uit eerdere stappen  
- [ ] Ja — validatie vindt plaats in de eerste stappen, maar de definitieve verzending verifieert de integriteit van de gegevens **niet** opnieuw  
- [ ] Ja — workflow bypass **is mogelijk** door stappen over te slaan of gegevens buiten de juiste volgorde in te dienen  

### Behandelt de applicatie "edge case"-gegevens die bedrijfsbeperkingen omzeilen op de juiste manier?
- [ ] Ja — beperkingen zijn robuust en behandelen ongebruikelijke maar syntactisch geldige invoer (bijv. schrikkeljaren, tijdzoneverschuivingen)  
- [ ] Ja — sommige edge cases worden afgehandeld, maar specifieke combinaties **kunnen** leiden tot onverwacht gedrag  
- [ ] Nee — edge cases leiden rechtstreeks tot logische fouten, zoals gratis aankopen of ongeoorloofde statuswijzigingen

---

## WSTG-BUSL-02 — Testen van de mogelijkheid om requests te vervalsen

Het vervalsen van requests binnen een business logic context omvat het manipuleren van door de applicatie gedefinieerde parameters om acties uit te voeren buiten de beoogde workflow of het autorisatieniveau. Aanvallers richten zich op processen die uit meerdere stappen bestaan, zoals checkout-systemen of accountregistratie, waarbij de status (state) wordt bijgehouden via client-side parameters die de server niet opnieuw valideert tegen een vertrouwde backend-bron. Door verborgen velden, JSON-waarden of REST-parameters zoals prijzen, hoeveelheden of interne statusvlaggen te wijzigen, kan een aanvaller betalingsvereisten omzeilen, privileges escaleren, of onbedoelde statuswijzigingen veroorzaken. Deze kwetsbaarheid komt doorgaans voor in stateful webformulieren of API's waarbij de server impliciet de representatie van de business state door de client vertrouwt tijdens een transactie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek* |

> *De ernst wordt Kritiek als het vervalste request leidt tot financieel verlies, ongeautoriseerde administratieve acties of volledige accountovername.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Valideert de applicatie transactionele parameters tegen een server-side "source of truth"?
- [ ] Ja — alle gevoelige parameters (prijs, rol, ID) worden gevalideerd tegen de database en een bypass is **niet mogelijk**  
- [ ] Ja — validatie wordt toegepast, maar kan worden omzeild via parameter pollution of alternatieve encoding  
- [ ] Nee — de applicatie vertrouwt op waarden aan de client-zijde om de kosten of de uitkomst van een transactie te bepalen *(Kritiek)*  

### Kunnen bedrijfskritische waarden (bijv. hoeveelheden, bedragen) worden aangepast naar ongeautoriseerde waarden?
- [ ] Nee — negatieve waarden, nulwaarden of buitengewoon hoge waarden worden door de server geweigerd  
- [ ] Ja — de server accepteert gewijzigde waarden, maar alleen binnen een beperkt bereik  
- [ ] Ja — negatieve of nulwaarden **kunnen** worden verzonden, wat leidt tot financiële bypasses of omzeiling van de logica  

### Worden verborgen velden of API-parameters gebruikt om de status (state) te behouden over processen met meerdere stappen?
- [ ] Nee — de status wordt veilig beheerd via server-side sessies of ondertekende tokens  
- [ ] Ja — de status wordt via de client doorgegeven, maar integriteitscontroles (HMAC/Signatures) zijn **ingeschakeld** en robuust  
- [ ] Ja — de status wordt via de client doorgegeven en integriteitscontroles zijn **uitgeschakeld** of kunnen worden verwijderd  

### Is het mogelijk om requests te vervalsen die verplichte tussenstappen in een workflow overslaan?
- [ ] Nee — de server dwingt een strikte volgorde van bewerkingen af voor elke transactie  
- [ ] Ja — stappen **kunnen** worden overgeslagen, maar de uiteindelijke actie vereist nog steeds geldige gegevens uit eerdere stappen  
- [ ] Ja — het uiteindelijke "submit" of "complete" request **kan** direct worden vervalst om autorisatie- of betalingsstappen te omzeilen

---

## WSTG-BUSL-03 — Testen van integriteitscontroles

Het testen van integriteitscontroles richt zich op het verifiëren of de applicatie waarborgt dat gegevens ongewijzigd en betrouwbaar blijven terwijl deze door verschillende bedrijfsprocessen bewegen of tussen de client en server worden uitgewisseld. Aanvallers richten zich op deze logica door kritieke parameters te manipuleren – zoals productprijzen, hoeveelheden, gebruikersrechten of transactiestatussen – binnen HTTP-verzoeken, verborgen velden of cookies. Als de applicatie server-side verificatie of robuuste integriteitscontroles zoals Hashing of HMACs mist, kan een tegenstander deze waarden manipuleren om fraude te plegen, eigen bevoegdheden te verhogen (Escalate), of beoogde bedrijfsbeperkingen te omzeilen. Deze test is essentieel voor elke applicatie die financiële transacties, gevoelige statusovergangen of workflows met meerdere stappen verwerkt waarbij de output van de ene fase dient als de vertrouwde input voor de volgende.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als het falen van de integriteit financiële diefstal, ongeautoriseerde Privilege Escalation of wijziging van persistente records mogelijk maakt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Worden gevoelige bedrijfsparameters blootgesteld aan manipulatie aan de client-zijde?
- [ ] Nee — gevoelige waarden worden uitsluitend aan de server-zijde beheerd  
- [ ] Ja — waarden zoals prijs, hoeveelheid of statusflags **worden** blootgesteld maar zijn versleuteld  
- [ ] Ja — waarden **worden** in platte tekst blootgesteld binnen verborgen velden, URL-parameters of cookies  

### Wordt er een integriteitsmechanisme (bijv. HMAC, Checksum) toegepast op door de client gecontroleerde parameters?
- [ ] Ja — een robuuste integriteitscontrole **wordt toegepast** en bij elk verzoek geverifieerd  
- [ ] Ja — een integriteitscontrole **wordt toegepast** maar gebruikt een zwak/voorspelbaar algoritme of een hardcoded geheim  
- [ ] Nee — er **worden geen** integriteitscontroles toegepast op gevoelige parameters  

### Kan de integriteitscontrole worden omzeild of opnieuw worden berekend door een aanvaller?
- [ ] Nee — verificatie van de handtekening wordt strikt afgedwongen en omzeiling is **niet mogelijk**  
- [ ] Ja — verificatie van de handtekening kan worden omzeild door de handtekeningparameter weg te laten  
- [ ] Ja — de handtekening **kan** opnieuw worden berekend omdat de geheime sleutel of logica is blootgesteld/zwak is  

### Voert de applicatie backend-revalidatie van gegevens uit tegen een vertrouwde bron?
- [ ] Ja — de server re-valideert alle waarden tegen de database en omzeiling is **niet mogelijk**  
- [ ] Ja — de server voert gedeeltelijke validatie uit maar omzeiling **is mogelijk** via specifieke randgevallen (edge cases)  
- [ ] Nee — de server vertrouwt door de client verstrekte gegevens zonder backend-verificatie *(Kritiek)*  

### Is het mogelijk om de status van een proces met meerdere stappen te manipuleren?
- [ ] Nee — de sessiestatus wordt server-side beheerd en kan niet worden gemanipuleerd  
- [ ] Ja — statusinformatie wordt via de client doorgegeven en manipulatie **is mogelijk** om stappen over te slaan of acties te herhalen

---

## WSTG-BUSL-04 — Testen op Proces-timing

Proces-timingaanvallen (Process timing attacks) omvatten het meten van de tijd die een applicatie nodig heeft om specifieke verzoeken te verwerken, om zo gevoelige informatie over interne toestanden of het bestaan van gegevens af te leiden. Door het analyseren van verschillen in responstijden op milliseconden-niveau — vaak veroorzaakt door early-exit conditionele logica, database lookups of cryptografische vergelijkingen die niet in constante tijd (non-constant-time) plaatsvinden — kunnen aanvallers een side-channel analyse uitvoeren om geldige gebruikersnamen te enumereren, wachtwoordfragmenten te verifiëren of het bestaan van specifieke records te bevestigen. Deze kwetsbaarheden komen doorgaans voor bij authenticatie-endpoints, wachtwoordherstelformulieren en resource-intensieve API-filters waar de backend-logica vertakt op basis van de geldigheid van de invoer. Vanuit het perspectief van een aanvaller dienen deze timingverschillen als een "boolean oracle" die feiten over de backend-gegevens onthult, zelfs wanneer de applicatie identieke, generieke foutmeldingen naar de gebruiker terugstuurt.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Tools:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### Vertoont de applicatie variërende responstijden voor geldige versus ongeldige resource-aanvragen?
- [ ] Nee — responstijden zijn identiek, ongeacht de geldigheid van de invoer  
- [ ] Ja — er bestaan verwaarloosbare timingverschillen, maar deze zijn **niet** statistisch significant  
- [ ] Ja — consistente timingverschillen zijn **waarneembaar** en bieden een betrouwbare oracle  

### Zijn er constant-time vergelijkingsfuncties of kunstmatige vertragingen geïmplementeerd om de responstijden te normaliseren?
- [ ] Ja — constant-time algoritmen zijn **toegepast** op alle gevoelige vergelijkingsoperaties *(Meest veilig)*  
- [ ] Ja — kunstmatige vertragingen of jitter zijn **ingeschakeld**, maar de onderliggende logica blijft non-constant-time  
- [ ] Nee — er zijn geen timing-mitigaties **toegepast**, waardoor directe meting van logische vertakkingen mogelijk is  

### Kan een aanvaller statistische analyse gebruiken om het timingsignaal te isoleren van netwerkruis?
- [ ] Nee — netwerk-jitter en applicatieruis maken timinganalyse **niet mogelijk**  
- [ ] Ja — met een voldoende grote steekproefomvang **kan** het timingsignaal worden geïsoleerd van de ruis  
- [ ] Ja — het timingverschil (delta) is groot genoeg zodat statistische normalisatie **niet** vereist is  

### Is account-enumeratie mogelijk via de login- of wachtwoordherstelfunctionaliteit?
- [ ] Nee — het bestaan van accounts kan niet worden vastgesteld via timing  
- [ ] Ja — timingverschillen stellen een externe aanvaller in staat om geldige gebruikersnamen of e-mailadressen te **enumereren**  

### Lekken resource-intensieve operaties (bijv. bestandsverwerking, complexe zoekopdrachten) het bestaan van gegevens via timing?
- [ ] Nee — resourceverbruik is uniform, ongeacht of een record wordt gevonden  
- [ ] Ja — het bestaan van gevoelige records **kan** worden afgeleid door de duur van de backend-verwerking te meten

---

## WSTG-BUSL-05 — Testen van limieten op het aantal keren dat een functie kan worden gebruikt

Deze test evalueert of een applicatie business logic restricties afdwingt op de frequentie en het totale aantal keren dat een specifieke actie of functie kan worden uitgevoerd door een enkele gebruiker, sessie of IP-adres. Het niet implementeren van deze limieten stelt aanvallers in staat om waardevolle functies te misbruiken—zoals het verzenden van SMS OTPs, het triggeren van e-mails voor het herstellen van wachtwoorden, het toepassen van kortingscodes of het uitvoeren van zware data-exports—wat kan leiden tot financieel verlies, uitputting van resources of reputatieschade. Deze kwetsbaarheden worden doorgaans gevonden in transactionele endpoints of communicatiefuncties en worden geëxploiteerd via geautomatiseerde scripts die de actie herhalen ver buiten de beoogde zakelijke drempels. Vanuit het perspectief van een aanvaller is dit een belangrijk doelwit voor SMS pumping, mail bombing of het brute-forcen van workflows die geen expliciete rate-limiting of op aantallen gebaseerde throttles hebben.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als de functie financiële transacties of SMS-kosten betreft, of de systeembrede beschikbaarheid beïnvloedt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### Definieert en handhaaft de applicatie limieten op gevoelige bedrijfsfuncties?
- [ ] Ja — limieten worden strikt gehandhaafd en bypass is **niet mogelijk**  
- [ ] Ja — limieten worden gehandhaafd maar zijn te hoog om misbruik te voorkomen  
- [ ] Nee — er worden geen limieten toegepast op de uitvoering van de functie  

### Worden rate limits en gebruikstellers server-side afgedwongen?
- [ ] Ja — handhaving is strikt server-side en **kan niet** worden gemanipuleerd  
- [ ] Nee — handhaving vertrouwt op client-side logica (bijv. JavaScript of verborgen velden)  
- [ ] Nee — er bestaat geen handhaving op welk niveau dan ook  

### Kunnen de gebruikslimieten worden omzeild via header- of sessiemanipulatie?
- [ ] Nee — bypass via `X-Forwarded-For`, `Origin`, of sessierotatie is **niet mogelijk**  
- [ ] Ja — limieten **kunnen** worden omzeild door het spoofen van IP-headers of het wissen van cookies  
- [ ] Ja — limieten **kunnen** worden omzeild door het aanmaken van meerdere accounts of sessies  

### Triggert het overschrijden van de limiet een passende defensieve reactie?
- [ ] Ja — applicatie retourneert `429 Too Many Requests` en logt de gebeurtenis *(Meest veilig)*  
- [ ] Ja — applicatie retourneert een foutmelding maar logt het potentiële misbruik **niet**  
- [ ] Nee — applicatie blijft verzoeken verwerken maar negeert de output  
- [ ] Nee — applicatie geeft geen reactie of crasht onder belasting  

### Is de impact van het misbruiken van de functie aanzienlijk voor de organisatie?
- [ ] Nee — misbruik van de functie heeft een verwaarloosbare kosten- of resource-impact  
- [ ] Ja — misbruik **kan** leiden tot gering resourceverbruik of irritatie bij gebruikers  
- [ ] Ja — misbruik **kan** leiden tot aanzienlijke financiële kosten (bijv. SMS-kosten) of denial of service *(Kritiek)*

---

## WSTG-BUSL-06 — Testen op het omzeilen van workflows

Workflow-omzeiling omvat het manipuleren van de logica van een applicatie om beoogde sequenties of voorwaarden binnen processen die uit meerdere stappen bestaan, te omzeilen. Aanvallers identificeren gevoelige endpoints—zoals betalingsbevestigingen, administratieve goedkeuringen of MFA-voltooiingsschermen—en proberen deze direct te benaderen, waarbij verplichte tussenstappen worden overgeslagen. Deze kwetsbaarheid doet zich voor wanneer de server-side logica er niet in slaagt een robuuste state machine te onderhouden en in plaats daarvan vertrouwt op de aanname dat een gebruiker het pad volgt dat door de UI wordt gedicteerd. Succesvolle exploitatie kan leiden tot kritieke fouten in de business logic, waaronder ongeautoriseerde transacties, privilege escalation en het omzeilen van mechanismen voor identiteitsverificatie.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als de omzeiling ongeautoriseerde financiële transacties, privilege escalation of administratieve toegang mogelijk maakt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Bevat de applicatie bedrijfsprocessen die uit meerdere fasen bestaan?
- [ ] Nee — de applicatie bestaat alleen uit acties met een enkel verzoek  
- [ ] Ja — processen met meerdere fasen (bijv. winkelwagentjes, wizards, registratie) **zijn** aanwezig  

### Wordt de volgorde van de stappen strikt afgedwongen door de server?
- [ ] Ja — server-side state tracking zorgt ervoor dat stappen **niet** kunnen worden overgeslagen  
- [ ] Ja — client-side controls suggereren een volgorde, maar de server valideert de voortgang **niet**  
- [ ] Nee — de applicatie dwingt **geen** specifieke volgorde van bewerkingen af  

### Kan een aanvaller de laatste fase van een proces bereiken door direct de terminale URL of het endpoint op te vragen?
- [ ] Nee — directe toegang tot terminale stappen is **niet mogelijk** zonder voorafgaande voltooiing  
- [ ] Ja — terminale stappen (bijv. `/api/v1/checkout/complete`) **kunnen** via een direct verzoek worden bereikt  

### Controleert de applicatie of de vereiste gegevens uit voorgaande stappen aanwezig en geldig zijn?
- [ ] Ja — de server valideert alle gegevens van voorgaande stappen voordat deze verder gaat  
- [ ] Nee — de server **is** kwetsbaar voor het "overslaan" van stappen waarbij gegevens worden verwacht maar niet worden geverifieerd  

### Kunnen beveiligingsmaatregelen zoals MFA of identiteitsverificatie worden omzeild door de workflow te manipuleren?
- [ ] Nee — kritieke controles kunnen **niet** worden omzeild via manipulatie van de workflow  
- [ ] Ja — het omzeilen van MFA of identiteitscontroles **is mogelijk** door naar de stap na de verificatie te springen

---

## WSTG-BUSL-07 — Testen van verdedigingsmechanismen tegen misbruik van de applicatie

Verdedigingsmechanismen tegen misbruik van applicaties evalueren het vermogen van de applicatie om afwijkend gebruikersgedrag te identificeren, te loggen en erop te reageren wanneer dit afwijkt van de beoogde business logic. In tegenstelling tot traditionele op signatures gebaseerde beveiliging, richten deze verdedigingen zich op het detecteren van patronen zoals snel opeenvolgende verzoeken, credential stuffing, of sequentiële enumeratie van resources die duiden op geautomatiseerd misbruik. Vanuit het perspectief van een aanvaller bepaalt deze test de weerbaarheid van de applicatie tegen automatisering en "low and slow"-aanvallen die zijn ontworpen om gegevens te exfiltreren of resources uit te putten zonder standaard beveiligingswaarschuwingen te activeren. Het niet implementeren van deze verdedigingsmechanismen resulteert vaak in grootschalige data scraping, account takeovers, of denial of service via legitieme maar resource-intensieve applicatiefuncties.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Worden rate-limiting of throttling mechanismen afgedwongen op gevoelige endpoints?
- [ ] Ja — strikte rate limiting **wordt toegepast** en afgedwongen op alle gevoelige endpoints *(Meest veilig)*  
- [ ] Ja — rate limiting **wordt toegepast** maar kan worden omzeild via IP-rotatie of header-manipulatie  
- [ ] Ja — rate limiting **wordt alleen toegepast** op authenticatie-endpoints, waardoor andere endpoints blootgesteld blijven  
- [ ] Nee — er **wordt geen** rate limiting of throttling toegepast op applicatiefuncties  

### Detecteert en blokkeert de applicatie geautomatiseerde interactiepatronen?
- [ ] Ja — gedragsanalyse of CAPTCHAs **zijn ingeschakeld** en blokkeren bots effectief  
- [ ] Ja — anti-automatisering **is ingeschakeld** maar omzeiling **is mogelijk** met behulp van headless browsers of sessie-cycling  
- [ ] Nee — de applicatie **kan geen** onderscheid maken tussen een menselijke gebruiker en een geautomatiseerd script  

### Wordt afwijkend gedrag gelogd en gevolgd door een defensieve reactie?
- [ ] Ja — misbruik triggert automatische blokkering en waarschuwt het security-team  
- [ ] Ja — misbruik wordt gelogd voor audit, maar er **wordt geen** automatische defensieve actie ondernomen  
- [ ] Nee — de applicatie **logt niet** en reageert niet op ongebruikelijke activiteitspatronen  

### Kunnen verdedigingsmechanismen worden omzeild door het manipuleren van client-side identifiers of headers?
- [ ] Nee — verdedigingsmechanismen vertrouwen op server-side status en geverifieerde telemetrie  
- [ ] Ja — controles **kunnen** worden omzeild door cookies te wissen of `User-Agent` strings te roteren  
- [ ] Ja — controles **kunnen** worden omzeild door het injecteren van headers zoals `X-Forwarded-For` of `X-Real-IP`

---

## WSTG-BUSL-08 — Testen van het uploaden van onverwachte bestandstypen

Het uploaden van onverwachte bestandstypen stelt aanvallers in staat om business logic-beperkingen te omzeilen door bestanden in te dienen die de applicatie niet bedoeld heeft te verwerken, wat mogelijk kan leiden tot Remote Code Execution (RCE), Cross-Site Scripting (XSS) of Denial of Service (DoS). Aanvallers onderzoeken deze kwetsbaarheden door bestandsextensies, MIME-typen en Magic Bytes aan te passen om de validatielogica aan de serverzijde te misleiden, zodat deze kwaadaardige inhoud accepteert. Dit doet zich doorgaans voor bij het uploaden van profielfoto's, documentbeheersystemen of bijlagen bij support-tickets waar onvoldoende validatie aanwezig is op de back-end. Succesvolle exploitatie kan de hostserver compromitteren als het geüploade bestand wordt uitgevoerd, of leiden tot client-side aanvallen als het bestand aan andere gebruikers wordt geserveerd met een onjuiste Content-Type.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst / Severity** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Beperkt de applicatie bestandsuploads tot een specifieke set extensies?
- [ ] Ja — een strikte whitelist van extensies wordt afgedwongen en bypass is **niet mogelijk**  
- [ ] Ja — een whitelist van extensies is aanwezig, maar een bypass **is mogelijk** via dubbele extensies of null bytes  
- [ ] Nee — elke bestandsextensie **kan** worden geüpload  

### Wordt de bestandsinhoud gevalideerd buiten de bestandsextensie?
- [ ] Ja — server-side logica verifieert Magic Bytes en voert diepgaande inhoudsinspectie uit  
- [ ] Ja — server-side logica verifieert het MIME-type uitsluitend via de `Content-Type` header  
- [ ] Nee — er **wordt geen** inhoudsvalidatie toegepast na de controle van de extensie  

### Kan een aanvaller code uitvoeren door een web shell of script te uploaden?
- [ ] Nee — geüploade bestanden worden opgeslagen in een niet-uitvoerbare directory of een storage bucket (S3/Azure Blob)  
- [ ] Ja — bestanden worden opgeslagen in een via het web toegankelijke directory, maar uitvoering **is uitgeschakeld** via serverconfiguratie  
- [ ] Ja — geüploade kwaadaardige scripts **kunnen** direct worden uitgevoerd via een voorspelbaar URL-pad *(Kritiek)*  

### Zijn client-side aanvallen zoals XSS mogelijk via geüploade bestandstypen?
- [ ] Nee — bestanden worden geserveerd met `Content-Disposition: attachment` en `X-Content-Type-Options: nosniff`  
- [ ] Ja — SVG- of HTML-bestanden **kunnen** worden geüpload en gerenderd in de browser, wat leidt tot XSS  
- [ ] Ja — metadata van afbeeldingen (EXIF) **wordt verwerkt** en weergegeven in de UI zonder opschoning (sanitization)

---

## WSTG-BUSL-09 — Test het uploaden van kwaadaardige bestanden

Functionaliteit voor het uploaden van bestanden stelt gebruikers in staat gegevens naar de server te verzenden, wat een direct vector vormt voor het introduceren van kwaadaardige inhoud in de omgeving van de applicatie. Aanvallers maken misbruik van zwakke validatielogica om web shells, malware of speciaal samengestelde bestanden — zoals SVG of HTML — te uploaden om Remote Code Execution (RCE) of Cross-Site Scripting (XSS) te bewerkstelligen. Deze kwetsbaarheid wordt doorgaans aangetroffen in functies zoals profielinstellingen, documentbeheersystemen en bijlagenverwerkers waar de server nalaat om bestandstypen, inhoud en uitvoeringsrechten correct te verifiëren. Succesvolle exploitatie leidt vaak tot volledige systeemcompromis, data-exfiltratie of het gebruik van de server als pivot point voor aanvallen op het interne netwerk.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### Biedt de applicatie functionaliteit om bestanden te uploaden?
- [ ] Nee — er bestaat geen functionaliteit voor het uploaden van bestanden  
- [ ] Ja — functionaliteit bestaat voor geauthenticeerde gebruikers  
- [ ] Ja — functionaliteit is beschikbaar voor **niet-geauthenticeerde** gebruikers *(Kritiek)*  

### Wordt validatie van bestandsextensies afgedwongen aan de serverzijde?
- [ ] Ja — een strikte allowlist van extensies wordt **toegepast** en bypass is **niet mogelijk**  
- [ ] Ja — een allowlist wordt gebruikt, maar bypass **is mogelijk** via hoofdlettergevoeligheid of dubbele extensies  
- [ ] Ja — er wordt een blocklist gebruikt, die **kan** worden omzeild met alternatieve extensies (bijv. `.phtml`, `.asa`)  
- [ ] Nee — er wordt geen extensievalidatie **toegepast** aan de serverzijde  

### Wordt de bestandsinhoud gevalideerd buiten de extensie of het MIME-type?
- [ ] Ja — de applicatie verifieert magic bytes en voert diepgaande inhoudsinspectie uit  
- [ ] Ja — de applicatie controleert alleen de `Content-Type` header, die eenvoudig kan worden gespooft  
- [ ] Nee — de applicatie vertrouwt volledig op de bestandsextensie voor validatie  

### Worden geüploade bestanden opgeslagen in een directory met uitvoeringsrechten?
- [ ] Nee — bestanden worden opgeslagen op een speciale opslagservice (bijv. S3) of in een niet-uitvoerbare directory  
- [ ] Ja — bestanden worden opgeslagen op de webserver, maar uitvoering is **uitgeschakeld** via configuratie  
- [ ] Ja — bestanden worden opgeslagen in een via het web toegankelijke directory en uitvoering **is ingeschakeld** *(Kritiek)*  

### Kunnen beveiligingsfilters worden omzeild via bestandsnaammanipulatie?
- [ ] Nee — bestandsnamen worden geschoond of willekeurig opnieuw gegenereerd door de server  
- [ ] Ja — filters **kunnen** worden omzeild met null bytes (bijv. `shell.php%00.jpg`)  
- [ ] Ja — path traversal karakters (bijv. `../../`) **kunnen** worden gebruikt om bestanden naar willekeurige directories te uploaden

---

## WSTG-BUSL-10 — Testen van de Betalingsfunctionaliteit

Het testen van de betalingsfunctionaliteit omvat het identificeren van Business Logic fouten die een aanvaler in staat stellen om payment gateways te omzeilen, ordertotalen te manipuleren of succesvolle transactiesignalen te spoofen. Deze kwetsbaarheden bevinden zich doorgaans in de communicatie tussen de applicatie, de client-browser en externe payment processors zoals Stripe, PayPal of interne grootboeksystemen. Een aanvaler kan proberen om prijsinstellingen tijdens het transport te wijzigen, negatieve hoeveelheden in te dienen om de totale kosten te verlagen, of succesvolle transactie-callbacks te replayen om bestellingen te voltooien zonder daadwerkelijke betaling. Succesvolle exploitatie leidt tot direct financieel verlies, voorraadverschillen en een mogelijke aantasting van de e-commerce-integriteit van de applicatie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Wordt het transactiebedrag aan de serverzijde gevalideerd voorafgaand aan de verwerking?
- [ ] Ja — bedrag wordt strikt gevalideerd tegen de productdatabase aan de serverzijde *(Meest veilig)*  
- [ ] Ja — bedrag wordt gevalideerd, maar een logic bypass **is mogelijk** via parameter pollution of het wisselen van valuta  
- [ ] Nee — het transactiebedrag **kan** worden gewijzigd in het request aan de clientzijde en wordt geaccepteerd door de backend  

### Kan het aantal artikelen worden gemanipuleerd, resulterend in een totaal van nul of een negatief bedrag?
- [ ] Nee — negatieve of nul-hoeveelheden worden afgewezen door de validatielogica van de applicatie  
- [ ] Ja — negatieve hoeveelheden worden geaccepteerd in de winkelwagen, maar het totaal wordt gecorrigeerd bij het afrekenen  
- [ ] Ja — negatieve hoeveelheden **kunnen** worden gebruikt om het totale orderbedrag te verlagen of een tegoed te verkrijgen *(Kritiek)*  

### Verwerkt de applicatie payment gateway callbacks of webhooks op een veilige manier?
- [ ] Ja — callbacks vereisen een geldige cryptografische handtekening die **niet** gespoofd kan worden  
- [ ] Ja — handtekeningen worden gecontroleerd, maar het geheim is zwak of de verificatie **kan** worden omzeild  
- [ ] Nee — de applicatie vertrouwt op niet-geauthenticeerde of niet-geverifieerde callbacks om de betalingsstatus te bevestigen  

### Is de applicatie kwetsbaar voor race conditions tijdens de afreken- of betalingsfase?
- [ ] Nee — transactionele integriteit en database-locking-mechanismen zijn **ingeschakeld**  
- [ ] Ja — race conditions **zijn mogelijk**, maar hebben geen invloed op de uiteindelijke prijs of levering  
- [ ] Ja — gelijktijdige requests **kunnen** leiden tot meerdere leveringen voor een enkele betaling of het "double-spending" van cadeaubonnen  

### Zijn kortingscodes of loyaliteitspunten onderhevig aan manipulatie of hergebruik?
- [ ] Nee — codes worden gevalideerd voor eenmalig gebruik en logische beperkingen worden **toegepast**  
- [ ] Ja — codes kunnen worden hergebruikt of toegepast op niet-in aanmerking komende artikelen, maar de impact is beperkt  
- [ ] Ja — aanvallers **kunnen** meerdere conflicterende codes toepassen of de vervaldata en gebruiksbeperkingen omzeilen

---

## WSTG-CLNT-01 — Testing for DOM Based Cross Site Scripting

DOM-based Cross-Site Scripting (DOM XSS) treedt op wanneer een applicatie client-side JavaScript bevat die gegevens van een onbetrouwbare bron op een onveilige manier verwerkt, meestal door de gegevens terug te schrijven naar het Document Object Model (DOM) via een gevaarlijke sink. In tegenstelling tot reflected of stored XSS, bevindt de kwetsbaarheid zich volledig in de client-side code; de payload wordt vaak helemaal niet naar de server verzonden, omdat deze wordt verwerkt door sinks zoals `innerHTML`, `eval()`, of `document.write()`. Aanvallers misbruiken dit door URL's op te stellen met kwaadaardige fragmenten of parameters die, wanneer ze door de browser van een slachtoffer worden geladen, willekeurige JavaScript uitvoeren in de context van de gebruikerssessie. Dit kan leiden tot de exfiltratie van sessietokens, diefstal van gevoelige gegevens en ongeautoriseerde acties uitgevoerd namens de geauthenticeerde gebruiker.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Worden onbetrouwbare bronnen gebruikt om door de gebruiker gecontroleerde gegevens vast te leggen in JavaScript?
- [ ] Nee — JavaScript gebruikt **geen** `location.hash`, `location.search`, of `document.referrer`  
- [ ] Ja — onbetrouwbare bronnen worden gebruikt, maar gegevens worden **niet** doorgegeven aan execution sinks  
- [ ] Ja — onbetrouwbare bronnen worden gebruikt en gegevens vloeien rechtstreeks in de client-side logica  

### Worden door de gebruiker gecontroleerde gegevens doorgegeven aan gevaarlijke DOM sinks?
- [ ] Nee — sinks zoals `innerHTML`, `outerHTML`, `document.write()`, of `eval()` worden **niet** gebruikt  
- [ ] Ja — gevaarlijke sinks zijn aanwezig, maar de gegevens worden strikt **gesaneerd** met een veilige bibliotheek zoals DOMPurify  
- [ ] Ja — gevaarlijke sinks zijn aanwezig en gegevens worden verwerkt met **onvoldoende** of aangepaste op regex gebaseerde filtering  
- [ ] Ja — gevaarlijke sinks zijn aanwezig en gegevens worden doorgegeven **zonder** enige sanering of encoding *(Kritiek)*  

### Kan de execution sink worden bereikt via een op URL gebaseerde payload?
- [ ] Nee — source-to-sink flow kan **niet** worden geactiveerd via externe invoer  
- [ ] Ja — flow is **mogelijk**, maar vereist complexe gebruikersinteractie  
- [ ] Ja — flow is **mogelijk** en kan worden geactiveerd via een eenvoudig URL-fragment of queryparameter  

### Neutraliseren moderne security headers of op de browser gebaseerde mitigaties het risico?
- [ ] Ja — een strikt `Content-Security-Policy` (CSP) met `script-src` en `trusted-types` is **ingeschakeld** en voorkomt uitvoering  
- [ ] Ja — er is een CSP aanwezig, maar `unsafe-inline` of zwakke hashes maken een bypass **mogelijk**  
- [ ] Nee — er is geen CSP aanwezig om op DOM gebaseerde uitvoering te beperken  

### Maakt de applicatie gebruik van client-side frameworks met ingebouwde beveiligingen?
- [ ] Ja — framework (bijv. Angular, React) codeert gegevens automatisch en een bypass is **niet mogelijk**  
- [ ] Ja — framework wordt gebruikt, maar "escape hatches" zoals `dangerouslySetInnerHTML` zijn **ingeschakeld**  
- [ ] Nee — de applicatie gebruikt vanilla JavaScript of legacy bibliotheken (bijv. oude jQuery-versies) zonder automatische escaping

---

## WSTG-CLNT-02 — Testen op JavaScript-uitvoering

Het testen van JavaScript-uitvoering richt zich op het identificeren van instanties waarbij een applicatie onjuist omgaat met door de gebruiker verstrekte gegevens, wat leidt tot de uitvoering van ongeautoriseerde scripts binnen de browsercontext van het slachtoffer. Deze kwetsbaarheid resulteert doorgaans in Cross-Site Scripting (XSS), waarmee aanvallers sessiecookies kunnen exfiltreren, het Document Object Model (DOM) kunnen manipuleren of acties kunnen uitvoeren namens de gebruiker. Penetratietesters onderzoeken sinks zoals `innerHTML`, `document.write()` en `eval()` waar ongezuiverde gegevens uit bronnen zoals URL-fragmenten, `localStorage` of `window.name` kunnen worden verwerkt. Succesvolle exploitatie brengt de integriteit en vertrouwelijkheid van de sessie van de gebruiker in gevaar en kan dienen als een pivot voor complexere aanvallen aan de clientzijde.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Tools:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### Verwerkt de applicatie door de gebruiker verstrekte gegevens in gevaarlijke JavaScript-sinks?
- [ ] Nee — alle gegevens zijn gesaneerd of worden gebruikt in veilige sinks zoals `textContent`  
- [ ] Ja — gegevens worden verwerkt in gevaarlijke sinks, maar sanering/codering **wordt toegepast**  
- [ ] Ja — gegevens worden verwerkt in gevaarlijke sinks en sanering **wordt niet toegepast**  

### Is er een Content Security Policy (CSP) aanwezig om scriptuitvoering te beperken?
- [ ] Ja — een restrictieve CSP is **ingeschakeld** en bypass **is niet mogelijk**  
- [ ] Ja — een CSP is **ingeschakeld**, maar bypass **is mogelijk** via `unsafe-inline` of onveilige CDN's  
- [ ] Nee — er is geen CSP **ingeschakeld**  

### Kan JavaScript worden uitgevoerd via URL-parameters of fragmenten (DOM-based XSS)?
- [ ] Nee — invoer is correct gecodeerd en uitvoering **is niet mogelijk**  
- [ ] Ja — invoer wordt gereflecteerd in de DOM, maar uitvoering **wordt voorkomen** door beveiligingen van moderne frameworks  
- [ ] Ja — invoer wordt direct uitgevoerd in de browser en exploitatie **is mogelijk**  

### Zijn event handlers of URI-schema's kwetsbaar voor script-injectie?
- [ ] Nee — invoer is gesaneerd om event-attributen en `javascript:`-schema's te verwijderen  
- [ ] Ja — event handlers **kunnen** worden geïnjecteerd, maar filters beperken de complexiteit van de payload  
- [ ] Ja — willekeurige JavaScript-uitvoering via `onerror`, `onload` of `href`-attributen **is mogelijk**

---

## WSTG-CLNT-03 — HTML Injection

HTML Injection vindt plaats wanneer een applicatie er niet in slaagt om door de gebruiker geleverde invoer te saniteren voordat deze in de browser wordt gerenderd, waardoor een aanvaller willekeurige HTML-tags in het Document Object Model (DOM) van de webpagina kan injecteren. Hoewel het vergelijkbaar is met Cross-Site Scripting (XSS), is het primaire doel van HTML Injection het manipuleren van de visuele weergave van de pagina of het faciliteren van phishing-aanvallen door het injecteren van kwaadaardige formulieren en misleidende inhoud. Aanvallers richten zich op parameters die worden gereflecteerd in de UI, zoals zoektermen, gebruikersprofieldelen of foutmeldingen, om slachtoffers te misleiden tot het onthullen van gevoelige inloggegevens of interactie met ongeautoriseerde links van derden. Deze kwetsbaarheid vormt een aanzienlijk risico voor de integriteit van de gebruikersinterface van de applicatie en kan een voorloper zijn van complexere social engineering- of sessiegerelateerde aanvallen.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Laag / Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Worden door de gebruiker gecontroleerde invoervelden in de DOM gereflecteerd zonder de juiste HTML-encoding?
- [ ] Nee — alle gereflecteerde invoer is strikt HTML-encoded voordat deze wordt gerenderd  
- [ ] Ja — sommige tekens **zijn** ge-encoded, maar bypasses voor bepaalde tags **zijn mogelijk**  
- [ ] Ja — invoer wordt onbewerkt gereflecteerd en HTML injection **is mogelijk**  

### Kunnen structurele HTML-elementen worden geïnjecteerd om de lay-out van de pagina te wijzigen?
- [ ] Nee — de applicatie filtert of verwijdert structurele tags zoals `<div>`, `<table>`, of `<iframe>`  
- [ ] Ja — structurele elementen **kunnen** worden geïnjecteerd, maar hebben **geen** significante impact op de UI  
- [ ] Ja — aanzienlijke UI-vervorming (defacement) **is mogelijk** via geïnjecteerde tags  

### Is het mogelijk om functionele phishing-elementen zoals formulieren of kwaadaardige links te injecteren?
- [ ] Nee — `<form>`, `<input>`, en `<a>` tags worden effectief geblokkeerd of gesaniteerd  
- [ ] Ja — kwaadaardige links **kunnen** worden geïnjecteerd om gebruikers naar externe sites om te leiden  
- [ ] Ja — functionele `<form>` elementen **kunnen** worden geïnjecteerd om inloggegevens te verzamelen *(Medium)*  

### Beperken beveiligingsheaders of Content Security Policies (CSP) de impact van de injectie?
- [ ] Ja — er is een beperkende CSP aanwezig en `form-action` of `frame-src` **is toegepast**  
- [ ] Nee — beveiligingsheaders zijn aanwezig, maar de CSP **is uitgeschakeld** of bevat unsafe-inline/wildcards  
- [ ] Nee — er zijn geen CSP of relevante beveiligingsheaders **ingeschakeld**

---

## WSTG-CLNT-04 — Testen op Client-Side URL Redirect

Client-side URL redirection treedt op wanneer een webapplicatie door de gebruiker gecontroleerde invoer accepteert—meestal via URL-parameters of hash-fragmenten—en deze gebruikt binnen een client-side redirection sink zoals `window.location` of `document.location.href`. Deze kwetsbaarheid wordt voornamelijk door aanvallers misbruikt om geavanceerde phishingcampagnes uit te voeren, aangezien het slachtoffer in eerste instantie het legitieme domein vertrouwt voordat deze ongemerkt wordt doorgeleid naar een kwaadaardige site. Naast phishing kunnen client-side redirects vaak worden gekoppeld (chained) om gevoelige gegevens te exfiltreren, zoals OAuth access tokens of sessie-identifiers, indien de omleiding plaatsvindt terwijl deze waarden aanwezig zijn in de URL of de `Referer` header. In moderne Single Page Applications (SPA's) komt deze kwetsbaarheid vaak voor in de routing-logica, "return to"-parameters na authenticatie of functionaliteiten voor het wisselen van taal.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Medium* |

> *De ernst (Severity) wordt Hoog als de redirect kan worden gebruikt om OAuth tokens of sessie-ID's te exfiltreren, of om beveiligingscontroles in een authenticatieflow te omzeilen.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Maakt de applicatie gebruik van client-side scripts om redirects te verwerken op basis van gebruikersinvoer?
- [ ] Nee — redirection wordt uitsluitend aan de serverzijde afgehandeld via `3xx` HTTP-statuscodes  
- [ ] Ja — de applicatie gebruikt JavaScript sinks zoals `window.location` om te navigeren op basis van URL-parameters  
- [ ] Ja — de applicatie maakt gebruik van een client-side framework-router (bijv. React Router, Vue Router) die door de gebruiker opgegeven paden verwerkt  

### Zijn er validatie- of whitelisting-controles geïmplementeerd voor de bestemmings-URL?
- [ ] Ja — een strikte **whitelist** van toegestane domeinen/paden wordt afgedwongen en een bypass is **niet mogelijk**  
- [ ] Ja — validatie is aanwezig maar vertrouwt op zwakke regex of blacklists, en een bypass is **mogelijk**  
- [ ] Nee — de applicatie accepteert elke willekeurige string als redirection-doel  

### Kan de redirection-logica worden omzeild met gangbare obfuscatietechnieken?
- [ ] Nee — controles verwerken correct gecodeerde tekens, protocol-relatieve URL's (`//`) en backslashes  
- [ ] Ja — bypass is **mogelijk** via URL-encoding of double-encoding  
- [ ] Ja — bypass is **mogelijk** via protocol-relatieve URL's of `@`-tekens (bijv. `https://expected.com@attacker.com`)  
- [ ] Ja — bypass is **mogelijk** via specifieke browser quirks of null-byte-injectie  

### Stelt het redirection-proces gevoelige gegevens bloot aan de bestemmingssite?
- [ ] Nee — er zijn geen gevoelige gegevens aanwezig in de URL of verzonden via headers tijdens de redirect  
- [ ] Ja — gevoelige gegevens (bijv. sessietokens, API-sleutels) **worden gelekt** naar de externe site via de `Referer` header  
- [ ] Ja — gevoelige gegevens in het URL-fragment (`#`) zijn **toegankelijk** voor de scripts van de bestemmingssite  

### Is het mogelijk om JavaScript uit te voeren via de redirect-sink (DOM-based XSS)?
- [ ] Nee — de sink staat alleen navigatie naar `http`- of `https`-protocollen toe  
- [ ] Ja — de redirect-sink staat `javascript:` of `data:` URI's toe, waardoor DOM-based XSS **mogelijk** is

---

## WSTG-CLNT-05 — Testen op CSS Injection

CSS Injection treedt op wanneer een applicatie toestaat dat door de gebruiker verstrekte invoer de Cascading Style Sheets (CSS) van een webpagina beïnvloedt zonder de juiste sanitization of escaping. Hoewel het vaak wordt gezien als een cosmetisch probleem, kunnen aanvallers CSS injection misbruiken om gevoelige gegevens, zoals CSRF-tokens of sessie-ID's, te exfiltreren door gebruik te maken van attribute selectors en background-image eigenschappen om externe verzoeken te triggeren. Deze kwetsbaarheid doet zich doorgaans voor bij eindpunten waar gebruikersvoorkeuren (bijv. aangepaste thema's, lettertypekleuren) worden gereflecteerd binnen `<style>` blokken of inline `style` attributen. Vanuit het perspectief van een aanvaller kan een succesvolle exploitatie leiden tot UI redressing, phishing via ongeautoriseerde wijziging van de lay-out, of heimelijke data-exfiltratie in omgevingen waar een strikt Content Security Policy (CSP) anders op JavaScript gebaseerde aanvallen zou blokkeren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als het injectiepunt de exfiltratie van gevoelige attributen zoals CSRF-tokens toestaat of als het kan worden gebruikt voor UI redressing in gevoelige workflows.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Reflecteert de applicatie door de gebruiker gecontroleerde invoer binnen CSS-contexten?
- [ ] Nee — invoer wordt **niet** gereflecteerd in style-tags, attributen of CSS-bestanden  
- [ ] Ja — invoer wordt gereflecteerd maar is strikt beperkt tot veilige alfanumerieke waarden  
- [ ] Ja — invoer wordt gereflecteerd binnen een `style` attribuut of `<style>` blok  

### Worden CSS-specifieke metatekens en functies correct gesaneerd?
- [ ] Ja — tekens zoals `{`, `}`, `:` en functies zoals `url()` worden **correct ge-escaped**  
- [ ] Ja — sanitization is aanwezig, maar een bypass **is mogelijk** via encoding of CSS-comments  
- [ ] Nee — er wordt geen sanitization toegepast op CSS-metatekens  

### Is data-exfiltratie mogelijk met behulp van CSS-selectors?
- [ ] Nee — selectors **kunnen geen** externe verzoeken triggeren of gegevens lekken  
- [ ] Ja — gedeeltelijke data-exfiltratie **is mogelijk** via attribute selectors en `background-image`  
- [ ] Ja — volledige exfiltratie van gevoelige tokens **is mogelijk** met behulp van geautomatiseerde brute-force technieken  

### Beperkt een Content Security Policy (CSP) het risico op CSS injection?
- [ ] Ja — `style-src` is beperkt tot 'self' en `img-src` of `connect-src` **is beperkt**  
- [ ] Ja — CSP is aanwezig maar gebruikt 'unsafe-inline' of staat wildcard externe domeinen toe  
- [ ] Nee — er is geen CSP geïmplementeerd om het laden van externe bronnen via CSS te voorkomen  

### Kan de kwetsbaarheid worden gebruikt voor UI redressing of phishing?
- [ ] Nee — de reikwijdte van de injectie is te beperkt om de lay-out significant te wijzigen  
- [ ] Ja — wijziging van de UI **is mogelijk**, wat het overlayeren van kwaadaardige elementen toestaat  
- [ ] Ja — volledige controle over de pagina-lay-out **is mogelijk**, wat zeer overtuigende phishing-aanvallen vergemakkelijkt

---

## WSTG-CLNT-06 — Testen op Client-Side Resource Manipulation

Client-side resource manipulation vindt plaats wanneer een applicatie toestaat dat door de gebruiker gecontroleerde invoer de bron of het pad van door de browser geladen bronnen beïnvloedt, zoals scripts, stylesheets of iframes. Door URI fragments, query parameters of andere DOM sources te manipuleren, kan een aanvaller de applicatie dwingen om kwaadaardige externe inhoud te laden of ongeautoriseerde code uit te voeren. Deze kwetsbaarheid wordt doorgaans aangetroffen in JavaScript-logica die dynamisch de `src`- of `href`-attributen van HTML-elementen vult zonder voldoende validatie tegen een vertrouwde allow-list. Vanuit het perspectief van een aanvaller wordt dit misbruikt door een URL op te stellen die het laden van bronnen omleidt naar een door de aanvaller beheerde server, wat kan leiden tot DOM-based Cross-Site Scripting (XSS), exfiltratie van gevoelige gegevens of UI redressing.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### Maakt de applicatie gebruik van client-side sinks om bronnen dynamisch te laden of in te sluiten?
- [ ] Nee — bronnen worden uitsluitend geladen via statische HTML of server-side logica  
- [ ] Ja — client-side JavaScript vult dynamisch attributen in zoals `src`, `href` of `data`  

### Zijn er validatiecontroles of allow-lists geïmplementeerd voor URI-bronnen?
- [ ] Ja — strikte allow-lists en origin-validatie **worden toegepast** op alle dynamische bronnen  
- [ ] Ja — validatie is aanwezig, maar vertrouwt op zwakke blacklists of eenvoudig te omzeilen regex  
- [ ] Nee — er worden geen validatiecontroles toegepast op door de gebruiker gecontroleerde paden van bronnen  

### Is het mogelijk om URI-validatie te omzeilen met behulp van alternatieve protocollen of codering?
- [ ] Nee — het omzeilen van filters voor bronnen is **niet mogelijk**  
- [ ] Ja — omzeiling **is mogelijk** met behulp van verschillende protocollen zoals `data:`, `vbscript:` of `javascript:`  
- [ ] Ja — omzeiling **is mogelijk** door gebruik te maken van gecodeerde karakters of null bytes om eenvoudige string-matches te omzeilen  

### Kan een aanvaller het laden van bronnen omleiden naar een extern, niet-vertrouwd domein?
- [ ] Nee — paden van bronnen zijn beperkt tot dezelfde origin of een vaste submap  
- [ ] Ja — de applicatie **kan** worden gedwongen om scripts of stijlen te laden vanaf een willekeurig extern domein  

### Wat is de maximale impact van de resource manipulation?
- [ ] Laag — manipulatie heeft alleen invloed op niet-uitvoerbare elementen zoals afbeeldingen of niet-gevoelige tekst  
- [ ] Gemiddeld — manipulatie maakt CSS-injectie of aanzienlijke UI redressing mogelijk  
- [ ] Hoog — manipulatie leidt tot DOM-based XSS of de uitvoering van willekeurige JavaScript *(Kritiek)*

---

## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) is een browsergebaseerd mechanisme dat een server toestaat om expliciet toegang tot zijn resources te verlenen vanuit verschillende origins, waardoor de Same-Origin Policy (SOP) effectief wordt versoepeld. Misconfiguraties treden op wanneer een applicatie willekeurige origins te veel vertrouwt, vaak door de `Origin` header te reflecteren in de `Access-Control-Allow-Origin` response header of door slecht geïmplementeerde regex whitelists te gebruiken. Vanuit het perspectief van een aanvaller kan een laks CORS-beleid worden misbruikt door een kwaadaardig script te hosten op een gecontroleerd domein dat geauthenticeerde verzoeken doet naar de kwetsbare applicatie, waardoor de aanvaller gevoelige gegevens, zoals CSRF tokens of PII, rechtstreeks uit de response body kan exfiltreren. Deze kwetsbaarheid is het meest kritiek op API-endpoints die geauthenticeerde sessies verwerken en niet-publieke informatie retourneren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Tools:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Implementeert de applicatie CORS-headers op geauthenticeerde endpoints?
- [ ] Nee — CORS is **niet ingeschakeld**; browser valt terug op de strikte Same-Origin Policy  
- [ ] Ja — CORS-headers zijn aanwezig en beperkt tot een strikte **whitelist**  
- [ ] Ja — CORS-headers zijn aanwezig maar **permissief** geconfigureerd  

### Hoe verwerkt de server de `Access-Control-Allow-Origin` (ACAO) header?
- [ ] ACAO is ingesteld op een specifiek, **vertrouwd** statisch domein  
- [ ] ACAO is ingesteld op `*` (wildcard) en credentials **kunnen niet** worden verzonden  
- [ ] ACAO is ingesteld op `*` (wildcard) en credentials **kunnen** worden verzonden *(Opmerking: De meeste browsers blokkeren dit)*  
- [ ] ACAO **reflecteert dynamisch** de waarde van de `Origin` header uit het verzoek  

### Is de `Access-Control-Allow-Credentials` (ACAC) header ingesteld op `true`?
- [ ] Nee — ACAC is **niet aanwezig** of ingesteld op `false`  
- [ ] Ja — ACAC is ingesteld op `true`, maar ACAO is **beperkt** tot vertrouwde origins  
- [ ] Ja — ACAC is ingesteld op `true` en ACAO **reflecteert** willekeurige origins *(Kritiek)*  

### Kan de origin-whitelist worden omzeild via subdomein- of null origin-manipulatie?
- [ ] Nee — whitelist wordt strikt afgedwongen en **kan niet** worden omzeild  
- [ ] Ja — whitelist accepteert **elk** subdomein van het doel (bijv. `attacker.target.com`)  
- [ ] Ja — whitelist accepteert de `null` origin via sandboxed iframes  
- [ ] Ja — whitelist is kwetsbaar voor regex bypasses (bijv. `target.com.attacker.com` of `target.com-safe.com`)  

### Staat de CORS-configuratie de exfiltratie van gevoelige gegevens toe?
- [ ] Nee — blootgestelde endpoints bevatten **geen** gevoelige of sessiespecifieke gegevens  
- [ ] Ja — geauthenticeerde endpoints retourneren PII, credentials of CSRF tokens die **kunnen** worden gelezen door een ongeautoriseerde origin

---

## WSTG-CLNT-08 — Testen op Cross Site Flashing

Cross-Site Flashing (XSF) is een kwetsbaarheid aan de clientzijde die optreedt wanneer een Flash-bestand (SWF) onjuist omgaat met door de gebruiker gecontroleerde invoer, waardoor een aanvaller kwaadaardige ActionScript-code kan uitvoeren of een verbinding kan maken met de JavaScript-omgeving van de browser. Door parameters zoals `FlashVars` of URL-querystrings te manipuleren die naar sinks zoals `ExternalInterface.call`, `getURL` of `loadMovie` worden gestuurd, kan een aanvaller acties uitvoeren in de context van het kwetsbare domein, waaronder session hijacking en data exfiltratie. Deze kwetsbaarheid wordt voornamelijk aangetroffen in legacy bedrijfsapplicaties die nog steeds SWF-bestanden hosten, waar onvoldoende invoervalidatie het mogelijk maakt dat de Flash-movie wordt hergebruikt als een vector voor Cross-Site Scripting (XSS) of om de Same-Origin Policy (SOP) te omzeilen via permissieve cross-domain configuraties.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Worden legacy Adobe Flash (.swf) bestanden gehost op de applicatie?
- [ ] Nee — er worden geen Flash-bestanden gehost of naar verwezen binnen de scope van de applicatie  
- [ ] Ja — er zijn SWF-bestanden aanwezig, maar deze bevatten alleen statische inhoud  
- [ ] Ja — er zijn SWF-bestanden aanwezig en deze accepteren dynamische parameters via `FlashVars` of URL-strings  

### Wordt invoer die naar ActionScript-sinks wordt gestuurd gesaneerd?
- [ ] Ja — alle invoer wordt strikt gevalideerd en ActionScript-sinks **kunnen niet** worden gemanipuleerd  
- [ ] Ja — validatie wordt toegepast, maar een bypass **is mogelijk** via specifieke encoding-technieken  
- [ ] Nee — invoer wordt direct doorgegeven aan gevoelige sinks zoals `ExternalInterface.call` of `getURL` *(Kritiek)*  

### Kan het Flash-bestand worden gebruikt om willekeurige JavaScript uit te voeren (XSS)?
- [ ] Nee — `ExternalInterface` is **uitgeschakeld** of de parameter `allowScriptAccess` is ingesteld op `never`  
- [ ] Ja — `allowScriptAccess` is ingesteld op `sameDomain` maar de SWF wordt gehost op het doeldomein  
- [ ] Ja — `allowScriptAccess` is ingesteld op `always`, wat XSS vanaf elk domein toestaat  

### Voorkomt het `crossdomain.xml` beleid ongeautoriseerde cross-origin verzoeken?
- [ ] Ja — `crossdomain.xml` is restrictief en staat alleen vertrouwde, specifieke origins toe  
- [ ] Nee — het `crossdomain.xml` bestand **ontbreekt**  
- [ ] Ja — het beleid is te permissief (bijv. `<allow-access-from domain="*" />`)  

### Kan het SWF-bestand worden gedwongen om externe, door de aanvaller gecontroleerde movies te laden?
- [ ] Nee — de applicatie **kan niet** worden gedwongen om externe SWF-bestanden te laden  
- [ ] Ja — de functies `loadMovie` of `loadMovieNum` accepteren niet-gevalideerde externe URL's

---

## WSTG-CLNT-09 — Testen op Clickjacking

Clickjacking, ook wel bekend als UI redressing, vindt plaats wanneer een aanvaller transparante of ondoorzichtige lagen gebruikt om een gebruiker te misleiden om op een knop of link op een andere pagina te klikken, terwijl de gebruiker de intentie had om op de bovenliggende pagina te klikken. Deze kwetsbaarheid tast de integriteit van gebruikersacties aan, wat kan leiden tot ongeautoriseerde gegevenswijziging, accountaanpassingen of financiële overschrijvingen die namens het slachtoffer worden uitgevoerd. Aanvallers exploiteren dit meestal door de doelwebsite in te sluiten in een `<iframe>` op een gecontroleerde kwaadaardige site en deze te overlappen met misleidende inhoud of verleidelijke lokmiddelen. Het komt het vaakst voor op pagina's die gevoelige, statuswijzigende acties uitvoeren, zoals "Account verwijderen"-knoppen, formulieren voor het opnieuw instellen van wachtwoorden of administratieve configuratiepanelen.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Medium / Hoog* |

> *De ernst wordt Hoog als gevoelige acties (bijv. geldovermakingen, accountverwijdering of privilege-escalatie) kunnen worden getriggerd via clickjacking.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Tools:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Implementeert de applicatie server-side headers om ongeautoriseerde framing te voorkomen?
- [ ] Ja — `X-Frame-Options` of `Content-Security-Policy` **zijn toegepast** en voorkomen framing *(Meest veilig)*  
- [ ] Ja — headers zijn aanwezig maar **verkeerd geconfigureerd** (bijv. `X-Frame-Options: ALLOW-FROM` die brede browserondersteuning mist)  
- [ ] Nee — er zijn geen headers voor framing-beveiliging **toegepast**  

### Is de `Content-Security-Policy` (CSP) `frame-ancestors` richtlijn geïmplementeerd?
- [ ] Ja — `frame-ancestors 'none'` of `'self'` **is toegepast**  
- [ ] Ja — `frame-ancestors` is aanwezig maar **staat** onbetrouwbare of te brede origins toe  
- [ ] Nee — richtlijn **ontbreekt** of CSP is niet geïmplementeerd  

### Is de `X-Frame-Options` (XFO) header correct geconfigureerd voor ondersteuning van legacy browsers?
- [ ] Ja — `DENY` of `SAMEORIGIN` **is toegepast**  
- [ ] Ja — XFO is aanwezig maar wordt door moderne browsers **genegeerd** omdat er een CSP `frame-ancestors` richtlijn bestaat  
- [ ] Nee — XFO-header **ontbreekt** of is ingesteld op een onveilige waarde  

### Kunnen de framing-beveiligingen worden omzeild met bekende technieken?
- [ ] Nee — bypass **niet mogelijk** via standaardtechnieken (double-framing, etc.)  
- [ ] Ja — beveiliging **kan** worden omzeild vanwege zwakke regex of logica in de `frame-ancestors` whitelist  
- [ ] Ja — beveiliging **kan** worden omzeild omdat de applicatie uitsluitend vertrouwt op JavaScript-gebaseerde frame-busting  

### Is een gevoelige actie exploiteerbaar via een clickjacking proof-of-concept?
- [ ] Nee — framing wordt geblokkeerd of er zijn geen gevoelige acties bereikbaar  
- [ ] Ja — UI redressing **is mogelijk** maar vereist complexe gebruikersinteractie  
- [ ] Ja — UI redressing **is mogelijk** en staat onmiddellijke statuswijzigende acties toe *(Kritiek)*

---

## WSTG-CLNT-10 — WebSockets Testen

WebSocket-testen richt zich op het full-duplex communicatiekanaal dat tot stand is gebracht tussen de client en de server, dat de traditionele HTTP-request-response-patronen omzeilt. Pentesters evalueren de beveiliging van het handshake-proces, de aanwezigheid van Cross-Site WebSocket Hijacking (CSWSH)-beveiligingen en de stritheid van de input-validatie die wordt toegepast op message payloads. Exploitatie omvat doorgaans het overnemen (hijacking) van een actieve sessie om gegevens in real-time te exfiltreren of het injecteren van kwaadaardige payloads die server-side kwetsbaarheden zoals Command Injection of SQLi triggeren via de persistente socket. Aangezien veel traditionele Web Application Firewalls (WAFs) WebSocket-verkeer niet effectief inspecteren, biedt dit protocol vaak een onopvallende vector voor het omzeilen van perimeterbeveiligingen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Teststatus** | Niet Uitgevoerd |
| **Severity** | Hoog / Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Tools:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### Wordt de WebSocket-verbinding tot stand gebracht via een beveiligd kanaal?
- [ ] Ja — `wss://` (WebSocket Secure) wordt afgedwongen voor alle verbindingen  
- [ ] Nee — `ws://` wordt gebruikt, wat person-in-the-middle (PITM)-afluisteren mogelijk maakt  

### Is de applicatie beschermd tegen Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] Nee — de server valideert de `Origin`-header strikt tijdens de handshake  
- [ ] Ja — `Origin`-header-validatie is zwak of **kan** worden omzeild via null of gespoofte origins  
- [ ] Ja — er wordt geen `Origin`-header-validatie uitgevoerd, waardoor cross-site aanvallers een verbinding kunnen initiëren *(Kritiek)*  

### Wordt input-validatie en sanitization toegepast op WebSocket message payloads?
- [ ] Ja — alle inkomende berichten worden gesaneerd en gevalideerd tegen een schema  
- [ ] Ja — er bestaat enige validatie, maar omzeiling **is mogelijk** met behulp van gecodeerde of niet-standaard payloads  
- [ ] Nee — berichten worden direct verwerkt, wat injectie (XSS, SQLi, RCE) of logische manipulatie mogelijk maakt  

### Worden authenticatie- en autorisatiecontroles uitgevoerd op elk WebSocket-bericht?
- [ ] Ja — de sessie wordt voor elk bericht geverifieerd of het protocol is stateless met tokens  
- [ ] Ja — authenticatie vindt plaats bij de handshake, maar autorisatie voor specifieke acties **wordt niet** per bericht afgedwongen  
- [ ] Nee — authenticatie wordt alleen gecontroleerd bij de handshake, waardoor session hijacking volledige toegang kan verlenen  

### Implementeert de server rate limiting of resource-beperkingen op de WebSocket?
- [ ] Ja — rate limiting en maximale berichtgroottes zijn **ingeschakeld** en worden afgedwongen  
- [ ] Nee — er bestaan geen limieten, wat Denial of Service (DoS) mogelijk maakt via message flooding of grote payloads

---

## WSTG-CLNT-11 — Test Web Messaging

Web Messaging, of `postMessage`, maakt cross-origin communicatie mogelijk tussen window-objecten, zoals een parent-pagina en een geopende popup of een ingebedde iframe. Dit mechanisme is cruciaal voor moderne webfunctionaliteit, maar brengt aanzienlijke risico's met zich mee als de ontvangende applicatie de identiteit van de afzender niet verifieert of de message payload onjuist verwerkt. Aanvallers maken misbruik van deze kwetsbaarheden door een kwaadaardige site te hosten die de doelapplicatie insluit en gemanipuleerde berichten verstuurt om onbedoelde acties uit te voeren, gevoelige gegevens te exfiltreren of DOM-based Cross-Site Scripting (XSS) te veroorzaken. Succesvolle exploitatie vindt plaats wanneer message listeners de `origin`-eigenschap niet strikt valideren of `message.data` rechtstreeks doorgeven aan gevaarlijke execution sinks.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Medium / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### Maakt de applicatie gebruik van de `postMessage` API voor cross-document communicatie?
- [ ] Nee — `postMessage` listeners zijn **niet** aanwezig in de client-side code  
- [ ] Ja — `postMessage` wordt gebruikt voor interne componentcommunicatie  
- [ ] Ja — `postMessage` wordt gebruikt om te communiceren met externe domeinen of widgets  

### Wordt de origin van inkomende berichten strikt gevalideerd?
- [ ] Ja — origin wordt gecontroleerd tegen een strikte whitelist met behulp van `===` of een identieke vergelijking *(Meest veilig)*  
- [ ] Ja — origin wordt gevalideerd met een regex, maar de logica **is** vatbaar voor een bypass (bijv. `^https://trusted.com`)  
- [ ] Nee — de `origin`-eigenschap wordt **niet** gecontroleerd, waardoor elk domein berichten kan verzenden  
- [ ] Nee — er wordt een wildcard `*` gebruikt in de `targetOrigin` van de afzender, wat mogelijk gegevens lekt naar kwaadaardige listeners  

### Wordt de message payload gesaneerd voordat deze door de applicatie wordt verwerkt?
- [ ] Ja — data wordt behandeld als platte tekst en er worden geen gevaarlijke sinks gebruikt  
- [ ] Ja — data wordt geparsed (bijv. `JSON.parse`) en gevalideerd voor gebruik, en een bypass is **niet mogelijk**  
- [ ] Nee — message data wordt rechtstreeks doorgegeven aan gevaarlijke sinks zoals `innerHTML`, `eval()` of `setTimeout()`  
- [ ] Nee — message data wordt gebruikt om het venster te navigeren of de `location.href` te wijzigen zonder validatie  

### Kan een aanvaller ongeoorloofde acties triggeren of gegevens exfiltreren via een kwaadaardig bericht?
- [ ] Nee — zelfs met een gespoofd bericht kunnen geen gevoelige acties of gegevens worden benaderd  
- [ ] Ja — een gemanipuleerd bericht **kan** gevoelige statuswijzigingen triggeren (bijv. wachtwoordwijziging, profielupdate)  
- [ ] Ja — een gemanipuleerd bericht **kan** leiden tot DOM-based XSS of exfiltratie van CSRF-tokens/sessiegegevens

---

## WSTG-CLNT-12 — Test Browser Storage

Het testen van browseropslag omvat het analyseren van de wijze waarop een applicatie gebruikmaakt van client-side mechanismen zoals LocalStorage, SessionStorage, IndexedDB en WebSQL om gegevens te behouden. Onveilig opgeslagen gevoelige informatie — inclusief PII, authentication tokens of session identifiers — kan worden gecompromitteerd als een aanvaller Cross-Site Scripting (XSS) uitvoert of fysieke toegang verkrijgt tot het apparaat van de gebruiker. Pentesters moeten vaststellen of gegevens in cleartext worden opgeslagen, of ze toegankelijk blijven na het uitloggen en of de opslagcyclus op de juiste manier wordt beheerd om datalekken te voorkomen. Vanuit het perspectief van een aanvaller zijn deze opslagmechanismen lucratieve doelen voor het exfiltreren van statusinformatie waarmee session hijacking of langdurige account-persistentie mogelijk wordt.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als session tokens of gevoelige PII worden opgeslagen in LocalStorage en toegankelijk zijn via XSS.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### Slaat de applicatie gevoelige informatie op in de browseropslag?
- [ ] Nee — de applicatie gebruikt geen browseropslag of slaat alleen niet-gevoelige UI-status op  
- [ ] Ja — gevoelige gegevens worden opgeslagen maar zijn versleuteld met robuuste client-side cryptografie  
- [ ] Ja — gevoelige gegevens (tokens, PII, secrets) worden opgeslagen in **cleartext** *(Gemiddeld)*  

### Zijn de opgeslagen gegevens toegankelijk voor niet-geautoriseerde scripts (XSS-risico)?
- [ ] Nee — gegevens worden opgeslagen in HttpOnly cookies (niet in browseropslag) of opslag wordt niet gebruikt  
- [ ] Ja — LocalStorage/SessionStorage wordt gebruikt, waardoor gegevens **volledig toegankelijk** zijn via elke XSS Payload *(Hoog)*  

### Wordt de browseropslag op de juiste manier gewist bij het uitloggen van de gebruiker?
- [ ] Ja — alle applicatiegerelateerde opslag wordt **expliciet gewist** tijdens het uitlogproces  
- [ ] Nee — opslag blijft behouden na uitloggen, maar bevat geen gevoelige sessiegegevens  
- [ ] Nee — authentication tokens of PII **blijven aanwezig** in de opslag nadat de sessie is beëindigd *(Gemiddeld)*  

### Gebruikt de applicatie IndexedDB of WebSQL voor grote/gevoelige datasets?
- [ ] Nee — deze opslagmechanismen worden niet gebruikt  
- [ ] Ja — gegevens worden opgeslagen en **correct gesaneerd** voordat ze in de DOM worden gerenderd  
- [ ] Ja — gevoelige datasets worden in **cleartext** opgeslagen binnen IndexedDB/WebSQL-structuren  

### Kan de integriteit van de opgeslagen gegevens worden gemanipuleerd om de applicatielogica te beïnvloeden?
- [ ] Nee — de applicatie valideert of ondertekent gegevens voordat ze vanuit de opslag worden verwerkt  
- [ ] Ja — het wijzigen van opslagwaarden (bijv. gebruikersrollen, flags) **is mogelijk** en resulteert in gewijzigd gedrag van de applicatie

---

## WSTG-CLNT-13 — Testen op Cross Site Script Inclusion

Cross-Site Script Inclusion (XSSI) treedt op wanneer een webapplicatie gevoelige gegevens exporteert in een dynamisch gegenereerd JavaScript-bestand of JSONP-respons, die een door een aanval gecontroleerde site vervolgens kan insluiten via een `<script>`-tag. Omdat script-insluiting de Same-Origin Policy (SOP) omzeilt, kan een aanvaller het script in zijn eigen context uitvoeren en globale objecten of variabelen overschrijven om de gevoelige informatie te exfiltreren. Deze kwetsbaarheid manifesteert zich doorgaans in geauthenticeerde eindpunten die gebruikersspecifieke gegevens zoals e-mailadressen, API-sleutels of sessiemetadata in scriptformaat retourneren. Vanuit het perspectief van een aanvaller omvat exploitatie het opstellen van een kwaadaardige pagina die naar het kwetsbare script verwijst en de resulterende wijzigingen in globale variabelen of functieaanroepen onderschept.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Gemiddeld / Hoog |

> *De Severity wordt Hoog als de gelekte gegevens CSRF-tokens, sessie-identifiers of zeer gevoelige PII bevatten.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Retourneren geauthenticeerde eindpunten dynamische JavaScript of JSONP die gevoelige informatie bevat?
- [ ] Nee — alle scripts zijn statisch en **kunnen geen** gebruikersspecifieke gegevens bevatten  
- [ ] Ja — er bestaan dynamische scripts, maar deze **bevatten geen** gevoelige informatie  
- [ ] Ja — dynamische scripts bevatten PII of tokens en **zijn** toegankelijk via verschillende origins  

### Is de `X-Content-Type-Options: nosniff`-header geïmplementeerd op gevoelige script-responses?
- [ ] Ja — de header is **toegepast** en voorkomt MIME-type sniffing  
- [ ] Nee — de header **ontbreekt**, wat mogelijk toestaat dat niet-scriptinhoud als script wordt uitgevoerd  

### Worden gevoelige scripts beschermd door anti-XSSI mechanismen zoals niet-uitvoerbare prefixes of aangepaste headers?
- [ ] Ja — scripts vereisen een aangepaste header of token die **niet** via een standaard `<script>`-tag kan worden verzonden  
- [ ] Ja — scripts maken gebruik van niet-uitvoerbare prefixes (bijv. `)]}'`) die **niet** eenvoudig kunnen worden omzeild  
- [ ] Nee — scripts zijn direct toegankelijk via GET-verzoeken met gebruik van ambient credentials *(Kritiek)*  

### Is exfiltratie van gevoelige gegevens mogelijk door globale variabelen of prototypes te overschrijven?
- [ ] Nee — gevoelige gegevens zijn lokaal gescoped of beschermd via moderne browserfuncties  
- [ ] Ja — gevoelige gegevens zijn toegewezen aan globale variabelen en **kunnen** worden onderschept  
- [ ] Ja — gegevens worden gelekt via JSONP-callbacks die **kunnen** worden onderschept

---

## WSTG-CLNT-14 — Testen op Reverse Tabnabbing

Reverse Tabnabbing is een client-side kwetsbaarheid waarbij een pagina die wordt geopend via `target="_blank"` een functionele referentie naar de bovenliggende pagina behoudt via het `window.opener` object. Dit stelt een door de aanvaller gecontroleerde bestemmingssite in staat om het originele, vertrouwde tabblad programmatisch om te leiden naar een kwaadaardige URL, meestal een phishing-pagina die is ontworpen om inloggegevens te stelen. De aanval maakt misbruik van het inherente vertrouwen van de gebruiker in het oorspronkelijke tabblad, aangezien het onwaarschijnlijk is dat zij merken dat de pagina die ze zojuist hebben verlaten, naar een ander domein is genavigeerd. Moderne beveiligingspraktijken vereisen het gebruik van de attributen `rel="noopener"` of `rel="noreferrer"` op anchor-tags, of het instellen van de `opener`-eigenschap op null in JavaScript, om deze communicatie tussen vensters te voorkomen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld* |

> *De ernst is Laag in de meeste moderne browsers, aangezien deze `target="_blank"` nu standaard instellen op `rel="noopener"`. De ernst wordt Gemiddeld als de applicatie zich richt op verouderde browsers of `window.open()` gebruikt zonder de eigenschap `opener` op null in te stellen.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Zijn er links aanwezig die in een nieuw venster of tabblad openen?
- [ ] Nee — er worden geen links met `target="_blank"` of gelijkwaardige op JavaScript gebaseerde omleidingen gebruikt  
- [ ] Ja — `target="_blank"` wordt uitsluitend gebruikt op interne, vertrouwde links  
- [ ] Ja — `target="_blank"` wordt gebruikt op externe links of door de gebruiker opgegeven URL's  

### Is de `window.opener`-relatie beperkt op anchor-tags?
- [ ] Ja — `rel="noopener"` of `rel="noreferrer"` wordt **consequent toegepast** op alle links met `target="_blank"` *(Meest veilig)*  
- [ ] Ja — attributen worden toegepast, maar **ontbreken** op risicovolle of door gebruikers gegenereerde links  
- [ ] Nee — attributen worden **niet toegepast**, waardoor de onderliggende pagina toegang heeft tot `window.opener`  

### Is de applicatie kwetsbaar voor Reverse Tabnabbing via JavaScript `window.open()`?
- [ ] Nee — `window.open()` wordt niet gebruikt of stelt expliciet de eigenschap `opener` in op null  
- [ ] Ja — `window.open()` wordt gebruikt en de `opener`-referentie **blijft toegankelijk** voor het onderliggende venster  

### Kan een aanvaller de bestemming bepalen van een link die in een nieuw tabblad opent?
- [ ] Nee — alle linkbestemmingen zijn hardcoded en verwijzen naar vertrouwde domeinen  
- [ ] Ja — linkbestemmingen worden door de gebruiker opgegeven (bijv. social media-profielen, bio-links) en zijn **niet** geschoond voor tabnabbing-beveiligingen

---

## WSTG-APIT-01 — API Reconnaissance

API Reconnaissance is het systematische proces van het identificeren en in kaart brengen van het API-aanvalsoppervlak om endpoints, ondersteunde methoden en onderliggende datastructuren te onthullen. Aanvallers voeren deze fase uit om ongedocumenteerde (shadow) API's, verouderde versies met legacy-kwetsbaarheden en blootgestelde documentatiebestanden zoals Swagger- of OpenAPI-definities die interne bedrijfslogica onthullen, te ontdekken. Door het analyseren van voorspelbare URL-patronen, client-side JavaScript-bestanden en resultaten van directory brute-force, kan een tegenstander een uitgebreide kaart van de API opbouwen om doelwitten voor Broken Object Level Authorization (BOLA) of Mass Assignment-aanvallen te identificeren. Deze verkenning richt zich doorgaans op algemene paden zoals `/api/v1/`, `/swagger.json` of `/graphql` om een roadmap te krijgen van de backend-functionaliteit van de applicatie.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Informatief / Laag |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Tools:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Zijn API-documentatiebestanden (Swagger, OpenAPI, WSDL) publiek toegankelijk?
- [ ] Nee — API-documentatie is **niet** toegankelijk of is beperkt door authenticatie  
- [ ] Ja — documentatie is toegankelijk maar vereist geldige inloggegevens  
- [ ] Ja — API-documentatie (bijv. `/swagger-ui.html`) is **publiek toegankelijk** zonder authenticatie  

### Kunnen ongedocumenteerde of "shadow" API-endpoints worden ontdekt via fuzzing?
- [ ] Nee — fuzzing van directories en endpoints leverde geen ongedocumenteerde paden op  
- [ ] Ja — ongedocumenteerde endpoints zijn gevonden maar kunnen **niet** worden geopend zonder autorisatie  
- [ ] Ja — ongedocumenteerde endpoints zijn gevonden en **kunnen** worden geopend zonder geldige autorisatie  

### Onthult de applicatie meerdere API-versies (bijv. /v1/, /v2/, /beta/)?
- [ ] Nee — alleen de huidige, geharde API-versie is toegankelijk  
- [ ] Ja — legacy-versies bestaan maar implementeren **dezelfde** beveiligingscontroles als de huidige versie  
- [ ] Ja — legacy-versies (bijv. `/v1/`) zijn toegankelijk en **missen** de beveiligingscontroles van nieuwere versies  

### Lekken client-side bronnen (JavaScript/mobiele apps) API-endpointstructuren of sleutels?
- [ ] Nee — client-side code bevat **geen** hardcoded API-paden of gevoelige sleutels  
- [ ] Ja — client-side code bevat API-endpoint mappings maar **geen** gevoelige sleutels  
- [ ] Ja — API-sleutels, secrets of interne endpoint-URL's **zijn** hardcoded in client-side bronnen  

### Zijn verborgen parameters of headers te ontdekken op bekende endpoints?
- [ ] Nee — parameter fuzzing (bijv. via `Arjun`) heeft **geen** verborgen invoer onthuld  
- [ ] Ja — verborgen parameters (bijv. `debug=true`, `admin=1`) zijn ontdekt maar zijn **niet** functioneel  
- [ ] Ja — verborgen parameters zijn ontdekt en **kunnen** worden gebruikt om het gedrag van de applicatie te wijzigen

---

## WSTG-APIT-02 — API Broken Object Level Authorization

Broken Object Level Authorization (BOLA), ook wel bekend als Insecure Direct Object Reference (IDOR), treedt op wanneer een API nalaat te valideren of een gebruiker de juiste machtigingen heeft om toegang te krijgen tot of controle uit te oefenen over een specifieke resource die wordt geïdentificeerd door een ID. Aanvallers maken hier misbruik van door systematisch identifiers te enumereren of te raden—zoals numerieke ID's of UUIDs—in request paths, query parameters of JSON bodies om toegang te krijgen tot gegevens van andere gebruikers. Deze kwetsbaarheid is het meest voorkomende en impactvolle probleem in moderne API-beveiliging en kan leiden tot massale data exfiltration, ongeautoriseerde wijzigingen of volledige account takeover. Vanuit het perspectief van een aanvaller is het doel om endpoints te identificeren die object-identifiers accepteren en te testen of de server-side autorisatielogica ontbreekt of kan worden omzeild door die ID's te manipuleren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Tools:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Zijn object-identifiers voorspelbaar of opsombaar binnen API-requests?
- [ ] Nee — identifiers zijn lang, willekeurig en cryptografisch veilig (bijv. UUIDv4)  
- [ ] Ja — identifiers zijn sequentiële gehele getallen (bijv. `101`, `102`)  
- [ ] Ja — identifiers volgen een voorspelbaar patroon of zijn afgeleid van openbare informatie  

### Voert de API server-side validatie uit op objecteigendom voor elk request?
- [ ] Ja — autorisatiecontroles worden toegepast voor elk request en bypass is **niet mogelijk**  
- [ ] Ja — controles worden toegepast maar een bypass **is mogelijk** via parameter pollution of method tunneling  
- [ ] Nee — de applicatie vertrouwt uitsluitend op het feit dat de gebruiker geauthenticeerd is zonder het eigendom van de specifieke resource te controleren  

### Is het mogelijk om toegang te krijgen tot de resources van een andere gebruiker of deze te wijzigen door de identifier te veranderen?
- [ ] Nee — ongeautoriseerde toegang tot resources van andere gebruikers is **niet mogelijk**  
- [ ] Ja — ongeautoriseerde **lees-toegang** (Horizontale BOLA) **is mogelijk**  
- [ ] Ja — ongeautoriseerde **wijziging** of **verwijdering** (Horizontale BOLA) **is mogelijk**  

### Staat de API toegang tot administratieve of systeem-objecten toe door ID's te vervangen?
- [ ] Nee — administratieve resources worden beschermd door secundaire autorisatielagen  
- [ ] Ja — toegang krijgen tot of het wijzigen van resources op systeemniveau (Verticale BOLA) **is mogelijk**  

### Kan de autorisatiecontrole worden omzeild door de identifier naar een ander deel van het request te verplaatsen?
- [ ] Nee — de autorisatielogica is consistent, ongeacht de locatie van de parameter  
- [ ] Ja — bypass **is mogelijk** wanneer de ID wordt verplaatst van het URL-pad naar de JSON body of headers  
- [ ] Ja — bypass **is mogelijk** wanneer er meerdere ID's worden opgegeven en de server de ongeautoriseerde ID verwerkt

---

## WSTG-APIT-99 — Testen van GraphQL

GraphQL is een querytaal voor API's waarmee clients specifieke datastructuren kunnen opvragen, maar misconfiguraties leiden vaak tot aanzienlijke beveiligingsrisico's, waaronder informatieonthulling en resource exhaustion. Kwetsbaarheden ontstaan doorgaans door ingeschakelde introspectie, een gebrek aan limieten voor querydiepte/complexiteit en broken object-level authorization (BOLA) binnen de resolver-functies. Aanvallers misbruiken deze endpoints door introspectie te gebruiken om het volledige schema in kaart te brengen, gebruik te maken van circulaire fragmenten om een Denial of Service (DoS) te veroorzaken, of autorisatie te omzeilen door ongeautoriseerde velden te benaderen via geneste queries. Het testen richt zich op de `/graphql`, `/v1/graphql` of `/graphiql` endpoints om te waarborgen dat de implementatie strikte toegangscontroles en resource management afdwingt.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek* |

> *De ernst wordt Kritiek als broken object-level authorization (BOLA) ongeautoriseerde toegang tot PII of administratieve mutaties mogelijk maakt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Tools:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### Is het GraphQL-introspectiesysteem ingeschakeld?
- [ ] Nee — introspectie is **uitgeschakeld** en het schema kan niet in kaart worden gebracht  
- [ ] Ja — introspectie is **ingeschakeld** maar vereist administratieve authenticatie  
- [ ] Ja — introspectie is **ingeschakeld** en publiekelijk toegankelijk *(Hoog)*  

### Worden er resourcelimieten (diepte en complexiteit) afgedwongen op queries?
- [ ] Ja — strikte limieten voor querydiepte en complexiteit zijn **toegepast** en worden afgedwongen  
- [ ] Ja — limieten zijn **toegepast** maar kunnen worden omzeild met aliassen of fragmenten  
- [ ] Nee — er zijn geen limieten **toegepast**, wat recursieve queries en DoS mogelijk maakt  

### Is Broken Object-Level Authorization (BOLA) aanwezig in resolvers?
- [ ] Nee — autorisatie wordt voor elke query **toegepast** op veld- en objectniveau  
- [ ] Ja — autorisatie is **toegepast** op sommige velden, maar gevoelige objecten zijn blootgesteld  
- [ ] Ja — autorisatie is **niet toegepast**, waardoor toegang tot elk object mogelijk is via ID-manipulatie  

### Zijn GraphQL-mutaties correct beveiligd tegen ongeautoriseerd gebruik?
- [ ] Nee — mutaties zijn **niet** aanwezig in het schema  
- [ ] Ja — mutaties vereisen geldige authenticatie en strikte autorisatiecontroles  
- [ ] Ja — mutaties zijn **mogelijk** voor niet-geauthenticeerde gebruikers of missen autorisatie  

### Lekken foutmeldingen gevoelige implementatie- of schemadetails?
- [ ] Nee — foutmeldingen zijn generiek en onthullen **geen** interne logica  
- [ ] Ja — foutmeldingen onthullen onderliggende databasetypen of suggesties via "Did you mean...?"  
- [ ] Ja — volledige stack traces en gevoelige configuratiegegevens worden **onthuld** in antwoorden