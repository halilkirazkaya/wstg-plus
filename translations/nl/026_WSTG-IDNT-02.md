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