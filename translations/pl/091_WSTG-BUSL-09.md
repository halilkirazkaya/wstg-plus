## WSTG-BUSL-09 — Testowanie przesyłania złośliwych plików

Funkcjonalność przesyłania plików pozwala użytkownikom na przesyłanie danych do serwera, co stanowi bezpośredni wektor wprowadzania złośliwej zawartości do środowiska aplikacji. Atakujący wykorzystują słabą logikę walidacji, aby przesyłać web shells, malware lub specjalnie spreparowane pliki — takie jak SVG lub HTML — w celu uzyskania Remote Code Execution (RCE) lub Cross-Site Scripting (XSS). Podatność ta występuje zazwyczaj w funkcjach takich jak ustawienia profilu, systemy zarządzania dokumentami oraz moduły obsługi załączników, gdzie serwer nie weryfikuje poprawnie typów plików, ich zawartości oraz uprawnień do wykonywania. Skuteczna eksploatacja często prowadzi do pełnego przejęcia systemu, eksfiltracji danych lub wykorzystania serwera jako punktu przeskoku (pivot) do ataków na sieć wewnętrzną.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Status testu** | Nie wykonano |
| **Poziom ważności** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### Czy aplikacja udostępnia funkcjonalność przesyłania plików?
- [ ] Nie — funkcjonalność przesyłania plików nie istnieje  
- [ ] Tak — funkcjonalność istnieje dla uwierzytelnionych użytkowników  
- [ ] Tak — funkcjonalność jest dostępna dla **nieuwierzytelnionych** użytkowników *(Krytyczne)*  

### Czy walidacja rozszerzeń plików jest wymuszana po stronie serwera?
- [ ] Tak — rygorystyczna biała lista (allowlist) rozszerzeń jest **stosowana** i jej obejście **nie jest możliwe**  
- [ ] Tak — biała lista jest stosowana, ale jej obejście **jest możliwe** poprzez manipulację wielkością liter lub podwójne rozszerzenia  
- [ ] Tak — stosowana jest czarna lista (blocklist), którą **można** obejść za pomocą alternatywnych rozszerzeń (np. `.phtml`, `.asa`)  
- [ ] Nie — żadna walidacja rozszerzeń nie jest **stosowana** po stronie serwera  

### Czy zawartość pliku jest walidowana poza samym rozszerzeniem lub typem MIME?
- [ ] Tak — aplikacja weryfikuje magic bytes i przeprowadza głęboką inspekcję zawartości  
- [ ] Tak — aplikacja sprawdza jedynie nagłówek `Content-Type`, który **można** łatwo sfałszować  
- [ ] Nie — aplikacja polega całkowicie na rozszerzeniu pliku w celu walidacji  

### Czy przesłane pliki są przechowywane w katalogu z uprawnieniami do wykonywania?
- [ ] Nie — pliki są przechowywane w dedykowanej usłudze (np. S3) lub w katalogu bez możliwości wykonywania  
- [ ] Tak — pliki są przechowywane na serwerze WWW, ale ich wykonywanie jest **wyłączone** poprzez konfigurację  
- [ ] Tak — pliki są przechowywane w katalogu dostępnym z poziomu WWW i ich wykonywanie **jest dozwolone** *(Krytyczne)*  

### Czy filtry bezpieczeństwa mogą zostać pominięte poprzez manipulację nazwą pliku?
- [ ] Nie — nazwy plików są oczyszczane (sanitized) lub losowo generowane przez serwer  
- [ ] Tak — filtry **mogą** zostać pominięte przy użyciu bajtów zerowych (np. `shell.php%00.jpg`)  
- [ ] Tak — znaki path traversal (np. `../../`) **mogą** być użyte do przesyłania plików do dowolnych katalogów  

---