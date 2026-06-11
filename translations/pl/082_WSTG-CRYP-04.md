## WSTG-CRYP-04 — Testowanie pod kątem słabych prymitywów kryptograficznych

Słabe prymitywy kryptograficzne obejmują stosowanie przestarzałych lub niebezpiecznych algorytmów, takich jak MD5, SHA-1, DES lub RC4, które są podatne na ataki kolizyjne (collision attacks), analizę częstotliwościową lub deszyfrowanie metodą Brute Force. Słabości te występują zazwyczaj podczas haszowania haseł, szyfrowania danych w spoczynku (data encryption at rest), generowania tokenów sesyjnych lub weryfikacji podpisów cyfrowych w backendzie aplikacji. Z perspektywy atakującego, zidentyfikowanie tych prymitywów pozwala na naruszenie integralności i poufności danych, np. poprzez łamanie przechwyconych haszy w celu odzyskania haseł w formie tekstu jawnego lub fałszowanie tokenów uwierzytelniających. Nowoczesne aplikacje powinny zamiast tego wykorzystywać standardowe w branży, odporne na kolizje i charakteryzujące się wysoką entropią prymitywy, takie jak Argon2, Bcrypt lub AES-GCM, aby zapewnić solidną ochronę przed kryptoanalizą.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Status testu** | Nie wykonano |
| **Dotkliwość** | Wysoka |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Czy do przechowywania wrażliwych danych używane są przestarzałe algorytmy haszujące, takie jak MD5 lub SHA-1?
- [ ] Nie — aplikacja korzysta z nowoczesnych algorytmów odpornych na kolizje, takich jak Argon2 lub Bcrypt  
- [ ] Tak — przestarzałe algorytmy są używane, ale **wyłącznie** do kontroli integralności danych niewrażliwych z punktu widzenia bezpieczeństwa  
- [ ] Tak — przestarzałe algorytmy **są używane** do haseł lub wrażliwych tokenów *(Wysoka)*  

### Czy obecnie stosowane są słabe symetryczne szyfry blokowe, takie jak DES, 3DES lub RC4?
- [ ] Nie — używany jest algorytm AES (128-bitowy lub wyższy) w bezpiecznym trybie, takim jak GCM lub CBC  
- [ ] Tak — starsze szyfry są **włączone** w celu zapewnienia kompatybilności wstecznej, ale **nie są** domyślne  
- [ ] Tak — starsze szyfry są główną metodą ochrony danych w spoczynku lub w transmisji  

### Czy aplikacja implementuje własną („roll-your-own”) lub niestandardową logikę kryptograficzną?
- [ ] Nie — aplikacja ściśle przestrzega standardowych, recenzowanych bibliotek kryptograficznych  
- [ ] Tak — istnieją niestandardowe wrappery, ale bazowe prymitywy **są** standardem branżowym  
- [ ] Tak — do wrażliwych danych **stosowane są** autorskie algorytmy kryptograficzne lub niestandardowa logika *(Krytyczna)*  

### Czy sole kryptograficzne i wektory inicjujące (IV) są zarządzane zgodnie z najlepszymi praktykami?
- [ ] Tak — sole są unikalne dla każdego użytkownika, a IV są nieprzewidywalne dla każdej operacji  
- [ ] Tak — sole są używane, ale **nie są** unikalne dla wszystkich rekordów  
- [ ] Nie — sole są statyczne, zahardkodowane lub ich brakuje, a IV **są używane wielokrotnie** w różnych sesjach  

### Czy długość kluczy asymetrycznych (RSA, Diffie-Hellman) jest wystarczająca według nowoczesnych standardów?
- [ ] Tak — klucze RSA/DH mają 2048 bitów lub więcej, albo używana jest kryptografia krzywych eliptycznych (ECC)  
- [ ] Nie — klucze mają mniej niż 2048 bitów, ale obejście zabezpieczeń **nie jest** trywialne  
- [ ] Nie — klucze mają 1024 bity lub mniej, co czyni je **podatnymi** na faktoryzację lub ataki obliczeniowe  

---