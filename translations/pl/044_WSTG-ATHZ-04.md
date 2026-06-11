## WSTG-ATHZ-04 — Testowanie pod kątem podatności Insecure Direct Object References (IDOR)

Podatność Insecure Direct Object References (IDOR) występuje, gdy aplikacja zapewnia bezpośredni dostęp do obiektów na podstawie danych wejściowych dostarczonych przez użytkownika, bez przeprowadzenia weryfikacji autoryzacji w celu potwierdzenia uprawnień żądającego. Podatność ta ma znaczący wpływ na poufność i integralność, ponieważ pozwala atakującym na przeglądanie, modyfikowanie lub usuwanie danych należących do innych użytkowników lub systemu. Zazwyczaj przejawia się w parametrach URL, danych w treści żądań POST lub kluczach JSON, które odnoszą się do wewnętrznych kluczy bazy danych, nazw plików lub identyfikatorów kont. Z perspektywy atakującego, eksploatacja polega na manipulowaniu tymi identyfikatorami — często poprzez inkrementację liczb całkowitych lub Fuzzing identyfikatorów UUID — w celu uzyskania dostępu do zasobów znajdujących się poza ich autoryzowanym zakresem.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Wysoki |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Narzędzia:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### Czy aplikacja używa bezpośrednich identyfikatorów obiektów w parametrach żądania?
- [ ] Nie — aplikacja używa odniesień pośrednich lub zaszyfrowanych map *(Najbardziej bezpieczne)*  
- [ ] Tak — aplikacja używa nieprzewidywalnych identyfikatorów (np. długich UUID/hashy)  
- [ ] Tak — aplikacja używa przewidywalnych identyfikatorów (np. inkrementalnych liczb całkowitych lub nazw użytkowników)  

### Czy autoryzacja po stronie serwera jest przeprowadzana dla każdego żądania zawierającego identyfikator obiektu?
- [ ] Tak — weryfikacja po stronie serwera **nie może** zostać pominięta dla żadnego identyfikatora  
- [ ] Tak — weryfikacja po stronie serwera jest obecna, ale jej obejście **jest możliwe** poprzez Parameter Pollution lub manipulację nagłówkami  
- [ ] Nie — **nie stosuje się** żadnej weryfikacji autoryzacji w celu sprawdzenia własności żądanego obiektu  

### Czy atakujący może uzyskać dostęp lub modyfikować obiekty należące do innych użytkowników (Horizontal IDOR)?
- [ ] Nie — dostęp do danych innych użytkowników **nie jest możliwy**  
- [ ] Tak — przeglądanie danych innych użytkowników **jest możliwe**, ale modyfikacja **nie jest możliwa**  
- [ ] Tak — zarówno przeglądanie, jak i modyfikacja danych innych użytkowników **są możliwe** *(Krytyczne)*  

### Czy możliwe jest uzyskanie dostępu do obiektów na poziomie administracyjnym lub systemowym (Vertical IDOR)?
- [ ] Nie — dostęp do obiektów o wyższych uprawnieniach **nie jest możliwy**  
- [ ] Tak — dostęp do konfiguracji administracyjnych lub plików systemowych **jest możliwy**  

### Czy aplikacja ujawnia identyfikatory obiektów poprzez wyszukiwanie, metadane lub inne punkty końcowe (endpoints)?
- [ ] Nie — identyfikatory **nie są ujawniane** w sposób niezamierzony  
- [ ] Tak — identyfikatory innych użytkowników/obiektów **mogą** zostać wyliczone poprzez pomocnicze punkty końcowe lub profile publiczne  

---