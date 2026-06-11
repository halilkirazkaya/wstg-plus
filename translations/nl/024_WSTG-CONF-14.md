## WSTG-CONF-14 — Testen van andere HTTP Security Header misconfiguraties

Het testen op HTTP security header misconfiguraties omvat het verifiëren van de aanwezigheid en correcte implementatie van defensieve headers die essentiële bescherming bieden tegen veelvoorkomende webgebaseerde aanvalsvectoren. Hoewel deze headers onderliggende kwetsbaarheden in de code niet repareren, verhoogt de afwezigheid ervan het risico op man-in-the-middle (MITM) aanvallen, exploitatie van MIME-sniffing en onbedoelde cross-origin datalekken via referrers aanzienlijk. Vanuit het perspectief van een aanvaller zijn ontbrekende security headers duidelijke indicatoren van een slecht geharde (hardened) omgeving, wat vaak complexere exploitatieketens zoals session hijacking of informatie-exfiltratie vergemakkelijkt. Deze configuraties worden doorgaans geëvalueerd op serverniveau en moeten consistent worden toegepast op alle response-typen, inclusief API-endpoints en statische bronnen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audit op aanwezigheid van Security Headers
| Header | Aanwezig | Ontbrekend | Aanbevolen configuratie |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` of `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Functie-specifiek, bijv. `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Hoe worden security headers afgedwongen over het gehele applicatiebereik?
- [ ] Ja — headers worden globaal toegepast via een centrale load balancer of reverse proxy en **kunnen niet** worden overschreven  
- [ ] Ja — headers worden toegepast op responses van hoofddocumenten, maar **ontbreken** bij API-responses of statische assets  
- [ ] Nee — headers worden inconsistent toegepast of **ontbreken** over het gehele applicatiedomein  

### Is de Strict-Transport-Security (HSTS) header correct geconfigureerd?
- [ ] Ja — `max-age` is ingesteld op een lange duur (bijv. 2 jaar) en `includeSubDomains` is **ingeschakeld**  
- [ ] Ja — `max-age` is ingesteld, maar `includeSubDomains` is **uitgeschakeld**, waardoor subdomeinen kwetsbaar blijven  
- [ ] Nee — HSTS header is **niet aanwezig** of `max-age` is ingesteld op 0, wat protocol-downgrade-aanvallen mogelijk maakt  

### Voorkomt de Referrer-Policy het lekken van gevoelige URL-parameters?
- [ ] Ja — policy is ingesteld op `no-referrer` of `same-origin` *(Meest veilig)*  
- [ ] Ja — policy is ingesteld op `strict-origin-when-cross-origin`  
- [ ] Nee — policy is **niet ingesteld** of is ingesteld op `unsafe-url`, wat mogelijk tokens of gevoelige PII (Personally Identifiable Information) in referer-headers lekt  

### Voorkomt de X-Content-Type-Options header MIME-sniffing?
- [ ] Ja — `nosniff` directive is **aanwezig** en correct geïmplementeerd  
- [ ] Nee — header **ontbreekt**, wat een browser mogelijk toestaat om niet-uitvoerbare bestanden (bijv. afbeeldingen) als scripts uit te voeren  

---