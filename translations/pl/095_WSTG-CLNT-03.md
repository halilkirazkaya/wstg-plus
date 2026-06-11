## WSTG-CLNT-03 — HTML Injection

HTML Injection występuje, gdy aplikacja nie przeprowadza sanityzacji danych wejściowych dostarczonych przez użytkownika przed ich wyrenderowaniem w przeglądarce, co pozwala atakującemu na wstrzyknięcie dowolnych tagów HTML do modelu obiektowego dokumentu (DOM) strony internetowej. Choć podatność ta jest podobna do Cross-Site Scripting (XSS), głównym celem HTML Injection jest manipulacja wizualną prezentacją strony lub ułatwienie ataków typu phishing poprzez wstrzykiwanie złośliwych formularzy i zwodniczych treści. Atakujący celują w parametry, które są odzwierciedlane w UI, takie jak frazy wyszukiwania, pola profilu użytkownika lub komunikaty o błędach, aby skłonić ofiary do ujawnienia poufnych danych uwierzytelniających (credentials) lub interakcji z nieautoryzowanymi linkami zewnętrznymi. Podatność ta stanowi istotne zagrożenie dla integralności interfejsu użytkownika aplikacji i może być wstępem do bardziej złożonych ataków socjotechnicznych lub związanych z sesją.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Niski / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Czy dane wejściowe kontrolowane przez użytkownika są odzwierciedlane w DOM bez odpowiedniego kodowania HTML?
- [ ] Nie — wszystkie odzwierciedlone dane są ściśle kodowane (HTML-encoded) przed wyrenderowaniem  
- [ ] Tak — niektóre znaki **są** kodowane, ale możliwe są **obejścia (bypasses)** dla określonych tagów  
- [ ] Tak — dane wejściowe są odzwierciedlane w formie surowej i HTML injection **jest możliwe**  

### Czy można wstrzyknąć strukturalne elementy HTML w celu zmiany układu strony?
- [ ] Nie — aplikacja filtruje lub usuwa tagi strukturalne, takie jak `<div>`, `<table>` lub `<iframe>`  
- [ ] Tak — elementy strukturalne **mogą** zostać wstrzyknięte, ale **nie mają** znaczącego wpływu na UI  
- [ ] Tak — znaczące zniekształcenie interfejsu (UI defacement) **jest możliwe** poprzez wstrzyknięte tagi  

### Czy możliwe jest wstrzyknięcie funkcjonalnych elementów phishingowych, takich jak formularze lub złośliwe linki?
- [ ] Nie — tagi `<form>`, `<input>` oraz `<a>` są skutecznie blokowane lub poddawane sanityzacji  
- [ ] Tak — złośliwe linki **mogą** zostać wstrzyknięte, aby przekierowywać użytkowników do zewnętrznych witryn  
- [ ] Tak — funkcjonalne elementy `<form>` **mogą** zostać wstrzyknięte w celu wyłudzania poświadczeń *(Średni)*  

### Czy nagłówki bezpieczeństwa lub Content Security Policy (CSP) ograniczają skutki wstrzyknięcia?
- [ ] Tak — wdrożono restrykcyjne CSP, a reguły `form-action` lub `frame-src` **są stosowane**  
- [ ] Nie — nagłówki bezpieczeństwa są obecne, ale CSP **jest wyłączone** lub zawiera wpisy unsafe-inline/wildcards  
- [ ] Nie — żadne CSP ani odpowiednie nagłówki bezpieczeństwa **nie są włączone**  

---