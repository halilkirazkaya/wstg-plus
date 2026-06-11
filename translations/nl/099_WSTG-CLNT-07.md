## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) is een browsergebaseerd mechanisme dat een server toestaat om expliciet toegang tot zijn resources te verlenen vanuit verschillende origins, waardoor de Same-Origin Policy (SOP) effectief wordt versoepeld. Misconfiguraties treden op wanneer een applicatie willekeurige origins te veel vertrouwt, vaak door de `Origin` header te reflecteren in de `Access-Control-Allow-Origin` response header of door slecht geïmplementeerde regex whitelists te gebruiken. Vanuit het perspectief van een aanvaller kan een laks CORS-beleid worden misbruikt door een kwaadaardig script te hosten op een gecontroleerd domein dat geauthenticeerde verzoeken doet naar de kwetsbare applicatie, waardoor de aanvaller gevoelige gegevens, zoals CSRF tokens of PII, rechtstreeks uit de response body kan exfiltreren. Deze kwetsbaarheid is het meest kritiek op API-endpoints die geauthenticeerde sessies verwerken en niet-publieke informatie retourneren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Tools:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Implementeert de applicatie CORS-headers op geauthenticeerde endpoints?
- [ ] Nee — CORS is **niet ingeschakeld**; browser valt terug op de strikte Same-Origin Policy  
- [ ] Ja — CORS-headers zijn aanwezig en beperkt tot een strikte **whitelist**  
- [ ] Ja — CORS-headers zijn aanwezig maar **permissief** geconfigureerd  

### Hoe verwerkt de server de `Access-Control-Allow-Origin` (ACAO) header?
- [ ] ACAO is ingesteld op een specifiek, **vertrouwd** statisch domein  
- [ ] ACAO is ingesteld op `*` (wildcard) en credentials **kunnen niet** worden verzonden  
- [ ] ACAO is ingesteld op `*` (wildcard) en credentials **kunnen** worden verzonden *(Opmerking: De meeste browsers blokkeren dit)*  
- [ ] ACAO **reflecteert dynamisch** de waarde van de `Origin` header uit het verzoek  

### Is de `Access-Control-Allow-Credentials` (ACAC) header ingesteld op `true`?
- [ ] Nee — ACAC is **niet aanwezig** of ingesteld op `false`  
- [ ] Ja — ACAC is ingesteld op `true`, maar ACAO is **beperkt** tot vertrouwde origins  
- [ ] Ja — ACAC is ingesteld op `true` en ACAO **reflecteert** willekeurige origins *(Kritiek)*  

### Kan de origin-whitelist worden omzeild via subdomein- of null origin-manipulatie?
- [ ] Nee — whitelist wordt strikt afgedwongen en **kan niet** worden omzeild  
- [ ] Ja — whitelist accepteert **elk** subdomein van het doel (bijv. `attacker.target.com`)  
- [ ] Ja — whitelist accepteert de `null` origin via sandboxed iframes  
- [ ] Ja — whitelist is kwetsbaar voor regex bypasses (bijv. `target.com.attacker.com` of `target.com-safe.com`)  

### Staat de CORS-configuratie de exfiltratie van gevoelige gegevens toe?
- [ ] Nee — blootgestelde endpoints bevatten **geen** gevoelige of sessiespecifieke gegevens  
- [ ] Ja — geauthenticeerde endpoints retourneren PII, credentials of CSRF tokens die **kunnen** worden gelezen door een ongeautoriseerde origin  

---