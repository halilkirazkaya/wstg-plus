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