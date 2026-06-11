## WSTG-IDNT-05 — Testen op zwak of niet-afgedwongen gebruikersnaambeleid

Het testen op zwak of niet-afgedwongen gebruikersnaambeleid richt zich op de regels voor het aanmaken van account-identifiers en hun gevoeligheid voor enumeratie (Enumeration) of spoofing. Zwak beleid staat vaak voorspelbare reeksen, algemene woordenboekwoorden of identifiers die interne werknemers-ID's spiegelen toe, wat de zoekruimte voor Brute Force- en Credential Stuffing-aanvallen aanzienlijk verkleint. Vanuit het perspectief van een aanvaller is het identificeren van deze patronen de eerste stap in grootschalige accountontdekking, wat vaak wordt bereikt door het analyseren van registratiefoutmeldingen, het gedrag van de wachtwoordreset-functionaliteit of metadata in openbare profielen. Het waarborgen van een robuust beleid voorkomt dat aanvallers systematisch het gebruikersbestand in kaart brengen en vergemakkelijkt de verdediging tegen gerichte phishing en geautomatiseerde pogingen tot Authentication bypass.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Laag / Gemiddeld |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Zijn gebruikersnamen gebaseerd op zeer voorspelbare of sequentiële patronen?
- [ ] Nee — gebruikersnamen zijn willekeurig, door de gebruiker gedefinieerd of strings met een hoge entropie  
- [ ] Ja — gebruikersnamen volgen een voorspelbaar formaat (bijv. `user1001`, `user1002`), maar de authenticatie is robuust  
- [ ] Ja — gebruikersnamen zijn **strikt sequentieel** of volgen een bekend bedrijfsformaat, wat eenvoudige Enumeration mogelijk maakt  

### Handhaaft de applicatie minimale lengte- en complexiteitseisen voor gebruikersnamen?
- [ ] Ja — strikt beleid voor lengte en tekenset **wordt afgedwongen** tijdens de registratie  
- [ ] Ja — er is beleid, maar dit staat extreem korte (bijv. 1-2 tekens) of te eenvoudige gebruikersnamen toe  
- [ ] Nee — er wordt **geen beleid** afgedwongen met betrekking tot de lengte of het type tekens van de gebruikersnaam  

### Kunnen geldige gebruikersnamen worden geënumereerd via applicatieresponses?
- [ ] Nee — de applicatie retourneert generieke foutmeldingen en vertoont een consistente timing voor alle pogingen  
- [ ] Ja — de applicatie retourneert verschillende foutmeldingen (bijv. "Gebruikersnaam al in gebruik"), maar Rate Limiting **wordt toegepast**  
- [ ] Ja — de applicatie **lekt** geldige gebruikersnamen via registratiefouten, wachtwoordresets of timingverschillen  

### Voorkomt het beleid de registratie van algemene of gereserveerde administratieve gebruikersnamen?
- [ ] Ja — de applicatie blokkeert algemene namen (bijv. `admin`, `root`, `support`) en door het systeem gereserveerde termen  
- [ ] Nee — gebruikers **kunnen** accounts registreren met gevoelige of administratieve namen  

### Is het gebruikersnaambeleid consistent over alle toegangspunten (API, Mobiel, Web)?
- [ ] Ja — beleid **wordt consistent toegepast** over alle interfaces en versies  
- [ ] Nee — legacy-endpoints of specifieke API-versies **dwingen niet** dezelfde gebruikersnaambeperkingen af  

---