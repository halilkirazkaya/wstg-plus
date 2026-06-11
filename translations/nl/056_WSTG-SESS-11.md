## WSTG-SESS-11 — Testen op Gelijktijdige Sessies

Testen op gelijktijdige sessies (Concurrent session testing) evalueert of een applicatie een enkel gebruikersaccount toestaat om meerdere gelijktijdige actieve sessies te onderhouden via verschillende browsers, apparaten of IP-adressen. Het niet beperken van gelijktijdige sessies vergroot de kans voor aanvallers om misbruik te maken van gekaapte sessietokens (session tokens) of gecompromitteerde inloggegevens zonder de legitieme gebruiker te waarschuwen of bestaande sessies te beëindigen. Dit gedrag wordt doorgaans beoordeeld door te authenticeren vanuit meerdere bronnen en de sessiestabiliteit te monitoren, wat vaak zwakheden in de sessiebeheerlogica (session management logic) of een gebrek aan veiligheidsgerichte bedrijfsregels aan het licht brengt. Vanuit het perspectief van een aanvaller maakt het ontbreken van concurrency control heimelijke persistentie mogelijk na een initiële inbreuk en vergemakkelijkt het parallelle geautomatiseerde aanvallen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Worden gelijktijdige sessies beperkt door het applicatiebeleid?
- [ ] Ja — slechts één sessie **is toegestaan** per gebruiker; nieuwe logins maken oude sessies ongeldig *(Meest veilig)*  
- [ ] Ja — een vast aantal gelijktijdige sessies **wordt afgedwongen** en kan niet worden overschreden  
- [ ] Nee — een onbeperkt aantal gelijktijdige sessies **is mogelijk**  

### Hoe reageert de applicatie wanneer de sessielimiet is bereikt?
- [ ] De oudste actieve sessie **wordt automatisch beëindigd** (mitigatie tegen session fixation/hijacking)  
- [ ] De gebruiker **wordt gevraagd** om bestaande sessies te beëindigen voordat de nieuwe sessie wordt geautoriseerd  
- [ ] De nieuwe inlogpoging **wordt geblokkeerd** totdat een handmatige afmelding plaatsvindt vanaf een ander apparaat  
- [ ] Er wordt geen actie ondernomen — meerdere sessies blijven gelijktijdig **ingeschakeld**  

### Biedt de applicatie een interface voor sessiebeheer aan de gebruiker?
- [ ] Ja — gebruikers **kunnen** actieve sessies bekijken (IP, apparaat, tijd) en deze op afstand beëindigen  
- [ ] Ja — gebruikers **kunnen** actieve sessies bekijken maar deze **niet** op afstand beëindigen  
- [ ] Nee — gebruikers **kunnen** geen andere actieve sessies zien of beheren die aan hun account zijn gekoppeld  

### Wordt de gebruiker op de hoogte gesteld wanneer gelijktijdige inlogactiviteit wordt gedetecteerd?
- [ ] Ja — een waarschuwing (e-mail, sms of in-app) **wordt geactiveerd** bij logins vanaf nieuwe apparaten of locaties  
- [ ] Nee — er **wordt geen melding gegeven** wanneer gelijktijdige sessies tot stand worden gebracht  

---