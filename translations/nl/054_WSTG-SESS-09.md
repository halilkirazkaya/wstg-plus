## WSTG-SESS-09 — Testen op Session Hijacking

Session hijacking vindt plaats wanneer een aanvaller een geldige sessie-identificatie (SID) onderschept, voorspelt of vastlegt om ongeautoriseerde toegang te krijgen tot de actieve sessie van een gebruiker. Deze kwetsbaarheid komt doorgaans voort uit onvoldoende beveiliging van de transportlaag, het ontbreken van cookie-beveiligingsvlaggen of voorspelbare SID-generatie-algoritmen die een aanvaller in staat stellen om authenticatie volledig te omzeilen. Vanuit het perspectief van een aanvaller biedt succesvolle exploitatie de mogelijkheid om het slachtoffer volledig te imiteren zonder dat de inloggegevens nodig zijn, wat vaak wordt bereikt via network sniffing, Cross-Site Scripting (XSS) of session fixation-aanvallen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Worden sessie-identificaties beschermd tijdens transport?
- [ ] Ja — `Secure` flag is **ingeschakeld** en HSTS wordt strikt afgedwongen  
- [ ] Ja — `Secure` flag is **ingeschakeld** maar HSTS is **uitgeschakeld**  
- [ ] Nee — `Secure` flag is **niet toegepast**, waardoor SID-interceptie via onversleutelde kanalen mogelijk is *(Hoog)*  

### Is de sessie-identificatie beschermd tegen toegang door client-side scripts?
- [ ] Ja — `HttpOnly` flag is **toegepast** op alle sessie-gerelateerde cookies  
- [ ] Nee — `HttpOnly` flag is **niet toegepast**, waardoor SID-exfiltratie via XSS mogelijk is *(Kritiek)*  

### Implementeert de applicatie sessiebinding aan clientkenmerken?
- [ ] Ja — sessie is gebonden aan clientkenmerken (IP/User-Agent) en hergebruik is **niet mogelijk**  
- [ ] Ja — sessiebinding is aanwezig maar omzeiling **is mogelijk** via header spoofing  
- [ ] Nee — geen sessiebinding aanwezig; SID **is geldig** bij gebruik vanaf elke bron  

### Is de sessie-identificatie voldoende willekeurig om voorspelling te voorkomen?
- [ ] Ja — SID maakt gebruik van een cryptografisch veilige pseudo-random getallengenerator (CSPRNG)  
- [ ] Nee — SID-lengte is onvoldoende of volgt een **voorspelbare** reeks/patroon  

### Worden gelijktijdige sessies beheerd om de aanvalsperiode te beperken?
- [ ] Nee — applicatie dwingt strikt één enkele actieve sessie per gebruiker af  
- [ ] Ja — meerdere gelijktijdige sessies zijn **ingeschakeld** maar worden gemonitord  
- [ ] Ja — onbeperkte gelijktijdige sessies **zijn mogelijk** over verschillende apparaten/locaties  

---