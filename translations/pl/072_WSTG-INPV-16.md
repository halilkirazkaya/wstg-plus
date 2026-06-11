## WSTG-INPV-16 — HTTP Request Smuggling

Ataki typu HTTP Request Smuggling (HRS) występują, gdy łańcuch serwerów HTTP (np. load balancer i serwer webowy back-end) w różny sposób interpretuje granice żądań, zazwyczaj z powodu konfliktów między nagłówkami `Content-Length` i `Transfer-Encoding`. Atakujący wykorzystuje tę desynchronizację do „przemycenia” (smuggle) wpisu do bufora żądań serwera back-end, skutecznie poprzedzając żądanie kolejnego legalnego użytkownika fragmentem kontrolowanym przez atakującego. Technika ta pozwala na przeprowadzenie poważnych ataków, w tym przejmowanie sesji (Session Hijacking), omijanie mechanizmów kontroli bezpieczeństwa oraz zatruwanie pamięci podręcznej (Cache Poisoning). Eksploatacja jest zazwyczaj wykonywana poprzez analizę opartą na czasie (timing-based analysis) lub poprzez obserwację nieoczekiwanych odpowiedzi z back-endu po wysłaniu kolejnych żądań do serwera.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom krytyczności** | Krytyczny / Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Narzędzia:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Czy serwery front-end i back-end obsługują nagłówek `Transfer-Encoding: chunked` w sposób spójny?
- [ ] Tak — oba serwery obsługują kodowanie chunked identycznie i desynchronizacja **nie jest możliwa**  
- [ ] Nie — serwery interpretują nagłówki w różny sposób, ale stosowana jest **sanityzacja/normalizacja**  
- [ ] Nie — desynchronizacja CL.TE (Content-Length/Transfer-Encoding) **jest możliwa** *(Krytyczny)*  
- [ ] Nie — desynchronizacja TE.CL (Transfer-Encoding/Content-Length) **jest możliwa** *(Krytyczny)*  

### Czy atakujący może zatruć pamięć podręczną serwera lub klienta za pomocą przemyconego żądania?
- [ ] Nie — buforowanie (caching) jest **wyłączone** lub niepodatne na zatruwanie  
- [ ] Tak — przemycone żądania **mogą** przekierowywać użytkowników do złośliwych domen lub wstrzykiwać skrypty poprzez zatruwanie pamięci podręcznej (Cache Poisoning)  

### Czy możliwe jest przechwycenie żądań innych użytkowników lub tokenów sesyjnych poprzez konkatenację żądań?
- [ ] Nie — smuggling **nie pozwala** na ujawnienie danych innych użytkowników  
- [ ] Tak — smuggling pozwala na dołączenie żądania kolejnego użytkownika do kontrolowanego przez atakującego body żądania `POST`, co czyni eksfiltrację danych **możliwą**  

### Czy infrastruktura korzysta z HTTP/2 i czy jest podatna na desynchronizację H2.CL lub H2.TE?
- [ ] Nie — aplikacja korzysta wyłącznie z HTTP/1.1 lub protokół HTTP/2 jest zaimplementowany bezpiecznie bez obniżania wersji (downgrading)  
- [ ] Tak — występuje obniżenie wersji z HTTP/2 do HTTP/1.1 (cleartext downgrade) i desynchronizacja **jest możliwa**  

### Czy nieprawidłowo sformułowane nagłówki (np. `Transfer-Encoding:  chunked` ze spacją) są obsługiwane w bezpieczny sposób?
- [ ] Tak — nieprawidłowe nagłówki są odrzucane lub normalizowane na wszystkich poziomach infrastruktury  
- [ ] Nie — desynchronizacja TE.TE **jest możliwa** z powodu niespójnej obsługi zniekształconych lub ukrytych nagłówków  

---