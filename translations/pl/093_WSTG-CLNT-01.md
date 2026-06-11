## WSTG-CLNT-01 — Testing for DOM Based Cross Site Scripting

DOM-based Cross-Site Scripting (DOM XSS) występuje, gdy aplikacja zawiera kod JavaScript po stronie klienta, który przetwarza dane z niezaufanego źródła w sposób niebezpieczny, zazwyczaj poprzez zapisanie tych danych z powrotem do Document Object Model (DOM) za pomocą niebezpiecznego ujścia (sink). W przeciwieństwie do wariantów reflected lub stored XSS, podatność ta istnieje całkowicie w kodzie po stronie klienta; payload często nie jest w ogóle wysyłany do serwera, ponieważ jest przetwarzany przez ujścia takie jak `innerHTML`, `eval()` lub `document.write()`. Atakujący wykorzystują tę podatność, tworząc adresy URL ze złośliwymi fragmentami lub parametrami, które po załadowaniu przez przeglądarkę ofiary wykonują dowolny kod JavaScript w kontekście sesji użytkownika. Może to prowadzić do eksfiltracji tokenów sesyjnych, kradzieży wrażliwych danych oraz wykonywania nieautoryzowanych działań w imieniu uwierzytelnionego użytkownika.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Narzędzia:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Czy niezaufane źródła (sources) są używane do przechwytywania danych kontrolowanych przez użytkownika w kodzie JavaScript?
- [ ] Nie — JavaScript **nie** używa `location.hash`, `location.search` ani `document.referrer`  
- [ ] Tak — niezaufane źródła są używane, ale dane **nie** są przekazywane do żadnych ujść wykonawczych (execution sinks)  
- [ ] Tak — niezaufane źródła są używane, a dane przepływają bezpośrednio do logiki po stronie klienta  

### Czy dane kontrolowane przez użytkownika są przekazywane do niebezpiecznych ujść (sinks) DOM?
- [ ] Nie — ujścia takie jak `innerHTML`, `outerHTML`, `document.write()` lub `eval()` **nie** są używane  
- [ ] Tak — niebezpieczne ujścia są obecne, ale dane są rygorystycznie **sanityzowane** przy użyciu bezpiecznej biblioteki, takiej jak DOMPurify  
- [ ] Tak — niebezpieczne ujścia są obecne, a dane są przetwarzane z użyciem **niewystarczającego** lub niestandardowego filtrowania opartego na wyrażeniach regularnych (regex)  
- [ ] Tak — niebezpieczne ujścia są obecne, a dane są przekazywane **bez** żadnej sanityzacji lub kodowania *(Krytyczne)*  

### Czy ujście wykonawcze (execution sink) może zostać osiągnięte poprzez payload w adresie URL?
- [ ] Nie — przepływ od źródła do ujścia (source-to-sink) **nie może** zostać wywołany poprzez zewnętrzne dane wejściowe  
- [ ] Tak — przepływ jest **możliwy**, ale wymaga złożonej interakcji ze strony użytkownika  
- [ ] Tak — przepływ jest **możliwy** i może zostać wywołany poprzez prosty fragment adresu URL lub parametr zapytania (query parameter)  

### Czy nowoczesne nagłówki bezpieczeństwa lub mechanizmy ochrony w przeglądarce neutralizują ryzyko?
- [ ] Tak — restrykcyjna polityka `Content-Security-Policy` (CSP) z dyrektywami `script-src` oraz `trusted-types` jest **włączona** i zapobiega wykonaniu kodu  
- [ ] Tak — nagłówek CSP jest obecny, ale `unsafe-inline` lub słabe skróty (hashes) sprawiają, że obejście (bypass) jest **możliwe**  
- [ ] No — brak nagłówka CSP mitygującego wykonanie kodu DOM  

### Czy aplikacja korzysta z frameworków frontendowych z wbudowanymi mechanizmami ochrony?
- [ ] Tak — framework (np. Angular, React) automatycznie koduje dane i obejście **nie jest możliwe**  
- [ ] Tak — framework jest używany, ale mechanizmy pomijania zabezpieczeń (escape hatches), takie jak `dangerouslySetInnerHTML`, są **włączone**  
- [ ] Nie — aplikacja używa czystego JavaScriptu (vanilla JavaScript) lub przestarzałych bibliotek (np. stare wersje jQuery) bez automatycznego kodowania (escaping)  

---