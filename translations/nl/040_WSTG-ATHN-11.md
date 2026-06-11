## WSTG-ATHN-11 — Testen van Multi-Factor Authenticatie (MFA)

Het testen van Multi-Factor Authenticatie (MFA) evalueert de robuustheid van de secundaire beveiligingslaag die is ontworpen om ongeautoriseerde toegang te voorkomen, zelfs wanneer primaire inloggegevens zijn gecompromitteerd. Aanvallers proberen regelmatig MFA te omzeilen door eindpunten te identificeren waar dit inconsistent wordt afgedwongen, zoals legacy API-versies, mobiele back-ends of wachtwoordherstel-workflows. Exploitatie omvat vaak het manipuleren van server-responses, het brute-forcen van kortstondige codes (OTP) of het misbruiken van race conditions en session management-fouten waardoor een gebruiker kan overgaan naar een geauthenticeerde status zonder de tweede factor op te geven.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### Wordt MFA consistent afgedwongen op alle authenticatieportalen?
- [ ] Ja — MFA is vereist voor alle web-, mobiele en op API gebaseerde inlogpogingen  
- [ ] Ja — maar MFA wordt **niet afgedwongen** op specifieke eindpunten (bijv. legacy `/api/v1/login` of mobiel-specifieke portalen)  
- [ ] Nee — MFA is **niet geïmplementeerd** in de applicatie *(Kritiek)*  

### Kan de MFA-verificatiestap worden omzeild via directe URL-navigatie of response-manipulatie?
- [ ] Nee — directe navigatie of parameter tampering **kan** de challenge niet omzeilen  
- [ ] Ja — direct navigeren naar interne dashboard-URL's **is mogelijk** zonder de MFA-challenge te voltooien  
- [ ] Ja — het manipuleren van de server-response (bijv. het wijzigen van een `401 Unauthorized` naar `200 OK`) **is mogelijk** om toegang te verkrijgen  

### Is het verificatieproces van de MFA-code beschermd tegen brute-force aanvallen?
- [ ] Ja — strikte Rate Limiting of account-lockout **wordt toegepast** na meerdere mislukte OTP-pogingen  
- [ ] Ja — Rate Limiting bestaat, maar het **is mogelijk** om dit te omzeilen via IP-rotatie of header-manipulatie  
- [ ] Nee — er wordt geen Rate Limiting afgedwongen en geautomatiseerd Brute Force van codes **is mogelijk**  

### Handhaaft de applicatie een veilige sessiestatus tijdens de MFA-transitie?
- [ ] Ja — sessietokens met hoge privileges worden **pas na** succesvolle voltooiing van MFA uitgegeven  
- [ ] Nee — een volledig functionele sessie-cookie wordt uitgegeven **voordat** MFA is voltooid, wat toegang tot sommige functies mogelijk maakt  
- [ ] Nee — de applicatie gebruikt een statische identifier of voorspelbare sessietransitie die **kan** worden overgenomen (hijacked)  

### Zijn alternatieve MFA-factoren (SMS, e-mail, back-upcodes) kwetsbaar voor exploitatie?
- [ ] Nee — alle factoren maken gebruik van veilige, niet-voorspelbare en tijdgebonden waarden  
- [ ] Ja — back-upcodes zijn voorspelbaar of **kunnen** worden geënumereerd  
- [ ] Ja — de "Code opnieuw verzenden"-functionaliteit kan worden misbruikt om SMS/e-mail flooding uit te voeren of gedeeltelijke contactgegevens te onthullen  

---