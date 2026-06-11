## WSTG-BUSL-03 — Testy kontroli integralności

Testowanie kontroli integralności koncentruje się na weryfikacji, czy aplikacja zapewnia, że dane pozostają niezmienione i wiarygodne podczas ich przesyłania przez różne procesy biznesowe lub między klientem a serwerem. Atakujący biorą na cel tę logikę, manipulując krytycznymi parametrami — takimi jak ceny produktów, ilości, uprawnienia użytkowników lub stany transakcji — wewnątrz żądań HTTP, ukrytych pól lub plików cookie (cookies). Jeśli w aplikacji brakuje weryfikacji po stronie serwera lub solidnych mechanizmów kontroli integralności, takich jak Hashing czy HMACs, napastnik może manipulować tymi wartościami w celu dokonania oszustwa, eskalacji własnych uprawnień lub obejścia zamierzonych ograniczeń biznesowych. Test ten jest kluczowy dla każdej aplikacji obsługującej transakcje finansowe, wrażliwe zmiany stanów lub wieloetapowe przepływy pracy (workflows), gdzie dane wyjściowe jednego etapu służą jako zaufane dane wejściowe dla następnego.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się Wysoki, jeśli naruszenie integralności umożliwia kradzież środków finansowych, nieautoryzowaną eskalację uprawnień lub modyfikację trwałych rekordów.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Czy wrażliwe parametry biznesowe są narażone na manipulację po stronie klienta?
- [ ] Nie — wrażliwe wartości są zarządzane wyłącznie po stronie serwera  
- [ ] Tak — wartości takie jak cena, ilość lub flagi statusu **są** ujawnione, ale zaszyfrowane  
- [ ] Tak — wartości **są** ujawnione w formie otwartego tekstu (plain text) w ukrytych polach, parametrach URL lub plikach cookie  

### Czy mechanizm integralności (np. HMAC, Checksum) jest stosowany do parametrów kontrolowanych przez klienta?
- [ ] Tak — solidna kontrola integralności **jest stosowana** i weryfikowana przy każdym żądaniu  
- [ ] Tak — kontrola integralności **jest stosowana**, ale używa słabego/przewidywalnego algorytmu lub zakodowanego na stałe (hardcoded) sekretu  
- [ ] Nie — żadne kontrole integralności **nie są stosowane** do wrażliwych parametrów  

### Czy kontrola integralności może zostać pominięta lub obliczona ponownie przez atakującego?
- [ ] Nie — weryfikacja sygnatury jest rygorystycznie wymuszana, a obejście **nie jest możliwe**  
- [ ] Tak — weryfikacja sygnatury może zostać pominięta poprzez usunięcie parametru sygnatury  
- [ ] Tak — sygnatura **może** zostać obliczona ponownie, ponieważ klucz tajny lub logika są ujawnione/słabe  

### Czy aplikacja wykonuje ponowną walidację danych po stronie backendu względem zaufanego źródła?
- [ ] Tak — serwer ponownie waliduje wszystkie wartości względem bazy danych i obejście **nie jest możliwe**  
- [ ] Tak — serwer wykonuje częściową walidację, ale obejście **jest możliwe** w określonych przypadkach brzegowych (edge cases)  
- [ ] Nie — serwer ufa danym dostarczonym przez klienta bez weryfikacji po stronie backendu *(Krytyczne)*  

### Czy możliwa jest manipulacja stanem procesu wieloetapowego?
- [ ] Nie — stan sesji jest zarządzany po stronie serwera i nie może być manipulowany  
- [ ] Tak — informacje o stanie są przekazywane przez klienta i manipulacja **jest możliwa** w celu pominięcia kroków lub powtórzenia działań  

---