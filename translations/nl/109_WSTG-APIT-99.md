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

---