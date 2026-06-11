## WSTG-IDNT-05 — Testowanie pod kątem słabej lub niewymuszonej polityki nazw użytkowników

Testowanie pod kątem słabej lub niewymuszonej polityki nazw użytkowników koncentruje się na regułach rządzących tworzeniem identyfikatorów kont oraz ich podatności na enumerację (Username Enumeration) lub podszywanie się (Spoofing). Słabe polityki często zezwalają na przewidywalne sekwencje, powszechne słowa słownikowe lub identyfikatory odzwierciedlające wewnętrzne identyfikatory pracowników, co znacząco zmniejsza przestrzeń poszukiwań dla ataków typu Brute Force i Credential Stuffing. Z perspektywy atakującego, identyfikacja tych wzorców jest pierwszym krokiem w masowym wykrywaniu kont, co często osiąga się poprzez analizę komunikatów o błędach rejestracji, zachowania funkcji resetowania hasła lub metadanych w profilach publicznych. Zapewnienie solidnej polityki uniemożliwia atakującym systematyczne mapowanie bazy użytkowników i ułatwia obronę przed ukierunkowanym Phishingiem oraz zautomatyzowanymi próbami obejścia uwierzytelniania.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Niski / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Czy nazwy użytkowników opierają się na wysoce przewidywalnych lub sekwencyjnych wzorcach?
- [ ] Nie — nazwy użytkowników są losowe, zdefiniowane przez użytkownika lub są ciągami o wysokiej entropii  
- [ ] Tak — nazwy użytkowników są zgodne z przewidywalnym formatem (np. `user1001`, `user1002`), ale uwierzytelnianie jest solidne  
- [ ] Tak — nazwy użytkowników są **ściśle sekwencyjne** lub są zgodne ze znanym formatem korporacyjnym, co ułatwia enumerację  

### Czy aplikacja wymusza wymagania dotyczące minimalnej długości i złożoności nazw użytkowników?
- [ ] Tak — podczas rejestracji **wymuszane są** rygorystyczne polityki dotyczące długości i zestawu znaków  
- [ ] Tak — polityki istnieją, ale pozwalają na niezwykle krótkie (np. 1-2 znaki) lub zbyt proste nazwy użytkowników  
- [ ] Nie — **nie jest wymuszana żadna polityka** dotycząca długości lub typów znaków w nazwach użytkowników  

### Czy poprawne nazwy użytkowników mogą zostać wyliczone poprzez odpowiedzi aplikacji?
- [ ] Nie — aplikacja zwraca ogólne komunikaty o błędach i wykazuje spójność czasową (Timing) dla wszystkich prób  
- [ ] Tak — aplikacja zwraca odrębne komunikaty o błędach (np. „Nazwa użytkownika jest już zajęta”), ale stosowane jest **Rate Limiting**  
- [ ] Tak — aplikacja **ujawnia** poprawne nazwy użytkowników poprzez błędy rejestracji, resetowanie hasła lub różnice w czasie odpowiedzi  

### Czy polityka zapobiega rejestracji powszechnych lub zarezerwowanych administracyjnych nazw użytkowników?
- [ ] Tak — aplikacja blokuje powszechne nazwy (np. `admin`, `root`, `support`) oraz terminy zarezerwowane systemowo  
- [ ] Nie — użytkownicy **mogą** rejestrować konta o nazwach wrażliwych lub administracyjnych  

### Czy polityka nazw użytkowników jest spójna we wszystkich punktach wejścia (API, Mobile, Web)?
- [ ] Tak — polityki **są stosowane** spójnie we wszystkich interfejsach i wersjach  
- [ ] Nie — starsze punkty końcowe (Legacy Endpoints) lub określone wersje API **nie wymuszają** tych samych ograniczeń nazw użytkowników  

---