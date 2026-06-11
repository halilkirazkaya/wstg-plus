## WSTG-BUSL-01 — Testen van Validatie van Bedrijfslogica-gegevens

Het testen van validatie van bedrijfslogica-gegevens richt zich op het vermogen van de applicatie om gegevens te identificeren en te weigeren die syntactisch correct zijn, maar logisch ongeldig binnen de context van het bedrijfsdomein. Aanvallers maken misbruik van deze gebreken door onverwachte waarden in te dienen—zoals negatieve hoeveelheden in een winkelmandje, datums in het verleden voor toekomstige evenementen, of extreem grote gehele getallen—om beperkingen te omzeilen en de status van de applicatie te manipuleren. Deze kwetsbaarheden bevinden zich vaak in workflows met meerdere stappen, afrekenprocessen en accountbeheermodules waar de server-side logica nalaat de contextuele integriteit van door de gebruiker verstrekte gegevens te verifiëren. Succesvolle exploitatie kan leiden tot financieel verlies, aantasting van de integriteit en ongeoorloofde toegang tot beperkte functies of gegevens.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Dwingt de applicatie logische bereiklimieten af op numerieke invoer?
- [ ] Ja — de applicatie weigert correct negatieve, nul- of excessief grote waarden waar dit ongepast is *(Meest veilig)*  
- [ ] Ja — de applicatie weigert sommige ongeldige bereiken maar **is** kwetsbaar voor integer overflow of boundary bypass  
- [ ] Nee — de applicatie accepteert elke numerieke waarde ongeacht de zakelijke context *(Kritiek)*  

### Zijn server-side validatiecontroles consistent in alle lagen van de applicatie?
- [ ] Ja — validatie wordt consistent toegepast op zowel de API/service- als de databaselagen  
- [ ] Ja — validatie **wordt toegepast** in de UI/Frontend, maar bypass via een direct API-verzoek **is mogelijk**  
- [ ] Nee — validatie wordt **niet toegepast** of is inconsistent, wat leidt tot logische gegevenscorruptie  

### Kan de bedrijfslogica worden ondermijnd door gegevens in workflows met meerdere stappen te manipuleren?
- [ ] Nee — de status wordt veilig op de server bijgehouden en er **kan niet** worden geknoeid met gegevens uit eerdere stappen  
- [ ] Ja — validatie vindt plaats in de eerste stappen, maar de definitieve verzending verifieert de integriteit van de gegevens **niet** opnieuw  
- [ ] Ja — workflow bypass **is mogelijk** door stappen over te slaan of gegevens buiten de juiste volgorde in te dienen  

### Behandelt de applicatie "edge case"-gegevens die bedrijfsbeperkingen omzeilen op de juiste manier?
- [ ] Ja — beperkingen zijn robuust en behandelen ongebruikelijke maar syntactisch geldige invoer (bijv. schrikkeljaren, tijdzoneverschuivingen)  
- [ ] Ja — sommige edge cases worden afgehandeld, maar specifieke combinaties **kunnen** leiden tot onverwacht gedrag  
- [ ] Nee — edge cases leiden rechtstreeks tot logische fouten, zoals gratis aankopen of ongeoorloofde statuswijzigingen  

---