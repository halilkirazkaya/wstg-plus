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