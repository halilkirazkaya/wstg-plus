## WSTG-INPV-06 — Testowanie pod kątem LDAP Injection

LDAP Injection występuje, gdy aplikacja włącza dane dostarczone przez użytkownika do filtrów LDAP (Lightweight Directory Access Protocol) bez odpowiedniej sanityzacji lub escapingu, co pozwala atakującemu na manipulowanie logiką zapytania. Poprzez wstrzykiwanie znaków specjalnych, takich jak gwiazdki, nawiasy i operatory logiczne, atakujący mogą modyfikować filtr wyszukiwania, aby ominąć mechanizmy uwierzytelniania lub wyeksfiltrować wrażliwe informacje z katalogu, w tym nazwy użytkowników, przynależność do grup i atrybuty organizacyjne. Podatność ta zazwyczaj objawia się w funkcjach takich jak wyszukiwarki katalogów firmowych, portale pracownicze lub systemy Single Sign-On (SSO) współpracujące z Active Directory lub OpenLDAP. Z perspektywy atakującego, udana eksploatacja często obejmuje techniki typu blind w celu enumeracji struktury katalogu lub wartości atrybutów bit po bicie, gdy bezpośredni wynik zapytania jest tłumiony przez aplikację.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Narzędzia:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### Czy aplikacja wykorzystuje LDAP do uwierzytelniania lub przeszukiwania katalogów?
- [ ] Nie — LDAP **nie jest używany** w architekturze aplikacji  
- [ ] Tak — LDAP jest używany, a wszystkie dane wejściowe są rygorystycznie sanityzowane lub parametryzowane  
- [ ] Tak — LDAP jest używany, a dane użytkownika są łączone z filtrami **bez** odpowiedniego escapingu  

### Czy metaznaki LDAP są odpowiednio escapowane lub filtrowane w danych wejściowych użytkownika?
- [ ] Tak — znaki takie jak `(`, `)`, `&`, `|`, `*` oraz `\` są **niedozwolone** lub są poprawnie escapowane  
- [ ] Nie — metaznaki **mogą** zostać wstrzyknięte w celu zmiany struktury filtra LDAP  

### Czy mechanizm uwierzytelniania może zostać pominięty poprzez wstrzyknięcie (injection)?
- [ ] Nie — logika logowania **nie jest podatna** na manipulację zapytaniami  
- [ ] Tak — uwierzytelnianie **może** zostać pominięte przy użyciu logicznego OR (`|`) lub wstrzyknięcia symbolu wieloznacznego (`*`) w polach nazwy użytkownika lub hasła  

### Czy możliwa jest ślepa eksfiltracja danych (blind data exfiltration) z usługi katalogowej?
- [ ] Nie — wyniki wyszukiwania są ograniczone i nie zaobserwowano różnic czasowych ani logicznych (boolean)  
- [ ] Tak — atrybuty katalogu **mogą** być enumerowane bit po bicie za pomocą technik boolean-based blind injection  
- [ ] Tak — pełne rekordy katalogu **są** odzwierciedlone w odpowiedzi aplikacji z powodu manipulacji zapytaniem  

### Czy drugorzędne komponenty aplikacji (np. moduły pocztowe, SSO) są podatne na wstrzykiwanie atrybutów LDAP?
- [ ] Nie — atrybuty LDAP są bezpiecznie obsługiwane we wszystkich komponentach  
- [ ] Tak — atakujący **może** zmodyfikować własne atrybuty (np. e-mail, przynależność do grupy) poprzez wstrzyknięcie kodu, aby podnieść uprawnienia lub przekierować komunikację  

---