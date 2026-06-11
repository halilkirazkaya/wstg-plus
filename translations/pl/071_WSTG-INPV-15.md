## WSTG-INPV-15 — HTTP Splitting Smuggling

Podatności typu HTTP Splitting i Smuggling wynikają z rozbieżności w sposobie, w jaki serwery proxy (frontend) i serwery zaplecza (backend) interpretują i przetwarzają granice żądań HTTP, w szczególności w odniesieniu do nagłówków `Content-Length` oraz `Transfer-Encoding`. Poprzez konstruowanie niejednoznacznych żądań, atakujący może „przemycić” (smuggle) ukryte żądanie do backendu lub wstrzyknąć sekwencje CRLF w celu rozszczepienia odpowiedzi, co prowadzi do zatrucia pamięci podręcznej (cache poisoning), przejęcia żądania (request hijacking) lub obejścia mechanizmów kontroli bezpieczeństwa. Wady te zazwyczaj objawiają się w złożonych środowiskach wykorzystujących reverse proxy, load balancery lub sieci CDN, które posiadają niespójną logikę parsowania. Z perspektywy atakującego pozwala to na przekierowanie ruchu użytkowników, kradzież poufnych tokenów sesyjnych oraz wykonywanie nieautoryzowanych działań w kontekście sesji innych użytkowników.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Narzędzia:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Czy środowisko jest podatne na przemycanie żądań (request smuggling) poprzez rozbieżności CL.TE lub TE.CL?
- [ ] Nie — serwery frontendowe i backendowe spójnie obsługują granice żądań  
- [ ] Tak — istnieją rozbieżności, ale eksploatacja **nie jest możliwa** ze względu na zabezpieczenia infrastrukturalne  
- [ ] Tak — przemycanie żądań CL.TE lub TE.CL **jest możliwe**, co pozwala ukrytym żądaniom dotrzeć do backendu *(Wysoki)*  
- [ ] Tak — TE.TE (podwójne kodowanie/obfuskacja) **jest możliwe** w celu obejścia filtrów frontendu *(Krytyczny)*  

### Czy możliwe jest uzyskanie rozszczepienia odpowiedzi HTTP (HTTP Response Splitting) poprzez wstrzyknięcie CRLF w nagłówkach?
- [ ] Nie — aplikacja poprawnie oczyszcza sekwencje CRLF we wszystkich danych wejściowych nagłówków  
- [ ] Tak — sekwencje CRLF są odzwierciedlane w nagłówkach, ale rozszczepienie odpowiedzi **nie jest możliwe**  
- [ ] Tak — wstrzyknięcie CRLF (**CRLF injection**) **jest możliwe**, co pozwala na wstrzyknięcie nagłówka lub zatrucie pamięci podręcznej (cache poisoning)  

### Czy mechanizmy kontroli bezpieczeństwa (WAF/ACL) są obchodzone przy użyciu przemyconych żądań?
- [ ] Nie — mechanizmy kontroli bezpieczeństwa mają zastosowanie zarówno do żądań zewnętrznych, jak i przemyconych  
- [ ] Tak — przemycone żądania **mogą** obejść reguły WAF frontendu lub listy kontroli dostępu (ACL) oparte na adresach IP  

### Czy możliwe jest przejęcie sesji innych użytkowników lub przekierowanie ich ruchu?
- [ ] Nie — strumienie żądań/odpowiedzi są odizolowane i nie mogą zostać skrzyżowane  
- [ ] Tak — przemycanie żądań (**request smuggling**) **jest możliwe** i pozwala na przechwytywanie żądań innych użytkowników *(Krytyczny)*  
- [ ] Tak — rozszczepienie odpowiedzi (**response splitting**) **jest możliwe** i pozwala na zatrucie pamięci podręcznej po stronie przeglądarki lub XSS  

---