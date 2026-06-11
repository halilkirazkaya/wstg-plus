## WSTG-APIT-02 — API Broken Object Level Authorization

Broken Object Level Authorization (BOLA), znane również jako Insecure Direct Object Reference (IDOR), występuje, gdy API nie przeprowadza walidacji, czy użytkownik posiada odpowiednie uprawnienia do uzyskania dostępu lub manipulacji konkretnym zasobem zidentyfikowanym przez identyfikator (ID). Atakujący wykorzystują tę podatność poprzez systematyczne enumerowanie lub zgadywanie identyfikatorów — takich jak numeryczne ID lub UUID — w ścieżkach żądań, parametrach zapytania lub strukturach JSON, aby uzyskać dostęp do danych należących do innych użytkowników. Luka ta jest najczęstszym i najbardziej dotkliwym problemem w nowoczesnym bezpieczeństwie API, mogącym prowadzić do masowej eksfiltracji danych (Exfiltration), nieautoryzowanej modyfikacji lub całkowitego przejęcia konta. Z perspektywy atakującego celem jest identyfikacja punktów końcowych (endpoints), które przyjmują identyfikatory obiektów, oraz przetestowanie, czy logika autoryzacji po stronie serwera jest nieobecna lub czy można ją pominąć poprzez manipulację tymi identyfikatorami.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Narzędzia:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Czy identyfikatory obiektów w żądaniach API są przewidywalne lub możliwe do wyliczenia (enumerable)?
- [ ] Nie — identyfikatory są długie, losowe i zabezpieczone kryptograficznie (np. UUIDv4)  
- [ ] Tak — identyfikatory są kolejnymi liczbami całkowitymi (np. `101`, `102`)  
- [ ] Tak — identyfikatory podążają za przewidywalnym wzorcem lub są oparte na publicznie dostępnych informacjach  

### Czy API przeprowadza walidację własności obiektu po stronie serwera dla każdego żądania?
- [ ] Tak — weryfikacja autoryzacji jest stosowana przy każdym żądaniu i jej obejście (bypass) jest **niemożliwe**  
- [ ] Tak — weryfikacja jest stosowana, ale jej obejście **jest możliwe** poprzez Parameter Pollution lub Method Tunneling  
- [ ] Nie — aplikacja polega wyłącznie na fakcie uwierzytelnienia użytkownika, nie sprawdzając własności konkretnego zasobu  

### Czy możliwy jest dostęp lub modyfikacja zasobu innego użytkownika poprzez zmianę identyfikatora?
- [ ] Nie — nieautoryzowany dostęp do zasobów innych użytkowników jest **niemożliwy**  
- [ ] Tak — nieautoryzowany dostęp do **odczytu** (Horyzontalne BOLA) **jest możliwy**  
- [ ] Tak — nieautoryzowana **modyfikacja** lub **usunięcie** (Horyzontalne BOLA) **jest możliwe**  

### Czy API pozwala na dostęp do obiektów administracyjnych lub systemowych poprzez podstawienie identyfikatorów?
- [ ] Nie — zasoby administracyjne są chronione przez dodatkowe warstwy autoryzacji  
- [ ] Tak — uzyskanie dostępu lub modyfikacja zasobów na poziomie systemowym (Wertykalne BOLA) **jest możliwe**  

### Czy mechanizm autoryzacji można pominąć, przenosząc identyfikator do innej części żądania?
- [ ] Nie — logika autoryzacji jest spójna niezależnie od lokalizacji parametru  
- [ ] Tak — obejście **jest możliwe**, gdy ID zostanie przeniesione ze ścieżki URL do treści JSON lub nagłówków  
- [ ] Tak — obejście **jest możliwe**, gdy podanych zostanie wiele identyfikatorów, a serwer przetwarza ten nieautoryzowany  

---