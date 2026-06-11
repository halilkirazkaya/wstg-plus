## WSTG-ATHZ-03 — Testowanie pod kątem eskalacji uprawnień

Eskalacja uprawnień (Privilege Escalation) występuje, gdy atakujący wykorzystuje podatności w celu uzyskania dostępu do zasobów lub funkcjonalności zarezerwowanych dla użytkowników o wyższym poziomie autoryzacji lub innej tożsamości. W przypadku eskalacji pionowej (vertical escalation), standardowy użytkownik próbuje uzyskać dostęp do funkcji administracyjnych, podczas gdy eskalacja pozioma (horizontal escalation) polega na dostępie do danych należących do innego użytkownika o tym samym poziomie uprawnień. Błędy te zazwyczaj objawiają się w słabo zaimplementowanych listach kontroli dostępu (ACL), podatnościach typu Insecure Direct Object References (IDOR) lub poprzez manipulację parametrami (parameter tampering) w tokenach sesji lub tożsamości. Z perspektywy atakującego cel ten jest często osiągany poprzez manipulowanie numerycznymi identyfikatorami (ID), modyfikowanie parametrów opartych na rolach w plikach cookies lub tokenach JWT, bądź bezpośrednie wywoływanie ukrytych punktów końcowych API, w których brakuje weryfikacji autoryzacji po stronie serwera.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Status testu** | Nie przeprowadzono |
| **Krytyczność** | Wysoka / Krytyczna |

**Odnośniki:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Narzędzia:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Czy aplikacja implementuje wiele poziomów uprawnień lub architekturę wielodostępną (multi-tenancy)?
- [ ] Nie — aplikacja jest systemem jedno-użytkownikowym lub z jedną rolą  
- [ ] Tak — zdefiniowano wiele ról lub dzierżawców (tenants), co wymaga przeprowadzenia testów autoryzacji  

### Czy możliwa jest pozioma eskalacja uprawnień (dostęp na tym samym poziomie)?
- [ ] Nie — weryfikacja autoryzacji **jest stosowana** do wszystkich identyfikatorów zasobów i właścicieli sesji  
- [ ] Tak — dostęp do danych innych użytkowników **jest możliwy** poprzez IDOR lub manipulację parametrami  
- [ ] Tak — wyciek danych **jest możliwy**, ale modyfikacja danych innych użytkowników **nie jest możliwa**  

### Czy możliwa jest pionowa eskalacja uprawnień (z niskiego na wysoki poziom)?
- [ ] Nie — punkty końcowe administracyjne rygorystycznie wymuszają kontrolę dostępu opartą na rolach (RBAC)  
- [ ] Tak — funkcje administracyjne **mogą** być dostępne dla użytkowników o niskich uprawnieniach poprzez bezpośredni dostęp do adresu URL  
- [ ] Tak — funkcje administracyjne **mogą** być dostępne poprzez manipulację nagłówkami związanymi z rolami, plikami cookie lub oświadczeniami (claims) w tokenie JWT  

### Czy role lub uprawnienia użytkowników mogą być modyfikowane poprzez Mass Assignment lub Parameter Pollution?
- [ ] Nie — pola oparte na rolach są wyłącznie do odczytu i **nie mogą** być modyfikowane przez użytkowników  
- [ ] Tak — uprawnienia użytkownika **mogą** zostać podniesione poprzez dołączenie ukrytych parametrów (np. `role=admin`) w procesie aktualizacji profilu lub rejestracji  

### Czy weryfikacja autoryzacji jest konsekwentnie wymuszana we wszystkich wersjach API i metodach HTTP?
- [ ] Tak — mechanizmy kontrolne **są stosowane** konsekwentnie we wszystkich wersjach i metodach  
- [ ] Nie — starsze wersje API (np. `/v1/`) lub określone metody HTTP (np. `PUT`, `DELETE`, `PATCH`) **omijają** autoryzację  
- [ ] Nie — funkcje administracyjne są ukryte w interfejsie użytkownika (UI), ale **włączone** na poziomie API dla wszystkich użytkowników  

---