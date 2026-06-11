## WSTG-CONF-08 — Test RIA Cross Domain Policy

Rich Internet Application (RIA) cross-domain-beleid omvat bestanden zoals `crossdomain.xml` en `clientaccesspolicy.xml` die definiëren hoe webclients—met name Adobe Flash en Microsoft Silverlight—omgaan met cross-origin-verzoeken. Deze bestanden zijn cruciaal voor de beveiliging omdat ze expliciet de Same-Origin Policy (SOP) overschrijven, waarbij wordt bepaald welke externe domeinen toestemming hebben om te communiceren met de applicatie en de antwoorden te lezen. Te tolerante configuraties, zoals het gebruik van wildcards in het `domain`-attribuut, stellen aanvallers in staat om kwaadaardige RIA-objecten op een site van derden te hosten die gevoelige gegevens kunnen exfiltreren of acties kunnen uitvoeren namens geauthenticeerde gebruikers. Pentesters lokaliseren deze bestanden doorgaans in de web root om het vertrouwensniveau te evalueren dat aan externe bronnen is verleend en om potentiële cross-origin datalekken te identificeren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Gemiddeld / Hoog* |

> *De ernst wordt Hoog als de applicatie gevoelige gebruikersgegevens of sessie-identificatoren verwerkt die geëxfiltreerd kunnen worden via een tolerant beleid.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Zijn cross-domain policy-bestanden (`crossdomain.xml` of `clientaccesspolicy.xml`) aanwezig in de web root?
- [ ] Nee — bestanden **kunnen niet** worden gevonden in de root-directory  
- [ ] Ja — `crossdomain.xml` (Flash) **is** aanwezig  
- [ ] Ja — `clientaccesspolicy.xml` (Silverlight) **is** aanwezig  
- [ ] Ja — beide RIA policy-bestanden **zijn** aanwezig  

### Is het `allow-access-from` domein-attribuut beperkt tot vertrouwde origins?
- [ ] Nee — bestanden bestaan, maar bevatten geen `allow-access-from` vermeldingen  
- [ ] Ja — specifieke, vertrouwde domeinen staan expliciet op de whitelist en bypass is **niet mogelijk**  
- [ ] Ja — domeinen staan op de whitelist, maar bevatten onbetrouwbare derde partijen of subdomeinen  
- [ ] Ja — een wildcard `*` **wordt gebruikt**, wat toegang verleent vanaf **elke** origin *(Kritiek)*  

### Staat het beleid onveilige communicatie over HTTP toe voor HTTPS-bronnen?
- [ ] Nee — `secure="true"` is ingesteld (of standaard), wat voorkomt dat HTTP-origins toegang krijgen tot HTTPS-gegevens  
- [ ] Ja — `secure="false"` is expliciet ingesteld, waardoor MITM-aanvallers beveiligde gegevens kunnen exfiltreren  

### Zijn HTTP-headers te tolerant geconfigureerd in de cross-domain configuratie?
- [ ] Nee — `allow-http-request-headers-from` is beperkt of **niet ingeschakeld**  
- [ ] Ja — specifieke headers zijn toegestaan voor vertrouwde domeinen  
- [ ] Ja — wildcard `*` wordt gebruikt voor headers, waardoor aanvallers aangepaste headers kunnen verzenden vanaf elk domein  

### Is de `site-control` (master-policy) configuratie restrictief?
- [ ] Nee — `permitted-cross-domain-policies` is ingesteld op `none` of `master-only`  
- [ ] Ja — het beleid staat toe dat andere bestanden op de server hun eigen regels definiëren (`all`)  

---