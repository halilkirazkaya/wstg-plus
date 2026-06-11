## WSTG-ATHN-01 — Testowanie przesyłania poświadczeń przez kanał szyfrowany

Testowanie przesyłania poświadczeń przez kanał szyfrowany ma na celu upewnienie się, że wrażliwe dane uwierzytelniające, takie jak nazwy użytkowników, hasła i tokeny sesyjne, są chronione przed przechwyceniem podczas transmisji. Atakujący znajdujący się w sieci — na przykład poprzez ataki Man-in-the-Middle (MitM) w publicznych sieciach Wi-Fi lub na przejętych sieciach wewnętrznych — mogą używać narzędzi do sniffing pakietów w celu przechwycenia poświadczeń przesyłanych otwartym tekstem, jeśli protokół TLS/SSL nie jest prawidłowo wymuszony. Podatność ta występuje zazwyczaj w punktach końcowych (endpoints) logowania, formularzach resetowania hasła oraz nagłówkach uwierzytelniania API, w których aplikacja nie wymusza HTTPS lub nieprawidłowo przekierowuje z HTTP. Udana eksploatacja zapewnia atakującemu pełny dostęp do kont użytkowników, prowadząc do całkowitego naruszenia danych użytkownika i potencjalnego ruchu bocznego (Lateral Movement) wewnątrz aplikacji.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Narzędzia:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Czy protokół HTTPS jest wymuszany w całym procesie uwierzytelniania?
- [ ] Tak — HSTS jest **włączony**, a cały ruch jest rygorystycznie wymuszany przez HTTPS  
- [ ] Tak — następuje przekierowanie do HTTPS, ale HSTS **nie jest włączony**  
- [ ] Nie — poświadczenia **mogą** być przesyłane przez nieszyfrowany protokół HTTP  

### Czy strony logowania i formularze są serwowane przez kanał szyfrowany?
- [ ] Tak — strona zawierająca formularz logowania jest dostarczana przez HTTPS  
- [ ] Nie — strona logowania jest serwowana przez HTTP, co pozwala na przejęcie akcji formularza (form-action hijacking) lub sniffing poświadczeń  

### Czy flaga `Secure` jest stosowana do wszystkich wrażliwych plików cookie sesji?
- [ ] Tak — flaga `Secure` jest **stosowana** do wszystkich plików cookie uwierzytelniania i sesji  
- [ ] Tak — flaga `Secure` jest **stosowana** tylko do niektórych plików cookie  
- [ ] Nie — flaga `Secure` **nie jest stosowana**, co pozwala na eksfiltrację plików cookie poprzez nieszyfrowane żądania  

### Czy serwer wspiera słabe wersje TLS lub niebezpieczne zestawy szyfrów?
- [ ] Nie — tylko nowoczesne wersje TLS (1.2+) i silne szyfry są **włączone**  
- [ ] Tak — starsze wersje (TLS 1.0/1.1) lub słabe szyfry (RC4, DES, CBC) są **obsługiwane**  

### Czy aplikacja zapobiega przesyłaniu poświadczeń do domen zewnętrznych przez kanały nieszyfrowane?
- [ ] Tak — żadne wrażliwe dane nie są wysyłane do domen zewnętrznych lub wszystkie połączenia zewnętrzne są wymuszane przez HTTPS  
- [ ] Nie — nagłówki uwierzytelniania lub poświadczenia są wysyłane do zewnętrznych punktów końcowych (np. analityka lub SSO) przez HTTP  

---