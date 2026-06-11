## WSTG-CONF-06 — Testowanie metod HTTP

Testowanie metod HTTP polega na identyfikacji czasowników (verbs) obsługiwanych przez serwer WWW i aplikację, aby upewnić się, że udostępniona jest wyłącznie niezbędna funkcjonalność. Poza standardowymi metodami `GET` i `POST`, serwery mogą nieumyślnie włączać niebezpieczne metody, takie jak `PUT`, `DELETE` lub `TRACE`, które mogą pozwolić atakującym na przesyłanie złośliwych plików, usuwanie istniejącej zawartości lub eksfiltrację ciasteczek sesyjnych poprzez Cross-Site Tracing (XST). Pentesterzy badają nagłówki odpowiedzi, takie jak `Allow` lub `Public`, i próbują ominąć ograniczenia, stosując techniki Method Overriding lub Verb Tampering w celu uzyskania dostępu do nieautoryzowanych funkcjonalności. Ten test jest kluczowy dla utwardzania powierzchni ataku i zapobiegania błędom konfiguracji, które mogłyby prowadzić do przejęcia serwera lub nieautoryzowanej manipulacji danymi.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Niski / Średni* |

> *Poziom krytyczności staje się wysoki (High), jeśli `PUT` lub `DELETE` pozwalają na nieautoryzowaną manipulację plikami lub jeśli `TRACE` ułatwia eksfiltrację tokenów sesyjnych.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Czy niebezpieczne metody HTTP, takie jak `PUT` lub `DELETE`, są wyłączone w całej aplikacji?
- [ ] Tak — włączone są wyłącznie bezpieczne metody (GET, POST, HEAD)  
- [ ] Nie — metody `PUT` lub `DELETE` **są włączone**, ale wymagają poprawnego uwierzytelnienia  
- [ ] Nie — metody `PUT` lub `DELETE` **są włączone** i dostępne bez uwierzytelnienia *(Krytyczny)*  

### Czy metoda `TRACE` jest wyłączona, aby zapobiec Cross-Site Tracing (XST)?
- [ ] Tak — metoda `TRACE` **jest wyłączona** lub zwraca kod 405 Method Not Allowed  
- [ ] Nie — metoda `TRACE` **jest włączona**, ale nie zwraca wrażliwych nagłówków  
- [ ] Nie — metoda `TRACE` **jest włączona** i zwraca nagłówki `Cookie` lub `Authorization` *(Średni)*  

### Czy serwer obsługuje nadpisywanie metod HTTP (np. `X-HTTP-Method-Override`)?
- [ ] Nie — serwer **nie** przetwarza nagłówków nadpisywania metod  
- [ ] Tak — serwer przetwarza nagłówki nadpisywania, ale mechanizmy kontroli dostępu **są egzekwowane**  
- [ ] Tak — serwer przetwarza nagłówki nadpisywania i możliwe jest **obejście kontroli dostępu**  

### Czy serwer odpowiada w bezpieczny sposób na dowolne lub błędnie sformułowane metody HTTP?
- [ ] Tak — serwer zwraca kod 405 Method Not Allowed lub 501 Not Implemented  
- [ ] Nie — serwer zwraca kod 200 OK lub 500 Error dla dowolnych metod, takich jak `JEFF` lub `TEST`  
- [ ] Nie — użycie dowolnych metod pozwala na obejście filtrów uwierzytelniania lub autoryzacji  

### Czy metoda `OPTIONS` jest ograniczona lub skonfigurowana tak, aby zminimalizować ujawnianie informacji?
- [ ] Tak — metoda `OPTIONS` jest wyłączona lub zwraca minimalny nagłówek `Allow`  
- [ ] Nie — metoda `OPTIONS` ujawnia szeroki zakres włączonych metod, w tym czasowniki DAV lub debugowania  

---