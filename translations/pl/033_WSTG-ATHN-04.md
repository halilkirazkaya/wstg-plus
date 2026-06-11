## WSTG-ATHN-04 — Testowanie pod kątem obejścia schematu uwierzytelniania

Obejście schematu uwierzytelniania polega na identyfikacji i wykorzystaniu błędów w logice aplikacji, które pozwalają atakującemu na uzyskanie dostępu do chronionych zasobów bez podania poprawnych danych uwierzytelniających. Podatność ta występuje zazwyczaj, gdy aplikacja polega na kontrolach po stronie klienta (client-side), nie wymusza autoryzacji po stronie serwera przy każdym żądaniu lub pozostawia administracyjne „tylne furtki” (backdoory) i punkty końcowe debugowania (debug endpoints) dostępne w środowisku produkcyjnym. Atakujący wykorzystują te słabości poprzez wymuszone przeglądanie (forced browsing) wrażliwych adresów URL, manipulowanie parametrami HTTP, takimi jak `authenticated=true`, lub podszywanie się pod nagłówki (spoofing headers), aby oszukać aplikację i sprawić, by uznała sesję za już ustanowioną. Skuteczna eksploatacja skutkuje całkowitym przełamaniem obwodu uwierzytelniania, co potencjalnie prowadzi do nieautoryzowanego wycieku danych (data exfiltration) lub pełnego przejęcia systemu.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Status testu** | Nie wykonano |
| **Krytyczność** | Krytyczny |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Narzędzia:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Czy do wrażliwych stron wewnętrznych można uzyskać bezpośredni dostęp bez aktywnej sesji?
- [ ] Nie — bezpośredni dostęp skutkuje przekierowaniem do logowania lub odpowiedzią 403/404  
- [ ] Tak — niektóre wrażliwe strony są dostępne, ale dostarczają ograniczone lub niewrażliwe dane  
- [ ] Tak — wrażliwe strony administracyjne lub specyficzne dla użytkownika **są** w pełni dostępne bez uwierzytelnienia *(Krytyczny)*  

### Czy aplikacja polega na flagach lub parametrach po stronie klienta (client-side) w celu określenia statusu uwierzytelnienia?
- [ ] Nie — status uwierzytelnienia jest zarządzany wyłącznie poprzez stan sesji po stronie serwera  
- [ ] Tak — istnieją flagi po stronie klienta, ale ich modyfikacja **nie** przyznaje dostępu do obszarów chronionych  
- [ ] Tak — modyfikacja parametrów (np. `is_authenticated=true`, `user_role=admin`) **pozwala** na obejście uwierzytelniania  

### Czy uwierzytelnienie można obejść poprzez podszywanie się pod określone nagłówki HTTP lub ich manipulację?
- [ ] Nie — nagłówki takie jak `X-Forwarded-For`, `Referer` lub nagłówki niestandardowe nie mają wpływu na uwierzytelnianie  
- [ ] Tak — manipulowanie nagłówkami (np. `X-Custom-IP-Authorization`, `X-Remote-User`) **jest możliwe** i przyznaje nieautoryzowany dostęp  

### Czy istnieją alternatywne ścieżki lub artefakty programistyczne, które omijają standardowy przepływ logowania?
- [ ] Nie — wszystkie chronione zasoby są zabezpieczone przez scentralizowaną, gotową do użycia w produkcji kontrolę uwierzytelniania  
- [ ] Tak — przestarzałe punkty końcowe (legacy endpoints), trasy debugowania (np. `/debug/login`) lub zapomniane wersje API **są** dostępne bez poświadczeń  

### Czy aplikacja nie wymaga ponownego uwierzytelnienia lub weryfikacji sesji dla działań o wysokich uprawnieniach?
- [ ] Nie — działania o wysokich uprawnieniach lub wrażliwe wymagają świeżego lub ważnego tokenu sesji  
- [ ] Tak — po znalezieniu obejścia w jednym punkcie końcowym, **można** je wykorzystać do wykonywania działań administracyjnych w całej aplikacji  

---