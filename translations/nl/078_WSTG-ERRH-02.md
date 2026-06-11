## WSTG-ERRH-02 — Testen op Stack Traces

Stack traces worden gegenereerd wanneer een applicatie een exceptie niet op een correcte manier afhandelt, wat resulteert in het onthullen van de interne uitvoeringsstatus en de aanroep-hiërarchie aan de eindgebruiker. Voor een penetratietester zijn deze traces van onschatbare waarde, omdat ze het applicatieframework, bibliotheekversies, interne bestandssysteempaden en de logische flow onthullen, wat de benodigde inspanning voor reconnaissance aanzienlijk vermindert. Exploitatie omvat het opzettelijk veroorzaken van fouten via misvormde invoer, onverwachte datatypen of schendingen van grensvoorwaarden om de applicatie in een onstabiele staat te dwingen en informatie over de onderliggende technologiestack te exfiltreren. Dit lekken van informatie biedt vaak de noodzakelijke context om kwetsbare componenten van derden te identificeren of om specifieke exploits te ontwerpen voor logische fouten die zijn ontdekt in de onthulde codepaden.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld* |

> *De ernst stijgt naar Gemiddeld als de stack trace gevoelige architecturale details, databasequery's of interne configuratieparameters onthult.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Worden volledige stack traces teruggestuurd naar de client bij applicatiefouten?
- [ ] Nee — de applicatie retourneert generieke foutpagina's of aangepaste foutmeldingen  
- [ ] Ja — stack traces zijn **ingeschakeld**, maar alleen voor specifieke, niet-gevoelige modules  
- [ ] Ja — volledige stack traces zijn **ingeschakeld** en zichtbaar in de HTTP response body  

### Onthullen foutmeldingen gevoelige omgevingsinformatie?
- [ ] Nee — foutmeldingen bevatten geen technische of omgevingsdetails  
- [ ] Ja — meldingen onthullen **interne bestandspaden** of server-side **mapstructuren**  
- [ ] Ja — meldingen onthullen **databaseschema's**, **SQL-query's** of **versies van bibliotheken van derden**  

### Kunnen stack traces worden getriggerd door het manipuleren van invoerparameters?
- [ ] Nee — misvormde invoer wordt correct afgehandeld of geweigerd door validatie  
- [ ] Ja — traces kunnen worden getriggerd door **onverwachte datatypen** (bijv. het indienen van een array waar een string wordt verwacht)  
- [ ] Ja — traces worden getriggerd door **null bytes**, **speciale tekens** of **grenswaarden**  

### Wordt er consistent een globale error handler toegepast in de gehele applicatie?
- [ ] Ja — een globale exception handler wordt **consistent toegepast** op alle geteste endpoints  
- [ ] Ja — er is een handler aanwezig, maar deze is **niet toegepast** op legacy, API of nieuw toegevoegde endpoints  
- [ ] Nee — er is geen globaal mechanisme voor foutafhandeling **gedetecteerd**, wat resulteert in standaard serverfouten

---