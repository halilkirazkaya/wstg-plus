## WSTG-CRYP-02 — Testowanie podatności na Padding Oracle

Podatności typu Padding Oracle występują, gdy proces deszyfrowania aplikacji ujawnia informacje o tym, czy dopełnienie (padding) odszyfrowanego tekstu zaszyfrowanego (ciphertext) jest prawidłowe czy nie. Poprzez obserwację tych odpowiedzi kanałem bocznym (side-channel) — takich jak odrębne kody statusu HTTP, specyficzne komunikaty o błędach lub różnice w czasie odpowiedzi — atakujący może odszyfrować dane bez znajomości klucza szyfrującego i potencjalnie ponownie zaszyfrować dowolny tekst jawny (plaintext). Podatność ta zazwyczaj objawia się w zaszyfrowanych plikach cookie sesji, tokenach uwierzytelniających lub parametrach URL wykorzystujących szyfry blokowe w trybie Cipher Block Chaining (CBC) z dopełnieniem PKCS#7. Eksploatacja pozwala na całkowitą utratę poufności i może prowadzić do naruszenia integralności poprzez konstruowanie sfałszowanych bloków zaszyfrowanych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Wysoki / Krytyczny* |

> *Poziom krytyczności jest krytyczny, jeśli podatność występuje w zarządzaniu sesją, tokenach tożsamości lub szyfrowaniu danych osobowych (PII).

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Czy szyfr blokowy w trybie CBC jest używany dla wrażliwych danych?
- [ ] Nie — aplikacja używa uwierzytelnionego szyfrowania (AES-GCM/ChaCha20-Poly1305)  
- [ ] Tak — aplikacja używa szyfrów blokowych, ale dopełnienie (padding) **nie jest** wymagane (np. szyfry strumieniowe lub tryb CTR)  
- [ ] Tak — aplikacja używa trybu CBC i stosowane jest dopełnienie (**padding**)  

### Czy serwer ujawnia poprawność dopełnienia poprzez kanały boczne?
- [ ] Nie — serwer zwraca jednolite komunikaty o błędach i spójne czasy odpowiedzi  
- [ ] Tak — serwer zwraca różne kody statusu HTTP (np. 200 vs 500) dla błędów dopełnienia  
- [ ] Tak — serwer zwraca specyficzne komunikaty o błędach (np. "Invalid Padding", "Decryption failed")  
- [ ] Tak — serwer wykazuje mierzalne różnice czasowe podczas niepowodzenia deszyfrowania  

### Czy możliwa jest automatyczna deszyfracja tekstu zaszyfrowanego?
- [ ] Nie — kanały boczne **nie są** wystarczająco stabilne do eksploatacji  
- [ ] Tak — tekst zaszyfrowany (ciphertext) **może** zostać częściowo odszyfrowany (ostatnie kilka bloków)  
- [ ] Tak — pełne odzyskanie tekstu jawnego (plaintext) **jest możliwe** przy użyciu narzędzi takich jak `PadBuster`  

### Czy atakujący może wykonać atak bit-flipping lub sfałszować poprawne teksty zaszyfrowane?
- [ ] Nie — mechanizmy kontroli integralności (HMAC) są weryfikowane **przed** deszyfrowaniem  
- [ ] Tak — brak kontroli integralności, ale konstrukcja ładunku (Payload) **nie jest** wykonalna  
- [ ] Tak — konstrukcja dowolnego tekstu zaszyfrowanego **jest możliwa**, co pozwala na eskalację uprawnień (Privilege Escalation) lub wstrzykiwanie danych (Data Injection)  

---