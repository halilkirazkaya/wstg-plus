## WSTG-INPV-12 — Command Injection

Command Injection (wstrzykiwanie poleceń) występuje, gdy aplikacja przekazuje niebezpieczne dane dostarczone przez użytkownika, takie jak dane z formularzy, nagłówki HTTP lub pliki cookie, do powłoki systemowej (shell). Ta podatność pozwala atakującemu na wykonywanie dowolnych poleceń systemu operacyjnego na serwerze, zazwyczaj z uprawnieniami procesu aplikacji webowej. Z perspektywy atakującego, podatność ta jest wykorzystywana poprzez użycie metaznaków powłoki, takich jak średniki, potoki (pipes) lub backticki, w celu dołączenia złośliwych poleceń do zamierzonego przepływu wykonywania. Skutki są często katastrofalne i prowadzą do pełnego przejęcia systemu, nieautoryzowanej eksfiltracji danych lub ruchu bocznego (lateral movement) wewnątrz sieci wewnętrznej.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Status testu** | Nie wykonano |
| **Krytyczność** | Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Narzędzia:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### Czy aplikacja wywołuje polecenia na poziomie systemu lub pliki wykonywalne powłoki?
- [ ] Nie — aplikacja **nie** wchodzi w interakcję z powłoką systemu operacyjnego  
- [ ] Tak — aplikacja używa wewnętrznych API lub bibliotek wysokiego poziomu do zadań systemowych  
- [ ] Tak — aplikacja bezpośrednio wywołuje polecenia powłoki poprzez wywołania systemowe, takie jak `system()`, `exec()` lub `popen()`  

### Czy dane wejściowe użytkownika są walidowane lub sanityzowane przed przekazaniem do wywołań systemowych?
- [ ] Tak — stosowana jest ścisła biała lista (allow-listing) i walidacja danych wejściowych  
- [ ] Tak — stosowana jest sanityzacja lub filtrowanie znaków (escaping), ale możliwe jest obejście zabezpieczeń (bypass)  
- [ ] Nie — dane wejściowe użytkownika są przekazywane bezpośrednio do powłoki **bez** walidacji  

### Czy metaznaki powłoki mogą zostać użyte do manipulacji przepływem wykonywania poleceń?
- [ ] Nie — metaznaki są poprawnie filtrowane lub eskapowane  
- [ ] Tak — możliwe jest wykonanie typu in-band, gdzie wynik polecenia jest zwracany w odpowiedzi  
- [ ] Tak — możliwe jest wykonanie typu blind, gdzie wynik nie jest zwracany w odpowiedzi  

### Czy możliwa jest eksfiltracja danych poza pasmem (OOB) z hosta?
- [ ] Nie — serwer nie posiada połączeń wychodzących (egress) lub sygnały OOB (DNS/HTTP) zawodzą  
- [ ] Tak — eksfiltracja danych poprzez sygnały OOB oparte na DNS lub HTTP **jest możliwa**  

### Czy proces aplikacji działa z podwyższonymi uprawnieniami systemowymi?
- [ ] Nie — aplikacja działa na dedykowanym koncie usługowym o niskich uprawnieniach  
- [ ] Tak — aplikacja działa jako `root`, `Administrator` lub inny użytkownik o wysokich uprawnieniach *(Krytyczne)*  

---