## WSTG-ATHZ-05 — Testowanie podatności OAuth

Podatności OAuth obejmują szereg błędów w implementacji protokołów OAuth 2.0 lub OpenID Connect (OIDC), które często prowadzą do całkowitego przejęcia konta (account takeover) lub nieautoryzowanego dostępu do danych. Luki te zazwyczaj objawiają się podczas przepływu autoryzacji (authorization flow), a konkretnie w zakresie walidacji parametru `redirect_uri`, entropii i weryfikacji parametru `state` oraz bezpiecznej obsługi tokenów dostępu (access tokens) lub tokenów ID (ID tokens). Atakujący wykorzystują te słabości, przechwytując kody autoryzacyjne, przekierowując tokeny do domen kontrolowanych przez siebie za pomocą otwartych przekierowań (open redirects) lub przeprowadzając ataki Cross-Site Request Forgery (CSRF) w celu powiązania sesji ofiary z kontem atakującego. Ponieważ OAuth często służy jako podstawowy mechanizm uwierzytelniania, pojedynczy błąd konfiguracji może zagrozić integralności całego systemu tożsamości użytkowników.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Narzędzia:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Czy parametr `state` jest zaimplementowany i weryfikowany w celu zapobiegania CSRF?
- [ ] Tak — parametr `state` jest obowiązkowy, unikalny dla każdej sesji i rygorystycznie weryfikowany po stronie serwera  
- [ ] Tak — parametr `state` jest obecny, ale jest statyczny lub przewidywalny w różnych sesjach  
- [ ] Tak — parametr `state` jest obecny, ale aplikacja **nie weryfikuje** go podczas wywołania zwrotnego (callback)  
- [ ] Nie — parametr `state` **nie jest** używany w żądaniu autoryzacji *(Krytyczny)*  

### Czy `redirect_uri` jest rygorystycznie weryfikowany względem białej listy (whitelist)?
- [ ] Tak — akceptowane są wyłącznie dokładne dopasowania do wcześniej zarejestrowanej białej listy  
- [ ] Tak — walidacja jest stosowana, ale możliwe jest jej obejście (bypass) poprzez Path Traversal lub manipulację subdomenami  
- [ ] Tak — walidacja jest stosowana, ale możliwe jest jej obejście poprzez Parameter Pollution lub Fragment Injection  
- [ ] Nie — aplikacja **zezwala** na dowolne adresy URL w parametrze `redirect_uri` *(Krytyczny)*  

### Czy można zmanipulować `response_type`, aby zmienić przepływ (flow)?
- [ ] Nie — aplikacja akceptuje tylko zamierzony przepływ (np. `code`) i odrzuca inne  
- [ ] Tak — zmiana z `code` na `token` (Implicit flow) **jest możliwa**, co powoduje wyciek tokenów w adresie URL  
- [ ] Tak — przepływy hybrydowe (hybrid flows) lub nieautoryzowane wartości `response_type` są **włączone** i przetwarzane  

### Czy tokeny dostępu lub kody autoryzacyjne wyciekają poprzez nagłówki Referer lub historię przeglądarki?
- [ ] Nie — tokeny/kody są obsługiwane w bezpieczny sposób, a wrażliwe strony używają odpowiedniej polityki `Referrer-Policy`  
- [ ] Tak — tokeny/kody **wyciekają** do domen zewnętrznych poprzez nagłówek `Referer`  
- [ ] Tak — tokeny/kody **są widoczne** w historii przeglądarki z powodu niebezpiecznego buforowania (caching) lub żądań GET  

### Czy aplikacja wymusza zasadę najniższych uprawnień (Least Privilege) dla zakresów (scopes) OAuth?
- [ ] Tak — aplikacja żąda tylko minimalnych zakresów niezbędnych do działania danej funkcjonalności  
- [ ] Nie — aplikacja domyślnie żąda nadmiarowych lub administracyjnych zakresów uprawnień  
- [ ] Nie — użytkownik **nie może** przejrzeć ani zrezygnować z określonych zakresów (scopes) podczas fazy wyrażania zgody (consent phase)  

---