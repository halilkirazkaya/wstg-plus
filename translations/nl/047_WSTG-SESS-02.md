## WSTG-SESS-02 — Testing for Cookies Attributes

Sessiebeheer is vaak afhankelijk van HTTP-cookies om de status (state) te behouden, waardoor de beveiligingsattributen van deze cookies een primair doelwit zijn voor aanvallers. Door attributen zoals `HttpOnly`, `Secure` en `SameSite` weg te laten of onjuist te configureren, stellen applicaties sessietokens bloot aan diefstal via Cross-Site Scripting (XSS), interceptie via onversleutelde kanalen of Cross-Site Request Forgery (CSRF) aanvallen. Pentesters onderzoeken deze flags om te bepalen of een aanvaller sessie-identificatoren kan exfiltreren uit het document object model (DOM) van de browser of de browser kan dwingen deze te verzenden in cross-origin verzoeken. Een juiste configuratie van attributen is een fundamentele defense-in-depth maatregel om te waarborgen dat gevoelige tokens beperkt blijven tot geautoriseerde, veilige contexten.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analyse van de implementatie van cookie-flags
| Attribuut | Aanwezig | Ontbrekend |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Is de `HttpOnly`-flag toegepast op gevoelige sessie-cookies?
- [ ] Ja — toegepast op **alle** gevoelige sessie-cookies *(Meest veilig)*  
- [ ] Ja — alleen toegepast op sommige sessie-cookies  
- [ ] Nee — flag is **niet toegepast**, wat toegang via client-side scripts mogelijk maakt *(Kritiek)*  

### Wordt de `Secure`-flag afgedwongen voor cookies die via HTTPS worden verzonden?
- [ ] Ja — toegepast op **alle** cookies die via TLS worden verzonden  
- [ ] Nee — flag is **niet toegepast**, waardoor cookies via onversleutelde HTTP kunnen worden verzonden  

### Wordt het `SameSite`-attribuut gebruikt om CSRF-risico's te beperken?
- [ ] Ja — ingesteld op `Strict` of `Lax` for sessie-cookies  
- [ ] Ja — ingesteld op `None` maar met de `Secure`-flag aanwezig  
- [ ] Nee — attribuut **ontbreekt** of is ingesteld op `None` **zonder** `Secure`  

### Zijn de `Domain`- en `Path`-attributen beperkt tot de minimaal noodzakelijke scope?
- [ ] Ja — beperkt tot het **specifieke** subdomein en applicatiepad  
- [ ] Nee — scope is te **breed** (bijv. ingesteld op een hoofddomein of root `/`), wat het aanvalsoppervlak vergroot  

### Verlopen gevoelige sessie-cookies correct via de `Expires`- of `Max-Age`-attributen?
- [ ] Ja — passende verloopdata zijn ingesteld  
- [ ] Nee — cookies zijn persistent met een buitengewoon **lange** levensduur  
- [ ] Nee — cookies zijn alleen voor de sessie maar **missen** afdwinging van een server-side timeout  

---