## WSTG-SESS-10 — Testowanie JSON Web Tokens

Tokeny JSON Web Tokens (JWT) są powszechnie stosowane do bezstanowego zarządzania sesjami i propagacji tożsamości, jednak ich bezpieczeństwo zależy całkowicie od poprawnej implementacji algorytmów podpisu oraz rygorystycznej weryfikacji roszczeń (claims). Atakujący często próbują obejść uwierzytelnianie, wykorzystując błędy algorytmu „none”, łamiąc metodą Brute Force słabe klucze tajne HS256 lub przeprowadzając ataki typu algorithm-switching, w których asymetryczny klucz publiczny jest używany jako symetryczny sekret HMAC. Podatności te zazwyczaj występują w ciasteczkach sesyjnych lub nagłówkach Authorization, potencjalnie umożliwiając atakującemu eskalację uprawnień lub podszycie się pod dowolnego użytkownika poprzez modyfikację roszczeń, takich jak `role` lub `user_id`, bez naruszania kryptograficznej spójności tokena.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Materiały źródłowe:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Narzędzia:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### Czy serwer akceptuje algorytm „none”?
- [ ] Nie — serwer **odrzuca** tokeny używające `alg: none` w nagłówku  
- [ ] Tak — serwer **akceptuje** tokeny z `alg: none` i pustym podpisem *(Krytyczny)*  

### W jaki sposób weryfikowany jest podpis JWT i jak jest chroniony przed atakami Brute Force?
- [ ] Tak — używane są silne klucze asymetryczne (RS256/ES256) lub sekrety HMAC o wysokiej entropii  
- [ ] Tak — używany jest algorytm HS256, ale sekret **może** zostać złamany metodą Brute Force ze względu na niską entropię lub wartości domyślne  
- [ ] Nie — weryfikacja podpisu **nie jest stosowana** lub **może** zostać pominięta poprzez atak typu algorithm switching (z RS256 na HS256)  

### Czy standardowe roszczenia (exp, nbf, iat) są rygorystycznie egzekwowane przez backend?
- [ ] Tak — roszczenie `exp` (wygaśnięcie) jest obecne i **nie może** zostać pominięte  
- [ ] Tak — roszczenie `exp` jest obecne, ale serwer **nie egzekwuje** go podczas walidacji  
- [ ] Nie — roszczenia dotyczące wygaśnięcia są **nieobecne** lub ignorowane, co pozwala na bezterminowe ataki typu session replay  

### Czy implementacja zapobiega wstrzykiwaniu kluczy poprzez nagłówki (kid, jku, x5u)?
- [ ] Tak — nagłówki są sanityzowane i **dozwolone** są tylko zaufane wewnętrzne klucze/źródła  
- [ ] Nie — nagłówek `kid` (Key ID) jest podatny na directory traversal lub SQL Injection w celu wskazania na znany plik  
- [ ] Nie — nagłówki `jku` lub `x5u` **mogą** wskazywać na adresy URL kontrolowane przez atakującego w celu załadowania złośliwych kluczy  

---