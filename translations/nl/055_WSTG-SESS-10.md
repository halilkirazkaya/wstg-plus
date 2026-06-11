## WSTG-SESS-10 — Testen van JSON Web Tokens

JSON Web Tokens (JWTs) worden veelvuldig gebruikt voor stateless session management en identity propagation, maar de beveiliging hiervan is volledig afhankelijk van de correcte implementatie van signing algorithms en strikte claim-verificatie. Aanvallers proberen regelmatig authenticatie te omzeilen door misbruik te maken van "none" algorithm kwetsbaarheden, het brute-forcen van zwakke HS256 secret keys, of het uitvoeren van algorithm-switching attacks waarbij een asymmetrische public key wordt gebruikt als een symmetrisch HMAC secret. Deze kwetsbaarheden manifesteren zich doorgaans in session cookies of Authorization headers, wat een aanvaller potentieel in staat stelt om rechten te escaleren (privilege escalation) of een willekeurige gebruiker te imiteren door claims zoals `role` of `user_id` aan te passen zonder de cryptografische integriteit van het token ongeldig te maken.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Tools:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### Wordt het "none" algoritme geaccepteerd door de server?
- [ ] Nee — de server **weigert** tokens die `alg: none` gebruiken in de header  
- [ ] Ja — de server **accepteert** tokens met `alg: none` en een lege signature *(Kritiek)*  

### Hoe wordt de JWT-signature geverifieerd en beschermd tegen brute-force?
- [ ] Ja — er worden sterke asymmetrische keys (RS256/ES256) of high-entropy HMAC secrets gebruikt  
- [ ] Ja — HS256 wordt gebruikt, maar het secret **kan** worden ge-brute-forced vanwege lage entropie of standaardwaarden  
- [ ] Nee — signature-verificatie **wordt niet toegepast** of **kan** worden omzeild via algorithm switching (RS256 naar HS256)  

### Worden standaard claims (exp, nbf, iat) strikt gehandhaafd door de backend?
- [ ] Ja — `exp` (expiration) is aanwezig en **kan niet** worden omzeild  
- [ ] Ja — `exp` is aanwezig, maar de server **handhaaft** deze niet tijdens de validatie  
- [ ] Nee — expiration claims **ontbreken** of worden genegeerd, wat onbeperkte session replay mogelijk maakt  

### Voorkomt de implementatie header-gebaseerde key injection (kid, jku, x5u)?
- [ ] Ja — headers worden gesaneerd en alleen vertrouwde interne keys/bronnen zijn **toegestaan**  
- [ ] Nee — de `kid` (Key ID) header is kwetsbaar voor directory traversal of SQL Injection om naar een bekend bestand te verwijzen  
- [ ] Nee — de `jku` of `x5u` headers **kunnen** worden verwezen naar door de aanvaller beheerde URL's om kwaadaardige keys te laden  

---