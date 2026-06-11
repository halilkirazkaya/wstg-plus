## WSTG-INPV-03 — Testing for HTTP Verb Tampering

HTTP Verb Tampering polega na wykorzystywaniu słabości w sposobie, w jaki serwery WWW i frameworki aplikacji autoryzują dostęp do konkretnych zasobów w oparciu o użytą metodę HTTP. Atakujący próbują obejść ograniczenia bezpieczeństwa, zastępując standardowe metody, takie jak `GET` lub `POST`, alternatywami takimi jak `HEAD`, `PUT`, `OPTIONS`, a nawet dowolnymi, niestandardowymi ciągami znaków, które backend może przetwarzać w sposób niespójny. Podatność ta występuje zazwyczaj, gdy konfiguracje bezpieczeństwa — takie jak filtry web.xml w Java EE lub reguły autoryzacji .NET — jawnie wymieniają dozwolone metody, ale nie uwzględniają innych, lub gdy stosowane jest podejście „deny-by-method” zamiast „deny-all”. Skuteczna eksploatacja może skutkować nieautoryzowanym dostępem do funkcji administracyjnych, modyfikacją danych lub ujawnieniem informacji dotyczących wewnętrznej konfiguracji serwera.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom ważności** | Średni / Wysoki* |

> *Poziom ważności staje się Wysoki, jeśli funkcje administracyjne lub modyfikacja danych (PUT/DELETE) mogą być wykonywane bez uwierzytelnienia.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### Czy aplikacja odpowiada na niestandardowe lub dowolne metody HTTP?
- [ ] Nie — aplikacja odrzuca wszystkie metody z wyjątkiem tych jawnie wymaganych  
- [ ] Tak — aplikacja odpowiada na standardowe metody (OPTIONS, TRACE), ale **nie można** obejść zabezpieczeń  
- [ ] Tak — aplikacja akceptuje dowolne ciągi znaków (np. FOO, TEST) i przetwarza je jako żądania `GET` lub `POST`  

### Czy można obejść uwierzytelnianie/autoryzację poprzez zmianę metody HTTP?
- [ ] Nie — mechanizmy kontroli bezpieczeństwa są stosowane globalnie, niezależnie od metody HTTP (Verb)  
- [ ] Tak — mechanizmy kontroli istnieją, ale obejście **jest możliwe** przy użyciu metody `HEAD`  
- [ ] Tak — mechanizmy kontroli bezpieczeństwa **nie są stosowane** do metod alternatywnych, co pozwala na nieautoryzowany dostęp do chronionych punktów końcowych (API)  

### Czy na serwerze włączone są niebezpieczne metody, takie jak `PUT` lub `DELETE`?
- [ ] Nie — niebezpieczne metody są **wyłączone** lub zwracają kod 405 Method Not Allowed  
- [ ] Tak — metody są włączone, ale wymagają poprawnego uwierzytelnienia z wysokimi uprawnieniami  
- [ ] Tak — metody są **włączone** i pozwalają na nieautoryzowane przesyłanie plików (Exploit) lub usuwanie zasobów *(Krytyczne)*  

### Czy metoda `OPTIONS` ujawnia wrażliwe informacje o dozwolonych metodach?
- [ ] Nie — metoda `OPTIONS` jest **wyłączona** lub zwraca ogólną odpowiedź  
- [ ] Tak — `OPTIONS` zwraca nagłówek `Allow`, ale nie ujawnia chronionych metod wewnętrznych  
- [ ] Tak — `OPTIONS` ujawnia metody wyłącznie wewnętrzne lub administracyjne, co ułatwia dalszą eksploatację  

### Czy metoda `TRACE` jest włączona, potencjalnie umożliwiając Cross-Site Tracing (XST)?
- [ ] Nie — metody `TRACE` i `TRACK` są **wyłączone**  
- [ ] Tak — metoda `TRACE` jest **włączona** i zwraca nagłówki HTTP, w tym wrażliwe pliki cookie/tokeny  

---