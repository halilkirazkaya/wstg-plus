## WSTG-SESS-06 — Testowanie mechanizmu wylogowywania

Funkcjonalność wylogowywania jest krytycznym mechanizmem kontroli bezpieczeństwa, mającym na celu zakończenie sesji użytkownika i unieważnienie powiązanych identyfikatorów sesji zarówno po stronie klienta, jak i serwera. Nieprawidłowa implementacja wylogowywania umożliwia ataki typu Session Fixation lub Session Hijacking, ponieważ atakujący, który uzyska dostęp do komputera po tym, jak użytkownik się „wylogował”, mógłby potencjalnie ponownie użyć wciąż aktywnego tokena sesji. Pentesterzy oceniają to, sprawdzając, czy Session Cookie jest usuwany z przeglądarki, czy stan sesji po stronie serwera jest jawnie niszczony oraz czy nawigacja przyciskiem „Wstecz” pozwala na dostęp do zbuforowanych danych wrażliwych. Bezpieczna implementacja wylogowywania gwarantuje, że po wyzwoleniu tej akcji żadne kolejne żądania ze starym tokenem sesji nie zostaną autoryzowane przez aplikację.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Status testu** | Nieprzeprowadzony |
| **Ważność** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Narzędzia:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Czy funkcjonalny mechanizm wylogowywania jest obecny i dostępny?
- [ ] Tak — przycisk wylogowania istnieje i wyzwala żądanie zakończenia sesji  
- [ ] Nie — brak przycisku wylogowania lub nie wyzwala on zdarzenia zakończenia sesji  

### Czy identyfikator sesji jest unieważniany po stronie serwera?
- [ ] Tak — serwer odrzuca wszystkie kolejne żądania przy użyciu starego tokena sesji  
- [ ] Nie — serwer nadal akceptuje stary token sesji po wylogowaniu  

### Czy aplikacja czyści pliki cookie sesji w przeglądarce po wylogowaniu?
- [ ] Tak — pliki cookie są nadpisywane wygasłą datą lub pustą wartością  
- [ ] Tak — pliki cookie pozostają, ale nie są już ważne na serwerze  
- [ ] Nie — pliki cookie pozostają w przeglądarce i zachowują swoje pierwotne wartości  

### Czy po wylogowaniu można uzyskać dostęp do wrażliwych, uwierzytelnionych treści za pomocą przycisku „Wstecz” w przeglądarce?
- [ ] Nie — nagłówki `Cache-Control: no-store` lub podobne uniemożliwiają przeglądanie wrażliwych stron po wylogowaniu  
- [ ] Tak — wrażliwe strony są przechowywane w pamięci podręcznej (cache) i mogą być przeglądane poprzez nawigację po zakończeniu sesji  

---