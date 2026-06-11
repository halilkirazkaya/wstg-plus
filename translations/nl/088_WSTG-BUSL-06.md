## WSTG-BUSL-06 — Testen op het omzeilen van workflows

Workflow-omzeiling omvat het manipuleren van de logica van een applicatie om beoogde sequenties of voorwaarden binnen processen die uit meerdere stappen bestaan, te omzeilen. Aanvallers identificeren gevoelige endpoints—zoals betalingsbevestigingen, administratieve goedkeuringen of MFA-voltooiingsschermen—en proberen deze direct te benaderen, waarbij verplichte tussenstappen worden overgeslagen. Deze kwetsbaarheid doet zich voor wanneer de server-side logica er niet in slaagt een robuuste state machine te onderhouden en in plaats daarvan vertrouwt op de aanname dat een gebruiker het pad volgt dat door de UI wordt gedicteerd. Succesvolle exploitatie kan leiden tot kritieke fouten in de business logic, waaronder ongeautoriseerde transacties, privilege escalation en het omzeilen van mechanismen voor identiteitsverificatie.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als de omzeiling ongeautoriseerde financiële transacties, privilege escalation of administratieve toegang mogelijk maakt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Bevat de applicatie bedrijfsprocessen die uit meerdere fasen bestaan?
- [ ] Nee — de applicatie bestaat alleen uit acties met een enkel verzoek  
- [ ] Ja — processen met meerdere fasen (bijv. winkelwagentjes, wizards, registratie) **zijn** aanwezig  

### Wordt de volgorde van de stappen strikt afgedwongen door de server?
- [ ] Ja — server-side state tracking zorgt ervoor dat stappen **niet** kunnen worden overgeslagen  
- [ ] Ja — client-side controls suggereren een volgorde, maar de server valideert de voortgang **niet**  
- [ ] Nee — de applicatie dwingt **geen** specifieke volgorde van bewerkingen af  

### Kan een aanvaller de laatste fase van een proces bereiken door direct de terminale URL of het endpoint op te vragen?
- [ ] Nee — directe toegang tot terminale stappen is **niet mogelijk** zonder voorafgaande voltooiing  
- [ ] Ja — terminale stappen (bijv. `/api/v1/checkout/complete`) **kunnen** via een direct verzoek worden bereikt  

### Controleert de applicatie of de vereiste gegevens uit voorgaande stappen aanwezig en geldig zijn?
- [ ] Ja — de server valideert alle gegevens van voorgaande stappen voordat deze verder gaat  
- [ ] Nee — de server **is** kwetsbaar voor het "overslaan" van stappen waarbij gegevens worden verwacht maar niet worden geverifieerd  

### Kunnen beveiligingsmaatregelen zoals MFA of identiteitsverificatie worden omzeild door de workflow te manipuleren?
- [ ] Nee — kritieke controles kunnen **niet** worden omzeild via manipulatie van de workflow  
- [ ] Ja — het omzeilen van MFA of identiteitscontroles **is mogelijk** door naar de stap na de verificatie te springen  

---