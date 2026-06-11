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