## WSTG-INPV-11 — Code Injection

Code injection (wstrzykiwanie kodu) występuje, gdy aplikacja ewaluuje fragmenty kodu dostarczone przez użytkownika w swoim środowisku wykonawczym (runtime environment), zazwyczaj poprzez niebezpieczne funkcje specyficzne dla języka, takie jak `eval()`, `exec()` lub `system()`. Podatność ta różni się od command injection, ponieważ celuje w kontekst wykonawczy języka programowania (np. PHP, Python, Node.js), a nie w powłokę bazowego systemu operacyjnego. Atakujący wykorzystują te punkty do wykonywania dowolnej logiki, eksfiltracji zmiennych środowiskowych lub uzyskania pełnego zdalnego wykonania kodu (Remote Code Execution (RCE)) na serwerze. Pentesterzy powinni zbadać funkcjonalności przetwarzające wyrażenia matematyczne, szablony po stronie serwera (server-side templates) lub dynamiczne parametry konfiguracyjne, w których dane wejściowe mogą być interpretowane jako kod wykonywalny.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom dotkliwości** | Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Czy jakiekolwiek funkcje aplikacji ewaluują dynamiczne dane wejściowe za pomocą funkcji ewaluacji specyficznych dla języka?
- [ ] Nie — funkcje dynamicznej ewaluacji **nie występują** w kodzie źródłowym  
- [ ] Tak — funkcje takie jak `eval()`, `exec()` lub `include()` są używane, ale **nie przyjmują** danych wejściowych od użytkownika  
- [ ] Tak — funkcje są używane i przetwarzają dane dostarczone przez użytkownika  

### Czy przed ewaluacją stosowane są mechanizmy walidacji lub sanityzacji danych wejściowych?
- [ ] Tak — dane wejściowe są ściśle walidowane względem **sztywnej białej listy (whitelist)**  
- [ ] Tak — dane wejściowe są sanityzowane poprzez czarną listę (blacklisting), ale obejście (bypass) **jest możliwe**  
- [ ] Nie — dane wejściowe są przekazywane bezpośrednio do funkcji ewaluacji i **nie są stosowane żadne mechanizmy kontrolne**  

### Czy środowisko wykonawcze jest odizolowane (sandboxed) lub ograniczone w celu uniemożliwienia dostępu na poziomie systemu?
- [ ] Tak — wykonanie odbywa się w wysoce ograniczonej piaskownicy (sandbox), w której wywołania systemowe **nie mogą** być wykonywane  
- [ ] Tak — istnieją pewne ograniczenia, ale ucieczka z piaskownicy (sandbox escape) **jest możliwa**  
- [ ] Nie — środowisko pozwala na pełny dostęp do API języka i funkcji systemu operacyjnego *(Krytyczny)*  

### Czy Code Injection może zostać wykorzystane do eksfiltracji wrażliwych informacji?
- [ ] Nie — wykonanie jest typu "blind" i żadna komunikacja poza pasmem (out-of-band) **nie jest możliwa**  
- [ ] Tak — zmienne środowiskowe lub pliki lokalne **mogą** zostać wyeksfiltrowane za pomocą żądań HTTP/DNS  
- [ ] Tak — wyniki wykonanego kodu są **bezpośrednio odzwierciedlone** w odpowiedzi aplikacji  

### Czy aplikacja pozwala na zdalne dołączanie plików (Remote File Inclusion) lub dynamiczne ładowanie bibliotek?
- [ ] Nie — mogą być wykonywane tylko lokalne, predefiniowane ścieżki kodu  
- [ ] Tak — aplikacja **może** zostać zmuszona do załadowania i wykonania kodu ze zdalnego lub kontrolowanego przez atakującego źródła  

---