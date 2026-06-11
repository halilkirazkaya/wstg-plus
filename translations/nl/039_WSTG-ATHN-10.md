## WSTG-ATHN-10 — Testen op Zwakkere Authenticatie in Alternatieve Kanalen

Het testen op zwakkere authenticatie in alternatieve kanalen omvat het identificeren en analyseren van secundaire paden naar accounttoegang — zoals mobiele API's, wachtwoordherstelflows, helpdesk-interfaces of legacy-subdomeinen — die mogelijk niet dezelfde strikte beveiliging hanteren als het primaire webportaal. Deze alternatieve kanalen missen vaak robuuste multi-factor authenticatie (MFA), strikte rate limiting of complexe wachtwoordvereisten, waardoor een "zwakste schakel" ontstaat die het gehele authenticatiesysteem ondermijnt. Aanvallers richten zich specifiek op deze over het hoofd geziene toegangspunten om credential stuffing uit te voeren of MFA te omzeilen door misbruik te maken van discrepanties in de manier waarop verschillende interfaces identiteitsverificatie afhandelen. Het waarborgen van pariteit over alle authenticatie-oppervlakken is essentieel om ongeautoriseerde toegang te voorkomen en de integriteit van de sessie en gegevens van de gebruiker te behouden.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Zijn er alternatieve authenticatiekanalen aanwezig en geïdentificeerd?
- [ ] Nee — er bestaat slechts één enkel authenticatiekanaal  
- [ ] Ja — meerdere kanalen geïdentificeerd (bijv. mobiele app API, desktop-client, legacy-portaal, SOAP-services)  

### Dwingen alternatieve kanalen dezelfde beveiligingspariteit af als het primaire kanaal?
- [ ] Ja — alle kanalen dwingen **identieke** wachtwoordcomplexiteit en MFA-vereisten af  
- [ ] Ja — kanalen dwingen wachtwoordcomplexiteit af, maar MFA is **niet vereist** of **kan** worden omzeild  
- [ ] Nee — alternatieve kanalen hebben **aanzienlijk zwakkere** authenticatievereisten  

### Zijn rate limiting en account lockout consistent over alle kanalen?
- [ ] Ja — rate limiting en lockout worden **consistent afgedwongen** op alle eindpunten  
- [ ] Ja — controles zijn aanwezig, maar omzeiling **is mogelijk** op specifieke kanalen (bijv. mobiele API)  
- [ ] Nee — rate limiting of lockout wordt **niet toegepast** op secundaire authenticatiepaden  

### Kan MFA worden omzeild door over te schakelen naar een alternatief kanaal?
- [ ] Nee — MFA is verplicht en **kan niet** worden omzeild, ongeacht het toegangspunt  
- [ ] Ja — MFA is vereist op het webportaal, maar wordt **niet afgedwongen** op de mobiele of legacy-API  

### Zijn wachtwoordherstel- of "wachtwoord vergeten"-flows zwakker dan de standaard login?
- [ ] Nee — herstelflows maken gebruik van sterke, out-of-band verificatie en lekken geen informatie  
- [ ] Ja — herstelflows maken gebruik van zwakke beveiligingsvragen die eenvoudig kunnen worden geraden of opgezocht  
- [ ] Ja — herstelflows zijn kwetsbaar voor enumeratie of **vereisen niet** hetzelfde verificatieniveau als een standaard login  

---