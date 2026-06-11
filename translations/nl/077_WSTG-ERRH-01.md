## WSTG-ERRH-01 — Testen op Onjuiste Foutafhandeling

Onjuiste foutafhandeling treedt op wanneer een webapplicatie gevoelige technische details—zoals stack traces, database schema-informatie of interne bestandspaden—vrijgeeft via foutmeldingen. Aanvallers lokken deze fouten uit door het indienen van malformed input, het opvragen van niet-bestaande bronnen of het forceren van server-side exceptions om de onderliggende architectuur van de applicatie in kaart te brengen en potentiële vectoren voor verdere exploitatie te identificeren. Dit lekken van informatie dient vaak als voorloper van ernstigere aanvallen, waaronder SQL Injection of path traversal, door de aanvaller te voorzien van nauwkeurige omgevingsspecificaties. Een veilige implementatie moet generieke, gebruiksvriendelijke meldingen tonen, terwijl gedetailleerde diagnostische gegevens alleen in beveiligde, server-side logs worden vastgelegd.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Laag / Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### Gebruikt de applicatie een generieke, globale error handler voor niet-afgehandelde exceptions?
- [ ] Ja — generieke foutpagina's zijn **ingeschakeld** en onthullen **geen** technische details  
- [ ] Ja — aangepaste foutpagina's worden gebruikt, maar lekkage **is mogelijk** via specifieke headers  
- [ ] Nee — standaard server-foutpagina's (bijv. Tomcat, IIS, Nginx) zijn **zichtbaar**  

### Kan technische informatie worden geënumereerd via malformed input of randgevallen?
- [ ] Nee — fouten worden netjes afgehandeld met unieke referentie-ID's voor logging  
- [ ] Ja — stack traces of database-queries **worden vrijgegeven** in responses  
- [ ] Ja — interne bestandspaden, omgevingsvariabelen of serverversie-strings **worden vrijgegeven**  

### Lekken API-responses uitgebreide error-objecten of debug-informatie?
- [ ] Nee — API's retourneren gestandaardiseerde foutcodes en geschoonde JSON-berichten  
- [ ] Ja — API's retourneren verbose debug-objecten of volledige exception-details in de response body  
- [ ] Ja — stack traces zijn opgenomen in de `details` of `exception` velden van de JSON-response  

### Gedraagt de applicatie zich anders (Time-based of Content-based) wanneer er fouten optreden?
- [ ] Nee — response-signatures en timings zijn consistent, ongeacht het type fout  
- [ ] Ja — differentiële responses **kunnen** worden gebruikt om geldige versus ongeldige toestanden te enumereren (bijv. User Enumeration)  

---