## WSTG-INPV-16 — HTTP Request Smuggling

HTTP Request Smuggling (HRS) treedt op wanneer een keten van HTTP-servers (zoals een load balancer en een back-end webserver) de grenzen van een verzoek verschillend interpreteert, meestal als gevolg van conflicterende `Content-Length` en `Transfer-Encoding` headers. Een aanvaller exploiteert deze desynchronisatie om een item in de request buffer van de back-end server te "smokkelen", waardoor het verzoek van de volgende legitieme gebruiker effectief wordt voorafgegaan door een segment dat door de aanvaller wordt beheerd. Deze techniek maakt ernstige aanvallen mogelijk, waaronder session hijacking, het omzeilen van beveiligingscontroles en cache poisoning. Exploitatie wordt doorgaans uitgevoerd door middel van timing-gebaseerde analyse of door het observeren van onverwachte antwoorden van de back-end wanneer opeenvolgende verzoeken naar de server worden verzonden.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Teststatus** | Niet Uitgevoerd |
| **Ernst** | Kritiek / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Verwerken de front-end en back-end servers de `Transfer-Encoding: chunked` header op consistente wijze?
- [ ] Ja — beide servers verwerken chunked encoding op identieke wijze en desynchronisatie is **niet mogelijk**  
- [ ] Nee — servers interpreteren headers verschillend, maar opschoning/normalisatie **wordt toegepast**  
- [ ] Nee — CL.TE (Content-Length/Transfer-Encoding) desynchronisatie is **mogelijk** *(Kritiek)*  
- [ ] Nee — TE.CL (Transfer-Encoding/Content-Length) desynchronisatie is **mogelijk** *(Kritiek)*  

### Kan de aanvaller de server- of client-side cache vergiftigen met een gesmokkeld verzoek?
- [ ] Nee — caching is **uitgeschakeld** of niet vatbaar voor poisoning  
- [ ] Ja — gesmokkelde verzoeken **kunnen** gebruikers omleiden naar kwaadaardige domeinen of scripts injecteren via cache poisoning  

### Is het mogelijk om verzoeken of sessietokens van andere gebruikers te onderscheppen via request concatenation?
- [ ] Nee — smuggling staat blootstelling van gegevens tussen gebruikers **niet** toe  
- [ ] Ja — smuggling maakt het mogelijk om het verzoek van de volgende gebruiker toe te voegen aan een door de aanvaller beheerde `POST` body, waardoor exfiltratie **mogelijk** is  

### Maakt de infrastructuur gebruik van HTTP/2 en is deze vatbaar voor H2.CL of H2.TE desynchronisatie?
- [ ] Nee — de applicatie gebruikt alleen HTTP/1.1 of HTTP/2 is veilig geïmplementeerd zonder downgrading  
- [ ] Ja — er vinden HTTP/2 naar HTTP/1.1 cleartext downgrades plaats en desynchronisatie is **mogelijk**  

### Worden onjuist geformatteerde headers (bijv. `Transfer-Encoding:  chunked` met een spatie) veilig afgehandeld?
- [ ] Ja — onjuist geformatteerde headers worden geweigerd of genormaliseerd in alle lagen  
- [ ] Nee — TE.TE desynchronisatie is **mogelijk** vanwege inconsistente afhandeling van onjuist geformatteerde of verhulde headers  

---