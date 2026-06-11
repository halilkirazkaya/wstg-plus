## WSTG-CLNT-11 — Test Web Messaging

Web Messaging, of `postMessage`, maakt cross-origin communicatie mogelijk tussen window-objecten, zoals een parent-pagina en een geopende popup of een ingebedde iframe. Dit mechanisme is cruciaal voor moderne webfunctionaliteit, maar brengt aanzienlijke risico's met zich mee als de ontvangende applicatie de identiteit van de afzender niet verifieert of de message payload onjuist verwerkt. Aanvallers maken misbruik van deze kwetsbaarheden door een kwaadaardige site te hosten die de doelapplicatie insluit en gemanipuleerde berichten verstuurt om onbedoelde acties uit te voeren, gevoelige gegevens te exfiltreren of DOM-based Cross-Site Scripting (XSS) te veroorzaken. Succesvolle exploitatie vindt plaats wanneer message listeners de `origin`-eigenschap niet strikt valideren of `message.data` rechtstreeks doorgeven aan gevaarlijke execution sinks.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Medium / Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### Maakt de applicatie gebruik van de `postMessage` API voor cross-document communicatie?
- [ ] Nee — `postMessage` listeners zijn **niet** aanwezig in de client-side code  
- [ ] Ja — `postMessage` wordt gebruikt voor interne componentcommunicatie  
- [ ] Ja — `postMessage` wordt gebruikt om te communiceren met externe domeinen of widgets  

### Wordt de origin van inkomende berichten strikt gevalideerd?
- [ ] Ja — origin wordt gecontroleerd tegen een strikte whitelist met behulp van `===` of een identieke vergelijking *(Meest veilig)*  
- [ ] Ja — origin wordt gevalideerd met een regex, maar de logica **is** vatbaar voor een bypass (bijv. `^https://trusted.com`)  
- [ ] Nee — de `origin`-eigenschap wordt **niet** gecontroleerd, waardoor elk domein berichten kan verzenden  
- [ ] Nee — er wordt een wildcard `*` gebruikt in de `targetOrigin` van de afzender, wat mogelijk gegevens lekt naar kwaadaardige listeners  

### Wordt de message payload gesaneerd voordat deze door de applicatie wordt verwerkt?
- [ ] Ja — data wordt behandeld als platte tekst en er worden geen gevaarlijke sinks gebruikt  
- [ ] Ja — data wordt geparsed (bijv. `JSON.parse`) en gevalideerd voor gebruik, en een bypass is **niet mogelijk**  
- [ ] Nee — message data wordt rechtstreeks doorgegeven aan gevaarlijke sinks zoals `innerHTML`, `eval()` of `setTimeout()`  
- [ ] Nee — message data wordt gebruikt om het venster te navigeren of de `location.href` te wijzigen zonder validatie  

### Kan een aanvaller ongeoorloofde acties triggeren of gegevens exfiltreren via een kwaadaardig bericht?
- [ ] Nee — zelfs met een gespoofd bericht kunnen geen gevoelige acties of gegevens worden benaderd  
- [ ] Ja — een gemanipuleerd bericht **kan** gevoelige statuswijzigingen triggeren (bijv. wachtwoordwijziging, profielupdate)  
- [ ] Ja — een gemanipuleerd bericht **kan** leiden tot DOM-based XSS of exfiltratie van CSRF-tokens/sessiegegevens  

---