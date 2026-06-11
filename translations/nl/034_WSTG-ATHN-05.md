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