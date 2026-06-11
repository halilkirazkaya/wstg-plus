## WSTG-CLNT-08 — Testowanie pod kątem Cross Site Flashing

Cross-Site Flashing (XSF) to podatność po stronie klienta, występująca, gdy plik Flash (SWF) niewłaściwie przetwarza dane wejściowe kontrolowane przez użytkownika, umożliwiając atakującemu wykonanie złośliwego kodu ActionScript lub przedostanie się do środowiska JavaScript przeglądarki. Poprzez manipulację parametrami takimi jak `FlashVars` lub ciągi zapytań URL przekazywane do ujść danych (sinks) takich jak `ExternalInterface.call`, `getURL` lub `loadMovie`, atakujący może wykonywać działania w kontekście podatnej domeny, w tym przejmowanie sesji (session hijacking) i eksfiltrację danych. Podatność ta występuje głównie w starszych aplikacjach korporacyjnych (legacy), które nadal hostują pliki SWF, gdzie niewystarczająca walidacja danych wejściowych pozwala na wykorzystanie filmu Flash jako wektora dla Cross-Site Scripting (XSS) lub na obejście polityki Same-Origin Policy (SOP) poprzez zbyt liberalne konfiguracje między-domenowe (cross-domain).


| Pole | Wartość |
|---|---|
| **ID WSTG** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Średni / Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Czy w aplikacji hostowane są starsze pliki Adobe Flash (.swf)?
- [ ] Nie — żadne pliki Flash nie są hostowane ani przywoływane w zakresie aplikacji  
- [ ] Tak — pliki SWF są obecne, ale serwują wyłącznie treści statyczne  
- [ ] Tak — pliki SWF są obecne i przyjmują parametry dynamiczne poprzez `FlashVars` lub ciągi URL  

### Czy dane wejściowe przekazywane do ujść danych (sinks) ActionScript są oczyszczane?
- [ ] Tak — wszystkie dane wejściowe są rygorystycznie walidowane, a manipulacja ujściami ActionScript **nie jest możliwa**  
- [ ] Tak — walidacja jest stosowana, ale jej obejście **jest możliwe** za pomocą specyficznych technik kodowania  
- [ ] Nie — dane wejściowe są przekazywane bezpośrednio do wrażliwych ujść, takich jak `ExternalInterface.call` lub `getURL` *(Krytyczne)*  

### Czy plik Flash może zostać użyty do wykonania dowolnego kodu JavaScript (XSS)?
- [ ] Nie — `ExternalInterface` jest **wyłączone** lub parametr `allowScriptAccess` jest ustawiony na `never`  
- [ ] Tak — parametr `allowScriptAccess` jest ustawiony na `sameDomain`, ale plik SWF jest hostowany w docelowej domenie  
- [ ] Tak — parametr `allowScriptAccess` jest ustawiony na `always`, co umożliwia XSS z dowolnej domeny  

### Czy polityka `crossdomain.xml` zapobiega nieautoryzowanym żądaniom cross-origin?
- [ ] Tak — plik `crossdomain.xml` jest restrykcyjny i zezwala tylko na zaufane, konkretne źródła (origins)  
- [ ] Nie — plik `crossdomain.xml` **nie istnieje**  
- [ ] Tak — polityka jest zbyt liberalna (np. `<allow-access-from domain="*" />`)  

### Czy można wymusić na pliku SWF załadowanie zewnętrznych filmów kontrolowanych przez atakującego?
- [ ] Nie — nie można wymusić na aplikacji ładowania zewnętrznych plików SWF  
- [ ] Tak — funkcje `loadMovie` lub `loadMovieNum` przyjmują niewalidowane zewnętrzne adresy URL  

---