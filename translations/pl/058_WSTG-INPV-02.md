## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Stored Cross Site Scripting (XSS), znany również jako Persistent XSS (trwały XSS), występuje, gdy aplikacja odbiera dane od użytkownika i przechowuje je w trwałej bazie danych, systemie plików lub innym nośniku danych bez odpowiedniej walidacji lub kodowania. Gdy ofiara przechodzi następnie na stronę, która pobiera i wyświetla te niezsanitowane dane, złośliwy skrypt wykonuje się w kontekście jej przeglądarki w ramach źródła (origin) aplikacji. Podatność ta jest szczególnie niebezpieczna, ponieważ nie wymaga od ofiary kliknięcia w specjalnie przygotowany link; każdy użytkownik przeglądający zainfekowaną stronę staje się celem, co może prowadzić do przejęcia sesji (Session Hijacking), nieautoryzowanych działań lub wyłudzania poświadczeń (Credential Harvesting). Atakujący zazwyczaj celują w sekcje komentarzy, profile użytkowników, fora dyskusyjne oraz logi administracyjne, gdzie wprowadzone dane są stale renderowane dla innych użytkowników lub administratorów.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Wysoki (High) |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Narzędzia:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Czy istnieją punkty wejścia, które przechowują dane wejściowe użytkownika w celu późniejszego wyświetlenia ich innym użytkownikom?
- [ ] Nie — aplikacja nie przechowuje danych kontrolowanych przez użytkownika do późniejszego renderowania  
- [ ] Tak — dane wejściowe są przechowywane (np. profile, komentarze), ale **nie** są renderowane w kontekście HTML  
- [ ] Tak — dane wejściowe są przechowywane i renderowane dla innych użytkowników lub administratorów  

### Czy podczas renderowania zapisanych danych stosowane jest kodowanie danych wyjściowych lub sanityzacja?
- [ ] Tak — kodowanie wyjściowe zależne od kontekstu (context-aware output encoding) **jest stosowane** i obejście **nie jest możliwe** *(Najbezpieczniej)*  
- [ ] Tak — sanityzacja (np. `DOMPurify`) **jest stosowana**, ale obejście **jest możliwe** z powodu błędów w konfiguracji  
- [ ] Nie — dane są renderowane bezpośrednio w drzewie DOM **bez** kodowania lub sanityzacji *(Poziom wysoki)*  

### Czy walidacja lub sanityzacja danych wejściowych może zostać pominięta przy użyciu alternatywnych payloadów?
- [ ] Nie — filtry w bezpieczny sposób obsługują różne rodzaje kodowania, niestandardowe znaczniki oraz programy obsługi zdarzeń (event handlers)  
- [ ] Tak — obejście **jest możliwe** przy użyciu kodowania encji HTML lub specyficznych znaczników (np. `<svg>`, `<img>`)  
- [ ] Tak — obejście **jest możliwe** poprzez payloady typu polyglot lub Mutation-based XSS (mXSS)  

### Czy Content Security Policy (CSP) zapewnia dodatkową warstwę ochrony?
- [ ] Tak — restrykcyjna polityka CSP jest **włączona** i zapobiega wykonywaniu skryptów inline oraz ładowaniu skryptów z nieautoryzowanych źródeł  
- [ ] Tak — polityka CSP jest **włączona**, ale jest błędnie skonfigurowana (np. `unsafe-inline` lub zbyt szeroki `script-src`)  
- [ ] Nie — polityka CSP **nie jest włączona** dla aplikacji  

### Czy możliwe jest osiągnięcie "Stored XSS to RCE" lub innych łańcuchów o wysokim wpływie (Blind XSS)?
- [ ] Nie — wpływ jest ograniczony do kontekstu przeglądarki po stronie klienta  
- [ ] Tak — Stored XSS **może** zostać użyty do atakowania administratorów (Blind XSS) w panelach wewnętrznych  
- [ ] Tak — Stored XSS **może** zostać połączony z innymi podatnościami (np. CSRF, eksfiltracja wrażliwych danych)  

---