## WSTG-CLNT-04 — Testowanie pod kątem przekierowań URL po stronie klienta (Client-Side URL Redirect)

Przekierowanie URL po stronie klienta (Client-Side URL Redirect) występuje, gdy aplikacja webowa przyjmuje dane wejściowe kontrolowane przez użytkownika — zazwyczaj poprzez parametry URL lub fragmenty hash — i wykorzystuje je wewnątrz sinka przekierowań po stronie klienta, takiego jak `window.location` lub `document.location.href`. Podatność ta jest wykorzystywana przez atakujących głównie do przeprowadzania zaawansowanych kampanii phishingowych, ponieważ ofiara początkowo ufa legalnej domenie, zanim zostanie po cichu przekierowana do złośliwej witryny. Poza phishingiem, przekierowania po stronie klienta mogą być często łączone w łańcuchy (chained) w celu eksfiltracji wrażliwych danych, takich jak tokeny dostępu OAuth lub identyfikatory sesji (session identifiers), jeśli przekierowanie następuje, gdy wartości te są obecne w adresie URL lub nagłówku `Referer`. W nowoczesnych aplikacjach typu Single Page Application (SPA), podatność ta często pojawia się w logice routingu, parametrach "return to" po uwierzytelnieniu lub funkcjonalnościach zmiany języka.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Średni* |

> *Poziom istotności staje się Wysoki, jeśli przekierowanie może zostać wykorzystane do eksfiltracji tokenów OAuth, identyfikatorów sesji lub obejścia mechanizmów kontroli bezpieczeństwa w procesie uwierzytelniania.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Narzędzia:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Czy aplikacja używa skryptów po stronie klienta do obsługi przekierowań na podstawie danych wejściowych użytkownika?
- [ ] Nie — przekierowanie jest obsługiwane wyłącznie po stronie serwera za pomocą kodów stanu HTTP `3xx`  
- [ ] Tak — aplikacja używa JavaScript sinks, takich jak `window.location`, do nawigacji na podstawie parametrów URL  
- [ ] Tak — aplikacja używa routera frameworka po stronie klienta (np. React Router, Vue Router), który przetwarza ścieżki dostarczone przez użytkownika  

### Czy zaimplementowano walidację danych wejściowych lub mechanizmy białej listy (whitelisting) dla docelowego adresu URL?
- [ ] Tak — wymuszana jest rygorystyczna **biała lista** (whitelist) dozwolonych domen/ścieżek i obejście **nie jest możliwe**  
- [ ] Tak — walidacja istnieje, ale opiera się na słabych wyrażeniach regularnych (regex) lub czarnych listach (blacklists), a obejście **jest możliwe**  
- [ ] Nie — aplikacja akceptuje dowolny ciąg znaków jako cel przekierowania  

### Czy logikę przekierowania można obejść za pomocą typowych technik zaciemniania (obfuscation)?
- [ ] Nie — mechanizmy kontrolne poprawnie obsługują zakodowane znaki, adresy URL relatywne względem protokołu (`//`) oraz ukośniki wsteczne (backslashes)  
- [ ] Tak — obejście **jest możliwe** przy użyciu kodowania URL lub podwójnego kodowania (double-encoding)  
- [ ] Tak — obejście **jest możliwe** przy użyciu adresów URL relatywnych względem protokołu lub znaku `@` (np. `https://expected.com@attacker.com`)  
- [ ] Tak — obejście **jest możliwe** przy użyciu specyficznych cech przeglądarek (browser quirks) lub wstrzyknięcia bajtu zerowego (null byte injection)  

### Czy proces przekierowania ujawnia wrażliwe dane witrynie docelowej?
- [ ] Nie — żadne wrażliwe dane nie są obecne w adresie URL ani przesyłane w nagłówkach podczas przekierowania  
- [ ] Tak — wrażliwe dane (np. tokeny sesyjne, klucze API) **wyciekają** poprzez nagłówek `Referer` do witryny zewnętrznej  
- [ ] Tak — wrażliwe dane obecne we fragmencie adresu URL (`#`) **są dostępne** dla skryptów witryny docelowej  

### Czy możliwe jest wykonanie kodu JavaScript poprzez sink przekierowania (DOM-based XSS)?
- [ ] Nie — sink pozwala jedynie na nawigację do protokołów `http` lub `https`  
- [ ] Tak — sink przekierowania pozwala na użycie URI `javascript:` lub `data:`, co sprawia, że DOM-based XSS **jest możliwy**  

---