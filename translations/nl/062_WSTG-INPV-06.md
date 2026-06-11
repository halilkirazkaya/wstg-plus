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