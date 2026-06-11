## WSTG-ATHN-10 — Testowanie słabszego uwierzytelniania w kanałach alternatywnych

Testowanie pod kątem słabszego uwierzytelniania w kanałach alternatywnych polega na identyfikowaniu i analizowaniu drugorzędnych ścieżek dostępu do konta — takich jak API mobilne, procesy odzyskiwania haseł, interfejsy pomocy technicznej (help desk) lub starsze subdomeny — które mogą nie wymuszać rygorów bezpieczeństwa na tym samym poziomie, co główny portal webowy. Te alternatywne kanały często nie posiadają solidnego uwierzytelniania wieloskładnikowego (MFA), restrykcyjnych mechanizmów Rate Limiting lub złożonych wymagań dotyczących haseł, tworząc „najsłabsze ogniwo”, które podważa cały system uwierzytelniania. Atakujący celowo wybierają te pominięte punkty wejścia, aby przeprowadzać ataki typu Credential Stuffing lub omijać MFA, wykorzystując rozbieżności w sposobie weryfikacji tożsamości przez różne interfejsy. Zapewnienie parytetu zabezpieczeń na wszystkich powierzchniach uwierzytelniania jest niezbędne, aby zapobiec nieautoryzowanemu dostępowi oraz zachować integralność sesji i danych użytkownika.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Czy alternatywne kanały uwierzytelniania są obecne i zidentyfikowane?
- [ ] Nie — istnieje tylko pojedynczy kanał uwierzytelniania  
- [ ] Tak — zidentyfikowano wiele kanałów (np. API aplikacji mobilnej, klient desktopowy, portal legacy, usługi SOAP)  

### Czy alternatywne kanały wymuszają parytet bezpieczeństwa z kanałem głównym?
- [ ] Tak — wszystkie kanały wymuszają **identyczną** złożoność haseł i wymagania MFA  
- [ ] Tak — kanały wymuszają złożoność haseł, ale MFA **nie jest wymagane** lub **może** zostać pominięte  
- [ ] Nie — kanały alternatywne mają **znacznie słabsze** wymagania dotyczące uwierzytelniania  

### Czy mechanizmy Rate Limiting i blokady konta są spójne we wszystkich kanałach?
- [ ] Tak — Rate Limiting i blokady konta są **konsekwentnie wymuszane** we wszystkich punktach końcowych (endpoints)  
- [ ] Tak — mechanizmy kontrolne są obecne, ale ich ominięcie **jest możliwe** w określonych kanałach (np. API mobilne)  
- [ ] Nie — Rate Limiting lub blokada konta **nie są stosowane** w drugorzędnych ścieżkach uwierzytelniania  

### Czy można ominąć MFA poprzez przełączenie się na kanał alternatywny?
- [ ] Nie — MFA jest obowiązkowe i **nie może** zostać pominięte bez względu na punkt wejścia  
- [ ] Tak — MFA jest wymagane na portalu webowym, ale **nie jest wymuszane** w API mobilnym lub legacy  

### Czy procesy odzyskiwania haseł lub procedury „zapomniałem hasła” są słabsze niż standardowe logowanie?
- [ ] Nie — procesy odzyskiwania wykorzystują silną weryfikację pozapasmową (out-of-band) i nie ujawniają informacji  
- [ ] Tak — procesy odzyskiwania wykorzystują słabe pytania bezpieczeństwa, które **można** łatwo odgadnąć lub sprawdzić (OSINT)  
- [ ] Tak — procesy odzyskiwania są podatne na enumerację lub **nie wymagają** tego samego poziomu potwierdzenia tożsamości, co standardowe logowanie  

---