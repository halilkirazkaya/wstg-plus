## WSTG-CRYP-03 — Testowanie pod kątem przesyłania wrażliwych informacji kanałami nieszyfrowanymi

Przesyłanie wrażliwych danych kanałami nieszyfrowanymi, takimi jak zwykły protokół HTTP, naraża informacje na przechwycenie i modyfikację poprzez ataki typu Man-in-the-Middle (MITM). Ta podatność zazwyczaj występuje, gdy aplikacje webowe nie wymuszają protokołu Transport Layer Security (TLS) na wszystkich punktach końcowych (endpoints), szczególnie w starszych komponentach, formularzach logowania lub podczas początkowej fazy przejścia z HTTP na HTTPS. Z perspektywy atakującego pozwala to na pasywny sniffing poświadczeń uwierzytelniających, tokenów sesyjnych oraz danych osobowych (Personally Identifiable Information - PII) w przejętych lub niezaufanych segmentach sieci, takich jak publiczne sieci Wi-Fi. Zapewnienie, że wszystkie wrażliwe dane są zamknięte wewnątrz solidnego tunelu TLS, jest kluczowe dla zachowania poufności i integralności interakcji użytkownika oraz zapobiegania przejęciom sesji (session hijacking).


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Wysoki / Średni |

> *Poziom zagrożenia staje się Wysoki, jeśli poświadczenia uwierzytelniające, identyfikatory sesji lub PII są przesyłane tekstem jawnym (cleartext).

**Odnośniki:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### Czy aplikacja zezwala na komunikację przez nieszyfrowany protokół HTTP dla wrażliwych punktów końcowych?
- [ ] Nie — cały ruch aplikacji jest rygorystycznie wymuszany przez HTTPS *(Najbardziej bezpieczne)*  
- [ ] Tak — strony niewrażliwe są dostępne przez HTTP, ale wrażliwe punkty końcowe **nie są**  
- [ ] Tak — wrażliwe punkty końcowe (logowanie, koszyk, profil) **są** dostępne przez nieszyfrowany protokół HTTP *(Wysokie ryzyko)*  

### Czy zaimplementowano globalne przekierowanie z HTTP na HTTPS?
- [ ] Tak — aplikacja wykonuje przekierowanie 301/302 do HTTPS dla wszystkich żądań HTTP  
- [ ] Tak — przekierowanie jest wykonywane tylko dla konkretnych wrażliwych punktów końcowych  
- [ ] Nie — żadne przekierowanie z HTTP na HTTPS **nie jest stosowane**  

### Czy zaimplementowano HTTP Strict Transport Security (HSTS)?
- [ ] Tak — HSTS jest **włączony** z długim parametrem `max-age` oraz zawiera dyrektywy `includeSubDomains` i `preload`  
- [ ] Tak — HSTS jest **włączony**, ale brakuje dyrektyw `includeSubDomains` lub `preload`  
- [ ] Nie — HSTS **nie jest włączony** na serwerze aplikacji  

### Czy wrażliwe pliki cookie są chronione przed transmisją tekstem jawnym?

| Nazwa pliku cookie | Obecna flaga Secure | Brak flagi Secure |
|--------------------|:-------------------:|:-----------------:|
| `SessionID`        |                     |                   |
| `AuthToken`        |                     |                   |
| `CSRF-Token`       |                     |                   |

### Czy aplikacja przesyła wrażliwe dane do zewnętrznych API kanałami nieszyfrowanymi?
- [ ] Nie — wszystkie zewnętrzne wywołania API są wykonywane przez szyfrowane tunele (HTTPS)  
- [ ] Tak — niektóre niewrażliwe dane telemetryczne są wysyłane przez HTTP  
- [ ] Tak — klucze API, PII lub poświadczenia **są** wysyłane do podmiotów zewnętrznych przez nieszyfrowany protokół HTTP  

---