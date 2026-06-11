## WSTG-INPV-08 — Testowanie pod kątem SSI Injection

Server-Side Includes (SSI) Injection występuje, gdy aplikacja nie oczyszcza danych wejściowych użytkownika przed ich włączeniem do plików HTML przetwarzanych przez silnik SSI serwera. Atakujący wykorzystują tę podatność, wstrzykując dyrektywy SSI, takie jak `<!--#exec cmd="id" -->`, w celu wykonania dowolnych poleceń powłoki (shell), odczytu lokalnych plików lub eksfiltracji zmiennych środowiskowych. Podatność ta jest zazwyczaj spotykana w aplikacjach obsługujących starsze rozszerzenia plików, takie jak `.shtml`, `.shtm` lub `.stm`, lub w sytuacjach, gdy serwer WWW jest jawnie skonfigurowany do parsowania SSI w standardowych plikach HTML. Udana eksploatacja przyznaje atakującemu te same uprawnienia, które posiada proces serwera WWW, co często prowadzi do pełnego przejęcia systemu lub ujawnienia poufnych danych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Status testu** | Nie wykonano |
| **Dotkliwość** | Wysoka |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Czy serwer WWW obsługuje i przetwarza dyrektywy SSI?
- [ ] Nie — serwer **nie obsługuje** SSI lub rozszerzenia plików takie jak `.shtml` są **wyłączone**  
- [ ] Tak — SSI **jest włączone**, ale dane wejściowe użytkownika **nie są** odzwierciedlane w przetwarzanych stronach  
- [ ] Tak — SSI **jest włączone**, a dane wejściowe użytkownika **są** odzwierciedlane w przetwarzanych stronach  

### Czy można wykonywać dowolne polecenia systemowe za pomocą dyrektywy `#exec`?
- [ ] Nie — dyrektywa `#exec` jest **wyłączona** lub ograniczona przez konfigurację serwera  
- [ ] Tak — wykonywanie poleceń **jest możliwe** za pomocą Payloadów `#exec cmd` lub `#exec cgi`  

### Czy możliwa jest eksfiltracja poufnych plików lub zmiennych środowiskowych?
- [ ] Nie — dyrektywy takie jak `#include` lub `#config` są **wyłączone**  
- [ ] Tak — odczyt plików lokalnych (np. `/etc/passwd`) **jest możliwy** poprzez `#include virtual`  
- [ ] Tak — zmienne środowiskowe serwera **mogą** zostać wyeksfiltrowane za pomocą `#printenv` lub `#echo`  

### Czy walidacja danych wejściowych lub zabezpieczenia WAF są skuteczne przeciwko Payloadom SSI?
- [ ] Tak — stosowana **jest** rygorystyczna walidacja danych wejściowych i bypass **nie jest możliwy**  
- [ ] Tak — walidacja/WAF **jest stosowana**, ale bypass **jest możliwy** przy użyciu kodowania znaków lub innej składni SSI  
- [ ] Nie — brak walidacji lub ochrony WAF dla znaków SSI (`<`, `!`, `#`, `-`, `"`)  

---