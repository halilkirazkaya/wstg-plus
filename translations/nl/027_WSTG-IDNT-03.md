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