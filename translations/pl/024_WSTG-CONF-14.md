## WSTG-CONF-14 — Testowanie błędnych konfiguracji pozostałych nagłówków bezpieczeństwa HTTP

Testowanie błędnych konfiguracji nagłówków bezpieczeństwa HTTP polega na weryfikacji obecności i poprawnej implementacji nagłówków obronnych, które zapewniają niezbędną ochronę przed typowymi wektorami ataków internetowych. Chociaż nagłówki te nie naprawiają podatności w samym kodzie źródłowym, ich brak znacząco zwiększa ryzyko ataków typu man-in-the-middle (MITM), eksploitacji mechanizmu MIME-sniffing oraz niezamierzonego wycieku danych cross-origin poprzez nagłówki referrer. Z perspektywy atakującego, brakujące nagłówki bezpieczeństwa są wyraźnymi wskaźnikami słabo zabezpieczonego środowiska, co często ułatwia bardziej złożone łańcuchy ataków, takie jak session hijacking lub eksfiltracja informacji. Konfiguracje te są zazwyczaj oceniane na poziomie serwera i powinny być konsekwentnie stosowane we wszystkich typach odpowiedzi, w tym w punktach końcowych API i zasobach statycznych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Status testu** | Nie przeprowadzono |
| **Dotkliwość** | Niska / Średnia |

**Materiały źródłowe:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audyt obecności nagłówków bezpieczeństwa
| Nagłówek | Obecny | Brak | Zalecana konfiguracja |
|----------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` lub `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Zależnie od funkcji, np. `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### W jaki sposób nagłówki bezpieczeństwa są wymuszane w całym zakresie aplikacji?
- [ ] Tak — nagłówki są stosowane globalnie za pomocą centralnego mechanizmu load balancer lub reverse proxy i **nie mogą** zostać nadpisane  
- [ ] Tak — nagłówki są stosowane w odpowiedziach dla głównych dokumentów, ale **brak** ich w odpowiedziach API lub zasobach statycznych  
- [ ] Nie — nagłówki są stosowane niespójnie lub ich **brakuje** w całej domenie aplikacji  

### Czy nagłówek Strict-Transport-Security (HSTS) jest poprawnie skonfigurowany?
- [ ] Tak — parametr `max-age` jest ustawiony na długi czas (np. 2 lata), a `includeSubDomains` jest **włączony**  
- [ ] Tak — parametr `max-age` jest ustawiony, ale `includeSubDomains` jest **wyłączony**, co pozostawia subdomeny podatnymi na ataki  
- [ ] Nie — nagłówek HSTS **nie występuje** lub `max-age` jest ustawiony na 0, co pozwala na ataki typu protocol downgrade  

### Czy Referrer-Policy zapobiega wyciekowi wrażliwych parametrów URL?
- [ ] Tak — polityka jest ustawiona na `no-referrer` lub `same-origin` *(Najbardziej bezpieczne)*  
- [ ] Tak — polityka jest ustawiona na `strict-origin-when-cross-origin`  
- [ ] Nie — polityka **nie jest ustawiona** lub jest ustawiona na `unsafe-url`, co może prowadzić do wycieku tokenów lub wrażliwych danych PII w nagłówkach referer  

### Czy nagłówek X-Content-Type-Options zapobiega mechanizmowi MIME-sniffing?
- [ ] Tak — dyrektywa `nosniff` jest **obecna** i poprawnie zaimplementowana  
- [ ] Nie — nagłówka **brakuje**, co potencjalnie pozwala przeglądarce na wykonywanie plików niewykonywalnych (np. obrazów) jako skryptów  

---