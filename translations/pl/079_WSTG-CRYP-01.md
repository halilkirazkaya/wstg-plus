## WSTG-CRYP-01 — Testowanie pod kątem słabych zabezpieczeń warstwy transportowej

Testowanie pod kątem słabych zabezpieczeń warstwy transportowej obejmuje analizę implementacji i konfiguracji SSL/TLS w celu zapewnienia poufności i integralności danych podczas transmisji. Atakujący celują w błędy konfiguracji, takie jak przestarzałe protokoły (SSLv2, TLS 1.0), słabe zestawy szyfrów (NULL, RC4 lub anonimowe) oraz nieważne lub słabo podpisane certyfikaty, aby przeprowadzić ataki typu Man-in-the-Middle (MitM). Test ten zazwyczaj dotyczy punktów końcowych serwera WWW aplikacji lub systemów równoważenia obciążenia (load balancer), gdzie nawiązywana jest szyfrowana komunikacja. Skuteczna eksploatacja pozwala napastnikowi na przechwycenie wrażliwych danych, przejęcie sesji użytkownika lub obniżenie poziomu bezpieczeństwa połączenia do stanów niezabezpieczonych poprzez wymuszenie słabszego protokołu (downgrade) lub deszyfrację ruchu.


| Pole | Wartość |
|---|---|
| **ID WSTG** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Wysoki / Średni* |

> *Poziom istotności jest Wysoki, jeśli przesyłane są dane wrażliwe (PII, dane uwierzytelniające, tokeny sesyjne); Średni dla ogólnego ruchu informacyjnego.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Czy obsługiwane są przestarzałe lub niebezpieczne protokoły TLS/SSL?
- [ ] Nie — włączone są wyłącznie TLS 1.2 i TLS 1.3  
- [ ] Tak — włączone są TLS 1.0 lub TLS 1.1  
- [ ] Tak — włączone są SSLv2 lub SSLv3 *(Krytyczne)*  

### Czy serwer akceptuje słabe lub niebezpieczne zestawy szyfrów?
- [ ] Nie — obsługiwane są tylko silne, nowoczesne szyfry (np. AES-GCM, ChaCha20)  
- [ ] Tak — obsługiwane są szyfry ze słabym szyfrowaniem (np. 3DES, RC4) lub o niskiej długości klucza  
- [ ] Tak — obsługiwane są szyfry NULL, anonimowe (ADH) lub eksportowe *(Krytyczne)*  

### Czy certyfikat serwera jest ważny i zaufany?
- [ ] Tak — certyfikat jest ważny, pasuje do domeny i jest podpisany przez zaufany urząd certyfikacji (CA)  
- [ ] Nie — certyfikat wygasł lub używa słabego algorytmu podpisu (np. SHA-1)  
- [ ] Nie — certyfikat jest podpisany samodzielnie (self-signed) lub występuje niezgodność nazwy domeny (mismatch)  

### Czy serwer obsługuje Secure Renegotiation i zapobiega atakom typu Downgrade?
- [ ] Tak — Secure Renegotiation jest obsługiwana i mechanizm TLS Fallback SCSV jest włączony  
- [ ] Nie — Secure Renegotiation nie jest obsługiwana, co potencjalnie umożliwia wstrzyknięcie danych typu MitM  
- [ ] Nie — serwer jest podatny na ataki POODLE lub ROBOT  

### Czy nagłówek HTTP Strict Transport Security (HSTS) jest prawidłowo zaimplementowany?

| Nagłówek | Obecny | Brak |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Tak — nagłówek HSTS jest obecny z długim czasem `max-age` i włączoną opcją `includeSubDomains`  
- [ ] Tak — nagłówek HSTS jest obecny, ale brakuje opcji `includeSubDomains` lub ma on krótki `max-age`  
- [ ] Nie — nagłówek HSTS nie jest obecny  

---