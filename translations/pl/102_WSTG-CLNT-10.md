## WSTG-CLNT-10 — Testowanie WebSockets

Testowanie WebSockets koncentruje się na pełnodupleksowym kanale komunikacyjnym ustanowionym między klientem a serwerem, który pomija tradycyjne wzorce żądanie-odpowiedź protokołu HTTP. Pentesterzy oceniają bezpieczeństwo procesu Handshake, obecność mechanizmów ochrony przed Cross-Site WebSocket Hijacking (CSWSH) oraz rygorystyczność walidacji danych wejściowych (Input Validation) stosowanej wobec Payloadów wiadomości. Eksploatacja (Exploitation) zazwyczaj polega na przejęciu aktywnej sesji w celu eksfiltracji danych w czasie rzeczywistym lub wstrzykiwaniu złośliwych Payloadów, które wyzwalają luki po stronie serwera, takie jak Command Injection lub SQLi poprzez trwałe gniazdo (socket). Ponieważ wiele tradycyjnych zapór aplikacji internetowych (WAF) nie inspekcjonuje skutecznie ruchu WebSocket, protokół ten często stanowi dyskretny wektor do omijania zabezpieczeń obwodowych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Wysoki / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Narzędzia:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### Czy połączenie WebSocket jest ustanawiane przez bezpieczny kanał?
- [ ] Tak — `wss://` (WebSocket Secure) jest wymuszane dla wszystkich połączeń  
- [ ] Nie — używane jest `ws://`, co pozwala na podsłuch typu person-in-the-middle (PITM)  

### Czy aplikacja jest chroniona przed Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] Nie — serwer rygorystycznie waliduje nagłówek `Origin` podczas procesu Handshake  
- [ ] Tak — walidacja nagłówka `Origin` jest słaba lub **może** zostać pominięta poprzez wartości null lub sfałszowane źródła  
- [ ] Tak — nie jest wykonywana żadna walidacja nagłówka `Origin`, co pozwala atakującym cross-site na zainicjowanie połączenia *(Krytyczne)*  

### Czy walidacja i sanityzacja danych wejściowych (Input Validation/Sanitization) są stosowane wobec Payloadów wiadomości WebSocket?
- [ ] Tak — wszystkie wiadomości przychodzące są poddawane sanityzacji i walidowane względem schematu  
- [ ] Tak — istnieje pewna walidacja, ale jej obejście (bypass) **jest możliwe** przy użyciu zakodowanych lub niestandardowych Payloadów  
- [ ] Nie — wiadomości są przetwarzane bezpośrednio, co pozwala na ataki typu Injection (XSS, SQLi, RCE) lub manipulację logiką  

### Czy kontrole uwierzytelniania (Authentication) i autoryzacji (Authorization) są wykonywane przy każdej wiadomości WebSocket?
- [ ] Tak — sesja jest weryfikowana dla każdej wiadomości lub protokół jest bezstanowy (stateless) z użyciem tokenów  
- [ ] Tak — uwierzytelnianie odbywa się podczas Handshake, ale autoryzacja dla konkretnych działań **nie jest** wymuszana dla każdej wiadomości  
- [ ] Nie — uwierzytelnianie jest sprawdzane tylko podczas Handshake, co pozwala na przejęcie sesji w celu uzyskania pełnego dostępu  

### Czy serwer implementuje Rate Limiting lub ograniczenia zasobów dla protokołu WebSocket?
- [ ] Tak — Rate Limiting i maksymalne rozmiary wiadomości są **włączone** i wymuszane  
- [ ] Nie — nie istnieją żadne limity, co pozwala na atak Denial of Service (DoS) poprzez zalewanie wiadomościami (message flooding) lub duże rozmiary Payloadów  

---