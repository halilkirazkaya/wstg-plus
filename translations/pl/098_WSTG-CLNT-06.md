## WSTG-CLNT-06 — Testowanie pod kątem manipulacji zasobami po stronie klienta

Manipulacja zasobami po stronie klienta (Client-side resource manipulation) występuje, gdy aplikacja pozwala na kontrolowane przez użytkownika dane wejściowe w celu wpływania na źródło lub ścieżkę zasobów ładowanych przez przeglądarkę, takich jak skrypty, arkusze stylów lub ramki iframe. Poprzez manipulację fragmentami URI, parametrami zapytań (query parameters) lub innymi źródłami DOM, atakujący może wymusić na aplikacji załadowanie złośliwej zawartości zewnętrznej lub wykonanie nieautoryzowanego kodu. Podatność ta jest zazwyczaj spotykana w logice JavaScript, która dynamicznie uzupełnia atrybuty `src` lub `href` elementów HTML bez wystarczającej walidacji względem zaufanej białej listy (allow-list). Z perspektywy atakującego, podatność ta jest wykorzystywana poprzez spreparowanie adresu URL, który przekierowuje ładowanie zasobów na serwer kontrolowany przez atakującego, co potencjalnie skutkuje atakiem DOM-based Cross-Site Scripting (XSS), eksfiltracją wrażliwych danych lub manipulacją interfejsem użytkownika (UI redressing).


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Średni / Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Narzędzia:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### Czy aplikacja używa odbiorników (sinks) po stronie klienta do dynamicznego ładowania lub osadzania zasobów?
- [ ] Nie — zasoby są ładowane wyłącznie poprzez statyczny kod HTML lub logikę po stronie serwera  
- [ ] Tak — JavaScript po stronie klienta dynamicznie uzupełnia atrybuty zasobów, takie jak `src`, `href` lub `data`  

### Czy zaimplementowano mechanizmy walidacji lub białe listy (allow-lists) dla źródeł URI zasobów?
- [ ] Tak — rygorystyczne białe listy (allow-lists) i walidacja pochodzenia (origin validation) **są stosowane** do wszystkich dynamicznych zasobów  
- [ ] Tak — walidacja istnieje, ale opiera się na słabych czarnych listach (blacklists) lub łatwych do obejścia wyrażeniach regularnych (regex)  
- [ ] Nie — do kontrolowanych przez użytkownika ścieżek zasobów nie są stosowane żadne mechanizmy walidacji  

### Czy możliwe jest obejście walidacji URI przy użyciu alternatywnych protokołów lub kodowania?
- [ ] Nie — obejście filtrów zasobów **nie jest możliwe**  
- [ ] Tak — obejście **jest możliwe** przy użyciu innych protokołów, takich jak `data:`, `vbscript:` lub `javascript:`  
- [ ] Tak — obejście **jest możliwe** przy użyciu zakodowanych znaków lub bajtów zerowych (null bytes) w celu ominięcia prostego dopasowania ciągów znaków  

### Czy atakujący może przekierować ładowanie zasobów do zewnętrznej, niezaufanej domeny?
- [ ] Nie — ścieżki zasobów są ograniczone do tego samego pochodzenia (same origin) lub stałego podkatalogu  
- [ ] Tak — aplikacja **może** zostać zmuszona do ładowania skryptów lub stylów z dowolnej domeny zewnętrznej  

### Jaki jest maksymalny wpływ manipulacji zasobami?
- [ ] Niski — manipulacja wpływa tylko na elementy niewykonawcze, takie jak obrazy lub niewrażliwy tekst  
- [ ] Średni — manipulacja pozwala na wstrzykiwanie CSS (CSS injection) lub znaczące przekształcenie interfejsu (UI redressing)  
- [ ] Wysoki — manipulacja prowadzi do ataku DOM-based XSS lub wykonania dowolnego kodu JavaScript *(Krytyczny)*  

---