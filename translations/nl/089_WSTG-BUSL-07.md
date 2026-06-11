## WSTG-BUSL-07 — Testen van verdedigingsmechanismen tegen misbruik van de applicatie

Verdedigingsmechanismen tegen misbruik van applicaties evalueren het vermogen van de applicatie om afwijkend gebruikersgedrag te identificeren, te loggen en erop te reageren wanneer dit afwijkt van de beoogde business logic. In tegenstelling tot traditionele op signatures gebaseerde beveiliging, richten deze verdedigingen zich op het detecteren van patronen zoals snel opeenvolgende verzoeken, credential stuffing, of sequentiële enumeratie van resources die duiden op geautomatiseerd misbruik. Vanuit het perspectief van een aanvaller bepaalt deze test de weerbaarheid van de applicatie tegen automatisering en "low and slow"-aanvallen die zijn ontworpen om gegevens te exfiltreren of resources uit te putten zonder standaard beveiligingswaarschuwingen te activeren. Het niet implementeren van deze verdedigingsmechanismen resulteert vaak in grootschalige data scraping, account takeovers, of denial of service via legitieme maar resource-intensieve applicatiefuncties.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Worden rate-limiting of throttling mechanismen afgedwongen op gevoelige endpoints?
- [ ] Ja — strikte rate limiting **wordt toegepast** en afgedwongen op alle gevoelige endpoints *(Meest veilig)*  
- [ ] Ja — rate limiting **wordt toegepast** maar kan worden omzeild via IP-rotatie of header-manipulatie  
- [ ] Ja — rate limiting **wordt alleen toegepast** op authenticatie-endpoints, waardoor andere endpoints blootgesteld blijven  
- [ ] Nee — er **wordt geen** rate limiting of throttling toegepast op applicatiefuncties  

### Detecteert en blokkeert de applicatie geautomatiseerde interactiepatronen?
- [ ] Ja — gedragsanalyse of CAPTCHAs **zijn ingeschakeld** en blokkeren bots effectief  
- [ ] Ja — anti-automatisering **is ingeschakeld** maar omzeiling **is mogelijk** met behulp van headless browsers of sessie-cycling  
- [ ] Nee — de applicatie **kan geen** onderscheid maken tussen een menselijke gebruiker en een geautomatiseerd script  

### Wordt afwijkend gedrag gelogd en gevolgd door een defensieve reactie?
- [ ] Ja — misbruik triggert automatische blokkering en waarschuwt het security-team  
- [ ] Ja — misbruik wordt gelogd voor audit, maar er **wordt geen** automatische defensieve actie ondernomen  
- [ ] Nee — de applicatie **logt niet** en reageert niet op ongebruikelijke activiteitspatronen  

### Kunnen verdedigingsmechanismen worden omzeild door het manipuleren van client-side identifiers of headers?
- [ ] Nee — verdedigingsmechanismen vertrouwen op server-side status en geverifieerde telemetrie  
- [ ] Ja — controles **kunnen** worden omzeild door cookies te wissen of `User-Agent` strings te roteren  
- [ ] Ja — controles **kunnen** worden omzeild door het injecteren van headers zoals `X-Forwarded-For` of `X-Real-IP`  

---