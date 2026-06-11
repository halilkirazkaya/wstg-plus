## WSTG-ATHN-09 — Testen op Zwakke Functionaliteiten voor Wachtwoordwijziging of -herstel

Mechanismen voor het wijzigen en herstellen van wachtwoorden zijn cruciale onderdelen van authenticatiebeheer die, indien onjuist geïmplementeerd, aanvallers in staat stellen gebruikersaccounts over te nemen. Deze kwetsbaarheden omvatten doorgaans voorspelbare herstel-tokens, het ontbreken van accountblokkades tijdens brute-force pogingen, onveilige verzending van tokens, of de mogelijkheid om wachtwoorden te wijzigen zonder het huidige wachtwoord te verifiëren. Aanvallers misbruiken deze gebreken door herstellinks te onderscheppen of te raden, Host Header Injection uit te voeren om herstel-e-mails om te leiden, of session fixation te gebruiken om toegang te behouden na een wachtwoordwijziging. Het waarborgen dat deze processen sterke verificatie vereisen en gebruikmaken van cryptografische willekeurigheid is essentieel voor het behoud van accountintegriteit en het voorkomen van ongeautoriseerde toegang.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Is het huidige wachtwoord vereist om een nieuw wachtwoord in te stellen tijdens een standaard wijziging?
- [ ] Ja — het huidige wachtwoord **is vereist** en wordt geverifieerd  
- [ ] Ja — het huidige wachtwoord **is vereist** maar kan worden omzeild via parameter pollution of manipulatie  
- [ ] Nee — het huidige wachtwoord **wordt niet** gevraagd, wat accountovername via session hijacking mogelijk maakt *(Hoog)*  

### Zijn wachtwoordherstel-tokens cryptografisch veilig en onvoorspelbaar?
- [ ] Ja — tokens hebben een hoge entropie, zijn willekeurig en **niet voorspelbaar**  
- [ ] Ja — tokens worden gegenereerd maar vertonen een lage entropie of voorspelbare patronen (bijv. op basis van tijdstempel)  
- [ ] Nee — tokens zijn **voorspelbaar** of gebaseerd op statische gebruikersgegevens zoals Base64-gecodeerde e-mail/ID *(Kritiek)*  

### Kan de wachtwoordherstellink worden omgeleid via Host Header Injection?
- [ ] Nee — de applicatie negeert of valideert de `Host`-header voor het genereren van links  
- [ ] Ja — de applicatie gebruikt de `Host`- of `X-Forwarded-Host`-header om links op te bouwen, maar validatie **is aanwezig**  
- [ ] Ja — links **kunnen** worden omgeleid naar een door de aanvaller beheerd domein via header-manipulatie *(Hoog)*  

### Heeft de herstel-token een beperkte levensduur en onmiddellijke ongeldigverklaring?
- [ ] Ja — tokens verlopen snel en worden **onmiddellijk** ongeldig verklaard na gebruik of wachtwoordwijziging  
- [ ] Ja — tokens verlopen na een lange tijdsduur (bijv. 24+ uur) of staan **meervoudig gebruik** toe  
- [ ] Nee — tokens **verlopen nooit** of blijven geldig nadat het wachtwoord succesvol is gewijzigd  

### Zijn er rate limits of blokkades op de eindpunten voor wachtwoordherstel en -wijziging?
- [ ] Ja — strikte rate limiting en CAPTCHA's **worden toegepast** om enumeratie en brute-force te voorkomen  
- [ ] Ja — er bestaan beperkte controles, maar omzeiling **is mogelijk** via IP-rotatie of header-spoofing  
- [ ] Nee — er wordt geen rate limiting **toegepast**, wat bulk account-enumeratie of token brute-forcing mogelijk maakt  

---