## WSTG-SESS-01 — Testowanie schematu zarządzania sesją

Testowanie schematu zarządzania sesją koncentruje się na analizie sposobu, w jaki aplikacja obsługuje cykl życia i strukturę identyfikatorów sesji, aby upewnić się, że są one odporne na przewidywanie i przejęcie (hijacking). Atakujący przechwytują wiele tokenów sesji w celu przeprowadzenia analizy statystycznej, szukając wzorców, niskiej entropii (entropy) lub przewidywalnych przyrostów, które pozwalają na podrobienie (forge) ważnych sesji innych użytkowników. Słabości w schemacie często objawiają się jako podatności typu Session Fixation lub użycie niewystarczająco losowych wartości, zazwyczaj spotykanych w ciasteczkach HTTP (cookies), parametrach URL lub niestandardowych implementacjach nagłówków. Naruszenie schematu sesji jest atakiem o wysokim wpływie (high-impact), który przyznaje przeciwnikowi pełny dostęp do uwierzytelnionego stanu ofiary bez konieczności posiadania jej pierwotnych danych uwierzytelniających.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Wysoki |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Narzędzia:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Czy identyfikator sesji jest wystarczająco złożony i losowy?
- [ ] Tak — token przechodzi statystyczne testy losowości (wysoka entropia), a jego obejście (bypass) **nie jest możliwe**  
- [ ] Tak — token jest długi, ale zawiera przewidywalne wzorce lub statyczne segmenty  
- [ ] Nie — token jest sekwencyjny, oparty na przewidywalnych znacznikach czasu lub posiada **krytycznie niską** entropię *(Krytyczne)*  

### Gdzie identyfikator sesji jest przesyłany i przechowywany?
- [ ] Identyfikator jest przechowywany w ciasteczku z flagami `Secure` i `HttpOnly` *(Najbardziej bezpieczne)*  
- [ ] Identyfikator jest przechowywany w ciasteczku, ale brakuje mu flag `Secure` lub `HttpOnly`  
- [ ] Identyfikator jest przesyłany **wyłącznie** za pomocą parametrów URL lub żądań `GET` *(Wysokie ryzyko)*  

### Czy aplikacja wystawia nowy identyfikator sesji po pomyślnym uwierzytelnieniu?
- [ ] Tak — nowy, unikalny identyfikator sesji (Session ID) **jest generowany** natychmiast po zalogowaniu  
- [ ] Nie — identyfikator sesji pozostaje taki sam przed i po zalogowaniu *(Możliwy Session Fixation)*  

### Czy identyfikator sesji jest powiązany z konkretnym adresem IP lub User-Agent, aby zapobiec ponownemu użyciu?
- [ ] Tak — sesja jest unieważniana, jeśli odcisk palca (fingerprint) klienta ulegnie znacznej zmianie  
- [ ] Nie — sesja **może** być użyta ponownie na różnych adresach IP lub urządzeniach bez dodatkowej weryfikacji  

### Czy identyfikatory sesji są poprawnie unieważniane po stronie serwera po wylogowaniu lub wygaśnięciu sesji?
- [ ] Tak — stan sesji po stronie serwera jest **całkowicie niszczony** po wylogowaniu  
- [ ] Nie — sesja pozostaje ważna na serwerze, nawet jeśli ciasteczko po stronie klienta zostało usunięte  
- [ ] Nie — sesja **nigdy nie wygasa** lub posiada nadmiernie długi czas wygaśnięcia (timeout)  

---