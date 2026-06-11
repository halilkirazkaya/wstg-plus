## WSTG-SESS-04 — Testowanie pod kątem ujawnionych zmiennych sesyjnych

Ujawnienie zmiennych sesyjnych występuje, gdy wrażliwe identyfikatory sesji lub dane dotyczące stanu są przesyłane niezabezpieczonymi kanałami, takimi jak parametry zapytania URL (URL query strings), nagłówki Referer lub logi systemowe. Ekspozycja ta znacząco zwiększa ryzyko przejęcia sesji (Session Hijacking), ponieważ identyfikatory mogą być buforowane w historii przeglądarki, rejestrowane przez serwery proxy lub wyciekać do witryn osób trzecich za pośrednictwem nagłówka Referer. Pentesterzy identyfikują te podatności poprzez monitorowanie wszystkich żądań wychodzących oraz inspekcję logów aplikacji lub konfiguracji serwerów pod kątem niezamierzonego wycieku danych. W rzeczywistych warunkach atakujący wykorzystują te wycieki, pozyskując ważne tokeny sesyjne ze współdzielonych maszyn lub analizując wzorce ruchu w celu obejścia uwierzytelniania i uzyskania nieautoryzowanego dostępu do kont użytkowników.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się Wysoki, jeśli ujawniona zmienna jest tokenem sesyjnym umożliwiającym natychmiastowe przejęcie konta.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Narzędzia:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### Czy identyfikator sesji jest przesyłany w parametrach zapytania URL?
- [ ] Nie — identyfikatory sesji **nie** występują w parametrach zapytania URL  
- [ ] Tak — identyfikatory są obecne, ale sesja jest krótkotrwała lub niskiego ryzyka  
- [ ] Tak — unikalne identyfikatory sesji (Session IDs) **są** obecne w parametrach zapytania URL *(Krytyczne)*  

### Czy aplikacja ujawnia tokeny sesyjne poprzez nagłówek Referer?
- [ ] Nie — `Referrer-Policy` zapobiega wyciekom lub nie istnieją linki do zewnętrznych witryn  
- [ ] Tak — tokeny sesyjne **są** przesyłane do domen zewnętrznych za pośrednictwem nagłówka Referer  

### Czy zmienne sesyjne są przechowywane w logach po stronie serwera lub aplikacji?
- [ ] Nie — zmienne sesyjne są maskowane lub wykluczone z logów  
- [ ] Tak — zmienne sesyjne **są** zapisywane w logach aplikacji lub serwera WWW w formie jawnego tekstu  

### Czy aplikacja używa żądań GET do operacji zmieniających stan?
- [ ] Nie — aplikacja używa metody `POST` lub innych metod nieidempotentnych dla operacji wrażliwych  
- [ ] Tak — używana jest metoda `GET`, co powoduje buforowanie danych sesyjnych w historii przeglądarki i pośredniczących serwerach proxy  

### Czy nagłówek `Cache-Control` jest skonfigurowany tak, aby zapobiegać buforowaniu danych związanych z sesją?
- [ ] Tak — nagłówek `Cache-Control: no-store` (lub podobny) **jest stosowany** do wrażliwych odpowiedzi  
- [ ] Tak — zabezpieczenia istnieją, ale możliwe jest ich **obejście** w specyficznych przypadkach przeglądarek  
- [ ] Nie — wrażliwe odpowiedzi zawierające dane sesyjne **mogą** być buforowane przez przeglądarkę  

---