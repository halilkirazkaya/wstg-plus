## WSTG-INPV-05 — SQL Injection

SQL Injection występuje, gdy dane wejściowe dostarczone przez użytkownika są włączane do zapytań SQL bez odpowiedniej parametryzacji (parameterization) lub oczyszczania (sanitization), co pozwala atakującym na manipulowanie logiką zapytania. Udana eksploatacja (exploitation) może prowadzić do nieautoryzowanej eksfiltracji danych (data exfiltration), obejścia uwierzytelniania (authentication bypass), modyfikacji danych, a w niektórych przypadkach do zdalnego wykonywania kodu (remote code execution) poprzez funkcje bazy danych, takie jak `xp_cmdshell` lub `LOAD_FILE()`. Podatność ta powszechnie występuje w formularzach logowania, funkcjach wyszukiwania, parametrach REST API, nagłówkach HTTP oraz we wszelkich punktach końcowych (endpoints), które tworzą dynamiczne zapytania SQL na podstawie danych kontrolowanych przez użytkownika. Z perspektywy atakującego, zidentyfikowanie punktu iniekcji (injection point) stanowi główną bramę do pełnego przejęcia bazy danych i potencjalnego poruszania się bocznego (lateral movement) wewnątrz infrastruktury.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Status testu** | Nie wykonano |
| **Krytyczność** | Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Narzędzia:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Czy parametry kontrolowane przez użytkownika są przetwarzane przy użyciu bezpiecznych metod dostępu do bazy danych?
- [ ] Tak — aplikacja używa wyłącznie ORM lub zapytań parametryzowanych (parameterized queries) i ich obejście **nie jest możliwe**  
- [ ] Tak — używane są zapytania parametryzowane, ale obejście **jest możliwe** w przypadkach brzegowych (np. klauzule `ORDER BY`)  
- [ ] Nie — do budowania zapytań SQL używana jest bezpośrednia konkatenacja ciągów znaków (raw string concatenation) **bez** parametryzacji  

### Czy mechanizm uwierzytelniania może zostać pominięty poprzez SQL Injection?
- [ ] Nie — parametry logowania **nie są** podatne na iniekcję  
- [ ] Tak — obejście uwierzytelniania (authentication bypass) **jest możliwe** przy użyciu ładunków (payloads) opartych na tautologii (np. `' OR 1=1 --`)  

### Czy możliwa jest eksfiltracja danych za pomocą technik in-band lub out-of-band?
- [ ] Nie — nie zidentyfikowano eksfiltracji danych  
- [ ] Tak — eksfiltracja UNION-based lub error-based **jest możliwa**  
- [ ] Tak — eksfiltracja typu blind (Boolean lub Time-based) **jest możliwa**  
- [ ] Tak — eksfiltracja out-of-band (DNS/HTTP) **jest możliwa**  

### Czy funkcje aplikacji wykazują podatności na SQL Injection drugiego stopnia (second-order)?
- [ ] Nie — przechowywane dane są bezpiecznie obsługiwane podczas ich pobierania do późniejszych zapytań  
- [ ] Tak — dane wejściowe użytkownika są przechowywane, a następnie używane w niebezpiecznym zapytaniu SQL **bez** walidacji  

### Czy uprawnienia konta serwisowego bazy danych są ograniczone do niezbędnego minimum?
- [ ] Tak — konto serwisowe posiada **najniższe uprawnienia** (least privilege) (np. ograniczone do konkretnych tabel/działań)  
- [ ] Nie — konto serwisowe posiada podwyższone uprawnienia (np. uprawnienia `DROP TABLE`, `FILE` lub **włączoną** funkcję `xp_cmdshell`)  

---