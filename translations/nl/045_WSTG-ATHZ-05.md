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