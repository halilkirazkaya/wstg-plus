## WSTG-INPV-13 — Format String Injection

Format string injection vindt plaats wanneer een webapplicatie door de gebruiker gecontroleerde invoer rechtstreeks doorgeeft aan het format string-argument van een variadische functie, zoals de `printf`-familie in C of low-level logging-wrappers. Hoewel deze kwetsbaarheid minder vaak voorkomt in moderne managed-code webframeworks, blijft deze kritiek in verouderde (legacy) CGI-applicaties, native extensies of specifieke logging-systemen die interageren met low-level systeembibliotheken. Aanvallers misbruiken dit door format specifiers zoals `%x` of `%p` aan te leveren om gevoelige geheugenadressen en gegevens van de stack te lekken, of de `%n` specifier om willekeurige geheugenschrijfacties uit te voeren. Succesvolle exploitatie resulteert doorgaal in volledige systeemcompromitering via Remote Code Execution (RCE) of, minimaal, een impactvolle Denial-of-Service (DoS) conditie door procescrashes.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Verwerkt de applicatie gebruikersinvoer via native code of legacy CGI-interfaces?
- [ ] Nee — applicatie is volledig gebouwd op managed-code frameworks (bijv. JVM, CLR)  
- [ ] Ja — applicatie maakt gebruik van JNI, native C/C++ extensies of legacy binaire CGI's waar format string kwetsbaarheden **kunnen** bestaan  

### Wordt door de gebruiker verstrekte invoer doorgegeven als het primaire argument aan format-aware functies?
- [ ] Nee — invoer wordt doorgegeven als data-argumenten, en format strings zijn **statisch** en **hardcoded**  
- [ ] Ja — gebruikersinvoer wordt direct geconcatenereerd in het format string-argument, maar opschoning (sanitization) **wordt toegepast**  
- [ ] Ja — gebruikersinvoer wordt direct gebruikt als de format string en er **wordt geen** opschoning toegepast  

### Is het mogelijk om geheugeninhoud te lekken met behulp van format specifiers?
- [ ] Nee — invoer wordt gevalideerd en specifiers zoals `%p`, `%x` of `%s` **worden niet verwerkt**  
- [ ] Ja — het aanleveren van `%p` of `%x` specifiers resulteert in de reflectie van stack- of heap-geheugenadressen  
- [ ] Ja — gevoelige gegevens (bijv. pointers, stack cookies) **kunnen** worden geëxfiltreerd via format specifiers  

### Kan de applicatie worden gecrasht of gemanipuleerd met behulp van de `%n` specifier?
- [ ] Nee — specifiers die geheugenschrijfacties toestaan zijn **uitgeschakeld** of geblokkeerd door de compiler/omgeving  
- [ ] Ja — de applicatie crasht (DoS) wanneer `%s%s%s%s` of vergelijkbare strings worden opgegeven  
- [ ] Ja — willekeurige geheugenschrijfacties via `%n` zijn **mogelijk**, wat potentieel kan leiden tot Remote Code Execution (RCE)  

### Zijn moderne binaire beveiligingen (ASLR, DEP, Stack Canaries) te omzeilen via deze injectie?
- [ ] Nee — de omgeving is gehard (hardened) en geheugenlekken **kunnen niet** worden gebruikt om beveiligingen te omzeilen  
- [ ] Ja — format string injection **kan** worden gebruikt om basisadressen te lekken, waardoor het omzeilen van ASLR/DEP **mogelijk** wordt gemaakt  

---