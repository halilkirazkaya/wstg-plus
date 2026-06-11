## WSTG-BUSL-03 — Testen van integriteitscontroles

Het testen van integriteitscontroles richt zich op het verifiëren of de applicatie waarborgt dat gegevens ongewijzigd en betrouwbaar blijven terwijl deze door verschillende bedrijfsprocessen bewegen of tussen de client en server worden uitgewisseld. Aanvallers richten zich op deze logica door kritieke parameters te manipuleren – zoals productprijzen, hoeveelheden, gebruikersrechten of transactiestatussen – binnen HTTP-verzoeken, verborgen velden of cookies. Als de applicatie server-side verificatie of robuuste integriteitscontroles zoals Hashing of HMACs mist, kan een tegenstander deze waarden manipuleren om fraude te plegen, eigen bevoegdheden te verhogen (Escalate), of beoogde bedrijfsbeperkingen te omzeilen. Deze test is essentieel voor elke applicatie die financiële transacties, gevoelige statusovergangen of workflows met meerdere stappen verwerkt waarbij de output van de ene fase dient als de vertrouwde input voor de volgende.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als het falen van de integriteit financiële diefstal, ongeautoriseerde Privilege Escalation of wijziging van persistente records mogelijk maakt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Worden gevoelige bedrijfsparameters blootgesteld aan manipulatie aan de client-zijde?
- [ ] Nee — gevoelige waarden worden uitsluitend aan de server-zijde beheerd  
- [ ] Ja — waarden zoals prijs, hoeveelheid of statusflags **worden** blootgesteld maar zijn versleuteld  
- [ ] Ja — waarden **worden** in platte tekst blootgesteld binnen verborgen velden, URL-parameters of cookies  

### Wordt er een integriteitsmechanisme (bijv. HMAC, Checksum) toegepast op door de client gecontroleerde parameters?
- [ ] Ja — een robuuste integriteitscontrole **wordt toegepast** en bij elk verzoek geverifieerd  
- [ ] Ja — een integriteitscontrole **wordt toegepast** maar gebruikt een zwak/voorspelbaar algoritme of een hardcoded geheim  
- [ ] Nee — er **worden geen** integriteitscontroles toegepast op gevoelige parameters  

### Kan de integriteitscontrole worden omzeild of opnieuw worden berekend door een aanvaller?
- [ ] Nee — verificatie van de handtekening wordt strikt afgedwongen en omzeiling is **niet mogelijk**  
- [ ] Ja — verificatie van de handtekening kan worden omzeild door de handtekeningparameter weg te laten  
- [ ] Ja — de handtekening **kan** opnieuw worden berekend omdat de geheime sleutel of logica is blootgesteld/zwak is  

### Voert de applicatie backend-revalidatie van gegevens uit tegen een vertrouwde bron?
- [ ] Ja — de server re-valideert alle waarden tegen de database en omzeiling is **niet mogelijk**  
- [ ] Ja — de server voert gedeeltelijke validatie uit maar omzeiling **is mogelijk** via specifieke randgevallen (edge cases)  
- [ ] Nee — de server vertrouwt door de client verstrekte gegevens zonder backend-verificatie *(Kritiek)*  

### Is het mogelijk om de status van een proces met meerdere stappen te manipuleren?
- [ ] Nee — de sessiestatus wordt server-side beheerd en kan niet worden gemanipuleerd  
- [ ] Ja — statusinformatie wordt via de client doorgegeven en manipulatie **is mogelijk** om stappen over te slaan of acties te herhalen  

---