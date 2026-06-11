## WSTG-INPV-18 — Testowanie pod kątem Server-Side Template Injection

Server-Side Template Injection (SSTI) występuje, gdy aplikacja osadza dane wejściowe kontrolowane przez użytkownika w silniku szablonów po stronie serwera bez odpowiedniej sanityzacji lub maskowania (escaping). Pozwala to atakującemu na wstrzyknięcie złośliwych dyrektyw szablonów, które są następnie wykonywane na serwerze, co potencjalnie prowadzi do pełnego przejęcia systemu poprzez zdalne wykonywanie kodu (RCE). SSTI zazwyczaj objawia się w funkcjach takich jak dynamiczne generowanie wiadomości e-mail, strony profilowe oraz systemy powiadomień, które korzystają z silników takich jak Jinja2, Twig lub FreeMarker. Z perspektywy atakującego, podatność ta jest często wykorzystywana do wycieku wrażliwych zmiennych środowiskowych, odczytu plików wewnętrznych lub wykonywania poleceń systemowych poprzez wykorzystanie natywnych obiektów i metod silnika szablonów.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Status testu** | Nieprzeprowadzony |
| **Krytyczność** | Krytyczny |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Narzędzia:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### Czy aplikacja wykorzystuje silnik szablonów po stronie serwera do przetwarzania danych wejściowych użytkownika?
- [ ] Nie — aplikacja **nie** używa szablonów po stronie serwera dla treści dynamicznych  
- [ ] Tak — szablony są używane, ale dane użytkownika **nie** są osadzane w dyrektywach szablonów  
- [ ] Tak — dane użytkownika są osadzane w szablonach **bez** odpowiedniej sanityzacji  

### Czy podstawowe wyrażenia arytmetyczne są obliczane po wstrzyknięciu do pól wejściowych?
- [ ] Nie — ładunki (payloads) takie jak `{{7*7}}` lub `${7*7}` są odzwierciedlane jako dosłowne ciągi znaków  
- [ ] Tak — wyrażenia są obliczane (np. zwracany jest wynik `49`), co potwierdza, że silnik szablonów **jest podatny**  

### Czy istnieje możliwość uzyskania dostępu do wbudowanych obiektów lub metod silnika szablonów?
- [ ] Nie — dostęp do niebezpiecznych obiektów, metod lub konfiguracji jest **ograniczony** lub odizolowany (sandboxed)  
- [ ] Tak — wbudowane obiekty (np. `self`, `config`, `__class__`) **mogą** być dostępne w celu wycieku wrażliwych danych  
- [ ] Tak — zdalne wykonywanie kodu (RCE) poprzez wywołania systemowe lub biblioteki OS **jest możliwe**  

### Czy wdrożono mechanizm piaskownicy (sandbox) lub białej listy (allowlist) zapobiegający wykonywaniu niebezpiecznych dyrektyw?
- [ ] Tak — ścisła biała lista lub bezpieczna piaskownica jest wdrożona i jej obejście **nie jest możliwe**  
- [ ] Tak — piaskownica istnieje, ale jej obejście **jest możliwe** za pomocą specyficznych ładunków zależnych od silnika  
- [ ] Nie — **nie zastosowano** żadnej piaskownicy ani białej listy w silniku szablonów  

---