## WSTG-SESS-06 — Testen op Uitlogfunctionaliteit

De uitlogfunctionaliteit is een kritieke beveiligingsmaatregel die ontworpen is om de sessie van een gebruiker te beëindigen en de bijbehorende sessie-identificatoren zowel aan de client- als aan de serverzijde ongeldig te maken. Het niet correct implementeren van de uitlogfunctionaliteit maakt Session Fixation of Hijacking aanvallen mogelijk, aangezien een aanvaller die toegang krijgt tot een systeem nadat een gebruiker is "uitgelogd", potentieel het nog steeds actieve Session Token kan hergebruiken. Pentesters evalueren dit door te controleren of de sessiecookie uit de browser wordt verwijderd, of de server-side sessiestatus expliciet wordt vernietigd, en of navigatie via de 'Terug'-knop toegang geeft tot gecachte gevoelige gegevens. Een veilige implementatie van uitloggen zorgt ervoor dat zodra de actie is geactiveerd, geen enkel volgend verzoek met het oude Session Token meer door de applicatie wordt geautoriseerd.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Is er een functioneel uitlogmechanisme aanwezig en toegankelijk?
- [ ] Ja — uitlogknop is aanwezig en triggert een verzoek tot beëindiging  
- [ ] Nee — uitlogknop **ontbreekt** of triggert **geen** beëindigingsevent  

### Wordt de sessie-identificator aan de serverzijde ongeldig gemaakt?
- [ ] Ja — de server weigert alle volgende verzoeken die het oude Session Token gebruiken  
- [ ] Nee — de server **blijft** het oude Session Token **accepteren** na het uitloggen  

### Verwijdert de applicatie sessiecookies in de browser bij het uitloggen?
- [ ] Ja — cookies worden overschreven met een verlopen datum of een lege waarde  
- [ ] Ja — cookies blijven aanwezig maar zijn **niet langer geldig** op de server  
- [ ] Nee — cookies blijven in de browser aanwezig en **behouden** hun oorspronkelijke waarden  

### Is gevoelige geauthenticeerde content toegankelijk via de 'Terug'-knop van de browser na het uitloggen?
- [ ] Nee — `Cache-Control: no-store` of vergelijkbare headers voorkomen het bekijken van gevoelige pagina's na het uitloggen  
- [ ] Ja — gevoelige pagina's worden gecacht en **kunnen** worden bekeken via navigatie nadat de sessie is beëindigd  

---