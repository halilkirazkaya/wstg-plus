## WSTG-CLNT-11 — Test Web Messaging

Web Messaging, lub `postMessage`, umożliwia komunikację między różnymi pochodzeniami (cross-origin) pomiędzy obiektami okien, takimi jak strona nadrzędna a wywołane okno popup lub osadzona ramka iframe. Mechanizm ten jest kluczowy dla funkcjonowania nowoczesnych stron internetowych, ale wprowadza istotne ryzyko, jeśli aplikacja odbierająca komunikat nie zweryfikuje tożsamości nadawcy lub niewłaściwie obsłuży ładunek (payload) wiadomości. Atakujący wykorzystują te podatności, hostując złośliwą stronę, która osadza docelową aplikację i wysyła spreparowane komunikaty w celu wywołania niepożądanych działań, eksfiltracji wrażliwych danych lub wykonania ataku DOM-based Cross-Site Scripting (XSS). Skuteczna eksploatacja występuje, gdy procedury obsługi komunikatów (message listeners) nie walidują rygorystycznie właściwości `origin` lub przekazują dane `message.data` bezpośrednio do niebezpiecznych ujść (execution sinks).

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności (Severity)** | Średni / Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Narzędzia:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### Czy aplikacja wykorzystuje API `postMessage` do komunikacji między dokumentami?
- [ ] Nie — procedury obsługi (listeners) `postMessage` **nie** są obecne w kodzie po stronie klienta  
- [ ] Tak — `postMessage` jest używany do wewnętrznej komunikacji między komponentami  
- [ ] Tak — `postMessage` jest używany do komunikacji z domenami zewnętrznymi (third-party) lub widgetami  

### Czy pochodzenie (origin) komunikatów przychodzących jest ściśle walidowane?
- [ ] Tak — origin jest sprawdzany względem ścisłej białej listy (whitelist) przy użyciu operatora `===` lub porównania identycznego *(Najbezpieczniejsze)*  
- [ ] Tak — origin jest walidowany za pomocą wyrażenia regularnego (regex), ale logika **jest** podatna na obejście (np. `^https://trusted.com`)  
- [ ] Nie — właściwość `origin` **nie** jest sprawdzana, co pozwala dowolnej domenie na wysyłanie komunikatów  
- [ ] Nie — użyto symbolu wieloznacznego (wildcard) `*` w `targetOrigin` nadawcy, co potencjalnie ujawnia dane złośliwym odbiorcom  

### Czy ładunek (payload) komunikatu jest sanityzowany przed przetworzeniem przez aplikację?
- [ ] Tak — dane są traktowane jako zwykły tekst i nie są używane żadne niebezpieczne ujścia (sinks)  
- [ ] Tak — dane są parsowane (np. `JSON.parse`) i walidowane przed użyciem, a obejście walidacji **nie jest możliwe**  
- [ ] Nie — dane komunikatu są przekazywane bezpośrednio do niebezpiecznych ujść (sinks), takich jak `innerHTML`, `eval()` lub `setTimeout()`  
- [ ] Nie — dane komunikatu są używane do nawigacji w oknie lub zmiany `location.href` bez uprzedniej walidacji  

### Czy atakujący może wywołać nieautoryzowane działania lub dokonać eksfiltracji danych poprzez złośliwy komunikat?
- [ ] Nie — nawet przy sfałszowanym komunikacie nie można uzyskać dostępu do wrażliwych działań ani danych  
- [ ] Tak — spreparowany komunikat **może** wywołać zmiany stanu wrażliwych danych (np. zmiana hasła, aktualizacja profilu)  
- [ ] Tak — spreparowany komunikat **może** skutkować atakiem DOM-based XSS lub eksfiltracją tokenów CSRF/danych sesyjnych  

---