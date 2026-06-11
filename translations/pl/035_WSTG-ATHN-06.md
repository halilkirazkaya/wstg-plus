## WSTG-ATHN-06 — Testowanie pod kątem słabości pamięci podręcznej przeglądarki

Słabość pamięci podręcznej przeglądarki (Browser cache weakness) występuje, gdy wrażliwe informacje są przechowywane w lokalnej pamięci podręcznej przeglądarki i mogą zostać odzyskane przez nieuprawnionego użytkownika mającego dostęp do tej samej maszyny fizycznej. Brak odpowiednich nagłówków kontroli pamięci podręcznej (Cache-Control headers) pozwala na przetrzymywanie potencjalnie wrażliwych danych, takich jak informacje osobowe, szczegóły konta lub identyfikatory sesji, po wylogowaniu użytkownika lub zamknięciu sesji. Atakujący lub kolejni użytkownicy współdzielonych lub publicznych terminali mogą to wykorzystać poprzez nawigowanie w historii przeglądarki, użycie przycisku „Wstecz” lub sprawdzanie lokalnych plików pamięci podręcznej w celu eksfiltracji (exfiltrate) zbuforowanych odpowiedzi. Zapewnienie implementacji rygorystycznych dyrektyw, takich jak `Cache-Control: no-store` i `Pragma: no-cache`, jest kluczowe dla każdej aplikacji obsługującej uwierzytelnione treści, aby zagwarantować, że wrażliwe dane nigdy nie zostaną zapisane na dysku.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Niski / Średni* |

> *Poziom zagrożenia wzrasta do Średniego, jeśli aplikacja jest często używana na publicznych kioskach lub zawiera wysoce wrażliwe dane PII/PHI.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Narzędzia:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Czy na stronach uwierzytelnionych lub wrażliwych obecne są odpowiednie nagłówki cache-control?
- [ ] Tak — nagłówki `Cache-Control: no-store, no-cache` oraz `Pragma: no-cache` są **obecne** i **poprawnie skonfigurowane**  
- [ ] Tak — niektóre nagłówki są obecne, ale brakuje dyrektywy `no-store`, co pozwala na zapisywanie danych na dysku (disk caching)  
- [ ] Nie — nie zastosowano żadnych nagłówków związanych z pamięcią podręczną, polegając na domyślnym zachowaniu przeglądarki  

### Czy aplikacja używa dyrektywy `private` dla danych specyficznych dla użytkownika?
- [ ] Tak — dyrektywa `Cache-Control: private` jest **włączona** dla wszystkich spersonalizowanych treści, aby zapobiec buforowaniu przez serwery proxy (proxy caching)  
- [ ] Nie — treść jest oznaczona jako `public` lub brakuje dyrektywy `private`, co umożliwia jej **buforowanie** przez pośredniczące serwery proxy  

### Czy wrażliwe informacje są dostępne za pomocą przycisku „Wstecz” przeglądarki po poprawnym wylogowaniu?
- [ ] Nie — przeglądarka prosi o ponowne uwierzytelnienie lub strona **nie jest ładowana** z lokalnej pamięci podręcznej  
- [ ] Tak — wrażliwe informacje są **nadal widoczne** za pomocą przycisku wstecz po zakończeniu sesji  

### Czy wrażliwe pliki niebędące dokumentami HTML (np. PDF-y, eksporty CSV, odpowiedzi API JSON) są chronione przed buforowaniem?
- [ ] Nie — aplikacja nie generuje ani nie obsługuje wrażliwych plików niebędących dokumentami HTML  
- [ ] Tak — rygorystyczne nagłówki cache-control są **zastosowane** do wszystkich pobieranych plików wrażliwych oraz punktów końcowych API  
- [ ] Nie — wrażliwe pliki do pobrania lub odpowiedzi API są **przechowywane** w lokalnej pamięci podręcznej przeglądarki lub w katalogach tymczasowych  

### Czy aplikacja używa nagłówka `Expires: 0` lub daty z przeszłości, aby zapobiec użyciu nieaktualnych danych?
- [ ] Tak — nagłówek `Expires` jest ustawiony na `0` lub datę historyczną, **zapewniając**, że przeglądarka traktuje treść jako natychmiastowo wygasłą  
- [ ] Nie — brakuje nagłówka `Expires` lub jest on ustawiony na przyszłą datę dla stron wrażliwych  

---