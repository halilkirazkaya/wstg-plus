## WSTG-CONF-06 — Test HTTP-methoden

Het testen van HTTP-methoden omvat het identificeren van welke verbs door de webserver en applicatie worden ondersteund, om te waarborgen dat alleen noodzakelijke functionaliteit wordt blootgesteld. Naast de standaard `GET` en `POST` kunnen servers onbedoeld gevaarlijke methoden inschakelen zoals `PUT`, `DELETE`, of `TRACE`. Deze kunnen aanvallers in staat stellen om kwaadaardige bestanden te uploaden, bestaande inhoud te verwijderen of sessiecookies te exfiltreren via Cross-Site Tracing (XST). Pentesters onderzoeken response headers zoals `Allow` of `Public` en proberen beperkingen te omzeilen met behulp van method overriding-technieken of verb tampering om toegang te krijgen tot ongeautoriseerde functionaliteit. Deze test is cruciaal voor het harden van het aanvalsoppervlak en het voorkomen van misconfiguraties die kunnen leiden tot server-compromise of ongeautoriseerde datamanipulatie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld* |

> *De ernst wordt Hoog als `PUT` of `DELETE` ongeautoriseerde bestandsmanipulatie toestaan of als `TRACE` de exfiltratie van sessietokens vergemakkelijkt.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Zijn gevaarlijke HTTP-methoden zoals `PUT` of `DELETE` uitgeschakeld in de gehele applicatie?
- [ ] Ja — alleen veilige methoden (GET, POST, HEAD) **zijn ingeschakeld**  
- [ ] Nee — `PUT` of `DELETE` **zijn ingeschakeld** maar vereisen geldige authenticatie  
- [ ] Nee — `PUT` of `DELETE` **zijn ingeschakeld** en toegankelijk zonder authenticatie *(Kritiek)*  

### Is de `TRACE`-methode uitgeschakeld om Cross-Site Tracing (XST) te voorkomen?
- [ ] Ja — `TRACE`-methode **is uitgeschakeld** of retourneert een 405 Method Not Allowed  
- [ ] Nee — `TRACE`-methode **is ingeschakeld** maar reflecteert geen gevoelige headers  
- [ ] Nee — `TRACE`-methode **is ingeschakeld** en reflecteert `Cookie` of `Authorization` headers *(Gemiddeld)*  

### Wordt HTTP Method Overriding (bijv. `X-HTTP-Method-Override`) ondersteund door de server?
- [ ] Nee — server verwerkt **geen** method override headers  
- [ ] Ja — server verwerkt override headers maar access controls **worden afgedwongen**  
- [ ] Ja — server verwerkt override headers en een access control bypass **is mogelijk**  

### Reageert de server veilig op willekeurige of misvormde HTTP-methoden?
- [ ] Ja — server retourneert een 405 Method Not Allowed of 501 Not Implemented  
- [ ] Nee — server retourneert een 200 OK of 500 Error voor willekeurige methoden zoals `JEFF` of `TEST`  
- [ ] Nee — het gebruik van willekeurige methoden maakt het omzeilen van authenticatie- of autorisatiefilters mogelijk  

### Is de `OPTIONS`-methode beperkt of geconfigureerd om informatieblootstelling te minimaliseren?
- [ ] Ja — `OPTIONS` is uitgeschakeld of retourneert een minimale `Allow`-header  
- [ ] Nee — `OPTIONS` onthult een breed scala aan ingeschakelde methoden, inclusief DAV of debugging verbs  

---