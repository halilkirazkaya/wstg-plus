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