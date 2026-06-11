## WSTG-CONF-07 — Testowanie mechanizmu HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) to mechanizm bezpieczeństwa, który pozwala serwerowi WWW poinformować przeglądarki, że dostęp do niego powinien odbywać się wyłącznie przez protokół HTTPS, co zapobiega atakom typu protocol downgrade oraz przejmowaniu ciasteczek (cookie hijacking). Wymuszając szyfrowane połączenie, HSTS mityguje ataki typu Man-in-the-Middle (MitM), takie jak SSLStrip, w których atakujący przechwytuje początkowe nieszyfrowane żądanie HTTP, aby zapobiec przejściu na protokół TLS. Z perspektywy atakującego, brak tego nagłówka lub krótki czas trwania `max-age` stwarza okazję do przechwycenia wrażliwych tokenów sesyjnych lub poświadczeń podczas początkowego uścisku dłoni (handshake) przesyłanego tekstem jawnym (cleartext). Niniejszy test koncentruje się na weryfikacji obecności, czasu trwania oraz zakresu nagłówka `Strict-Transport-Security` we wszystkich wrażliwych punktach końcowych (endpoints) aplikacji.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Niski / Średni* |

> *Poziom krytyczności staje się Średni, jeśli aplikacja przetwarza dane osobowe (PII), tokeny sesyjne lub poświadczenia, ponieważ brak HSTS zwiększa szansę powodzenia ataków MitM.

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### Czy nagłówek `Strict-Transport-Security` jest obecny w odpowiedziach HTTPS?
- [ ] Tak — nagłówek **jest obecny** we wszystkich wrażliwych punktach końcowych  
- [ ] Tak — nagłówek **jest obecny**, ale tylko na stronie głównej  
- [ ] Nie — w aplikacji całkowicie **brakuje** nagłówka  

### Czy dyrektywa `max-age` jest ustawiona na wystarczający czas trwania?
- [ ] Tak — `max-age` jest ustawiony na rok lub dłużej (np. `31536000`)  
- [ ] Tak — `max-age` jest ustawiony, ale jest **zbyt krótki** (np. mniej niż 6 miesięcy)  
- [ ] Nie — dyrektywy `max-age` **brakuje** lub jest ustawiona na `0` *(Krytyczny)*  

### Czy polityka HSTS obejmuje wszystkie subdomeny?
- [ ] Tak — dyrektywa `includeSubDomains` **została zastosowana**  
- [ ] Nie — dyrektywy `includeSubDomains` **brakuje**, co naraża subdomeny na ataki typu downgrade  
- [ ] Nie — funkcja nie ma zastosowania, ponieważ subdomeny nie istnieją  

### Czy aplikacja jest zarejestrowana w mechanizmie HSTS Preloading?
- [ ] Tak — dyrektywa `preload` **jest obecna**, a domena znajduje się na liście HSTS preload  
- [ ] Tak — dyrektywa `preload` **jest obecna**, ale domena **nie znajduje się** jeszcze na liście preload  
- [ ] Nie — dyrektywy `preload` **brakuje**, co pozostawia lukę dla ataku MitM podczas pierwszej wizyty  

### Czy nagłówek HSTS jest błędnie przesyłany przez nieszyfrowany protokół HTTP?
- [ ] Nie — nagłówek jest przesyłany **wyłącznie** przez bezpieczne połączenia HTTPS  
- [ ] Tak — nagłówek **jest przesyłany** przez HTTP, co jest ignorowane przez przeglądarki i może ujawniać szczegóły konfiguracji  

---