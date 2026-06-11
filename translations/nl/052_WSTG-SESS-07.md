## WSTG-SESS-07 — Testen van Sessie-timeout

Sessie-timeout-testen evalueren of een applicatie de sessie van een gebruiker effectief beëindigt na een vooraf gedefinieerde periode van inactiviteit of totale duur. Deze controle is essentieel voor het beperken van het risico op Session Hijacking, met name op gedeelde werkstations of in scenario's waarin een aanvaller een sessie-identifier bemachtigt via netwerkinterceptie of XSS. Pentesters analyseren zowel idle timeouts, die worden geactiveerd na een periode van inactiviteit, als absolute timeouts, die de totale levensduur van een sessie beperken, ongeacht de activiteit. Vanuit het perspectief van een aanvaller bieden onbeperkte of extreem lange sessies een groter venster voor het behouden van ongeautoriseerde toegang en het omzeilen van de noodzaak voor herauthenticatie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Implementeert de applicatie een idle sessie-timeout?
- [ ] Ja — sessie verloopt na een korte periode (bijv. 15-30 minuten) van inactiviteit  
- [ ] Ja — sessie verloopt, maar de timeout-duur is **excessief lang** (bijv. > 24 uur)  
- [ ] Nee — sessie **blijft onbeperkt actief** tijdens inactiviteit  

### Wordt de sessie-timeout aan de serverzijde afgedwongen?
- [ ] Ja — server weigert verzoeken met verlopen tokens, ongeacht de status aan de clientzijde  
- [ ] Nee — timeout wordt **alleen** afgedwongen via client-side JavaScript of meta-refresh  

### Implementeert de applicatie een absolute sessie-timeout?
- [ ] Ja — sessie wordt beëindigd na een vaste maximale duur, ongeacht activiteit  
- [ ] Nee — sessie **kan** onbeperkt worden verlengd zolang er voortdurende activiteit is  

### Wordt de sessie-identifier op de server ongeldig gemaakt bij een timeout?
- [ ] Ja — sessietoken **kan niet** opnieuw worden gebruikt zodra de timeout-drempel is bereikt  
- [ ] Nee — sessietoken **is nog steeds geldig** op de server, zelfs nadat de client is omgeleid naar de inlogpagina  

### Kan de sessie onbeperkt in leven worden gehouden met behulp van geautomatiseerde heartbeat-verzoeken?
- [ ] Nee — absolute timeout of secundaire controles **voorkomen** oneindige sessieverlenging  
- [ ] Ja — periodieke achtergrondverzoeken (bijv. AJAX) **kunnen** de sessie onbeperkt behouden  

---