## WSTG-INPV-03 — Testing for HTTP Verb Tampering

HTTP Verb Tampering maakt misbruik van zwakheden in de manier waarop webservers en applicatieframeworks de toegang tot specifieke resources autoriseren op basis van de gebruikte HTTP-methode. Aanvallers proberen beveiligingsrestricties te omzeilen door standaardmethoden zoals `GET` of `POST` te vervangen door alternatieven zoals `HEAD`, `PUT`, `OPTIONS`, of zelfs willekeurige, niet-standaard strings die de backend mogelijk inconsistent verwerkt. Deze kwetsbaarheid treedt doorgaans op wanneer beveiligingsconfiguraties—zoals Java EE web.xml filters of .NET authorization regels—expliciet toegestane methoden vermelden maar geen rekening houden met andere, of wanneer een "deny-by-method" benadering wordt gebruikt in plaats van "deny-all." Succesvolle exploitatie kan resulteren in ongeautoriseerde toegang tot administratieve functies, gegevenswijziging of informatieonthulling over de interne configuratie van de server.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Test Status** | Niet uitgevoerd |
| **Severity** | Medium / High* |

> *Severity wordt High als administratieve functies of gegevenswijziging (PUT/DELETE) kunnen worden uitgevoerd zonder authenticatie.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### Reageert de applicatie op niet-standaard of willekeurige HTTP-methoden?
- [ ] Nee — de applicatie weigert alle methoden behalve degene die expliciet vereist zijn  
- [ ] Ja — de applicatie reageert op standaardmethoden (OPTIONS, TRACE) maar kan de beveiliging **niet** omzeilen  
- [ ] Ja — de applicatie accepteert willekeurige strings (bijv. FOO, TEST) en verwerkt deze als `GET` of `POST` verzoeken  

### Kan authenticatie/autorisatie worden omzeild door de HTTP-methode te wijzigen?
- [ ] Nee — beveiligingscontroles worden globaal toegepast, ongeacht het HTTP-verb  
- [ ] Ja — controles zijn aanwezig, maar omzeiling **is mogelijk** met de `HEAD`-methode  
- [ ] Ja — beveiligingscontroles worden **niet toegepast** op alternatieve methoden, wat ongeautoriseerde toegang tot beveiligde endpoints mogelijk maakt  

### Zijn gevaarlijke methoden zoals `PUT` of `DELETE` ingeschakeld op de server?
- [ ] Nee — gevaarlijke methoden zijn **uitgeschakeld** of retourneren een 405 Method Not Allowed  
- [ ] Ja — methoden zijn ingeschakeld maar vereisen geldige authenticatie met hoge privileges  
- [ ] Ja — methoden zijn **ingeschakeld** en maken ongeautoriseerde bestandsuploads of resource-verwijdering mogelijk *(Kritiek)*  

### Onthult de `OPTIONS`-methode gevoelige informatie over toegestane verbs?
- [ ] Nee — `OPTIONS` is **uitgeschakeld** of retourneert een generieke respons  
- [ ] Ja — `OPTIONS` retourneert de `Allow`-header maar geeft geen beperkte interne methoden prijs  
- [ ] Ja — `OPTIONS` onthult interne of administratieve methoden die helpen bij verdere exploitatie  

### Is de `TRACE`-methode ingeschakeld, wat mogelijk Cross-Site Tracing (XST) toestaat?
- [ ] Nee — `TRACE`- en `TRACK`-methoden zijn **uitgeschakeld**  
- [ ] Ja — `TRACE` is **ingeschakeld** en reflecteert HTTP-headers, inclusief gevoelige cookies/tokens  

---