## WSTG-INFO-05 — Przegląd zawartości stron internetowych pod kątem wycieku informacji

Przegląd zawartości strony internetowej obejmuje manualną i zautomatyzowaną inspekcję odpowiedzi aplikacji, w tym plików HTML, JavaScript oraz CSS, w celu zidentyfikowania wrażliwych informacji nieumyślnie udostępnionych użytkownikom. Atakujący badają kod źródłowy po stronie klienta (client-side source code) pod kątem komentarzy deweloperskich, ukrytych pól formularzy, wewnętrznych ścieżek serwera, zahardkodowanych (hardcoded) kluczy API oraz informacji diagnostycznych (debugging information), które mogą pomóc w dalszych fazach eksploatacji. Wycieki te często występują, gdy deweloperzy zapomną usunąć metadane lub kod debugujący przed przeniesieniem aplikacji na środowisko produkcyjne, co potencjalnie ujawnia błędy logiczne lub szczegóły infrastruktury backendowej. Z perspektywy atakującego stanowi to niskonakładową metodę mapowania wewnętrznej struktury aplikacji i odkrywania pominiętych punktów wejścia (entry points) bez wyzwalania alertów bezpieczeństwa czy interakcji z logiką backendową serwera.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Niski / Średni* |

> *Poziom zagrożenia staje się wysoki (High), jeśli zostaną wykryte wrażliwe zahardkodowane poświadczenia, klucze prywatne lub wewnętrzne zmienne środowiskowe.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Narzędzia:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Czy w kodzie źródłowym znajdują się komentarze deweloperskie zawierające wrażliwe informacje?
- [ ] Nie — nie znaleziono komentarzy lub znaleziono tylko ogólne, niewrażliwe komentarze  
- [ ] Tak — komentarze istnieją, ale **nie** zawierają wrażliwej logiki ani danych  
- [ ] Tak — komentarze **ujawniają** wewnętrzne ścieżki, zapytania SQL lub instrukcje administracyjne  
- [ ] Tak — komentarze **ujawniają** poświadczenia lub wrażliwe notatki deweloperskie *(High)*  

### Czy ukryte pola wejściowe HTML zawierają wrażliwe dane lub informacje o stanie?
- [ ] Nie — nie znaleziono ukrytych pól lub zawierają one tylko niewrażliwe tokeny  
- [ ] Tak — ukryte pola są używane do utrzymania stanu, ale są **zaszyfrowane** lub **podpisane**  
- [ ] Tak — ukryte pola zawierają hasła w formacie **plaintext**, role użytkowników lub wewnętrzne identyfikatory  

### Czy w plikach JavaScript znajdują się zahardkodowane (hardcoded) sekrety, klucze API lub wewnętrzne adresy IP?
- [ ] Nie — w plikach JS nie znaleziono wrażliwych ciągów znaków ani sekretów  
- [ ] Tak — pliki JS zawierają publiczne klucze API z **odpowiednimi** ograniczeniami  
- [ ] Tak — pliki JS zawierają **niezabezpieczone** klucze API, wewnętrzne adresy IP lub prywatne punkty końcowe (endpoints)  
- [ ] Tak — pliki JS zawierają **zahardkodowane** poświadczenia administracyjne lub klucze prywatne *(Critical)*  

### Czy aplikacja ujawnia wersje frameworków lub wewnętrzne ścieżki do plików w kodzie źródłowym?
- [ ] Nie — informacje o frameworkach i ścieżki do plików są **zminifikowane** (minified) lub **zaciemnione** (obfuscated)  
- [ ] Tak — nazwy i wersje frameworków są **widoczne**, ale posiadają poprawki (patched)  
- [ ] Tak — wewnętrzne ścieżki do plików lub bezwzględne ścieżki serwera są **ujawnione** w kodzie źródłowym  

### Czy kod źródłowy jest podatny na wycieki z powodu braku minifikacji lub zaciemniania (obfuscation)?
- [ ] Nie — kod produkcyjny jest **w pełni zminifikowany** i pozbawiony komentarzy  
- [ ] Tak — kod jest zminifikowany, ale komentarze **pozostają** w źródle  
- [ ] Tak — mapy źródłowe (pliki `.map`) są **publicznie dostępne**, co pozwala na pełną rekonstrukcję kodu źródłowego  

---