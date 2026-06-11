## WSTG-ERRH-01 — Testowanie pod kątem niewłaściwej obsługi błędów

Niewłaściwa obsługa błędów występuje, gdy aplikacja webowa ujawnia poufne szczegóły techniczne — takie jak ślady stosu (stack traces), informacje o schemacie bazy danych lub wewnętrzne ścieżki plików — poprzez odpowiedzi błędów. Atakujący wywołują te błędy, przesyłając nieprawidłowo sformatowane dane wejściowe, uzyskując dostęp do nieistniejących zasobów lub wywołując wyjątki po stronie serwera, aby zmapować bazową architekturę aplikacji i zidentyfikować potencjalne wektory dalszej eksploatacji. Ten wyciek informacji często służy jako prekursor poważniejszych ataków, w tym SQL Injection lub Path Traversal, dostarczając atakującemu precyzyjne specyfikacje środowiska. Bezpieczna implementacja musi zapewniać ogólne, przyjazne dla użytkownika komunikaty, podczas gdy szczegółowa diagnostyka powinna być rejestrowana wyłącznie w bezpiecznych logach po stronie serwera.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Status testu** | Nieprzeprowadzono |
| **Krytyczność** | Niska / Średnia |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### Czy aplikacja używa ogólnego, globalnego mechanizmu obsługi błędów dla nieobsłużonych wyjątków?
- [ ] Tak — ogólne strony błędów są **włączone** i **nie** ujawniają żadnych szczegółów technicznych  
- [ ] Tak — używane są własne strony błędów, ale wyciek **jest możliwy** poprzez konkretne nagłówki  
- [ ] Nie — domyślne strony błędów serwera (np. Tomcat, IIS, Nginx) są **widoczne**  

### Czy informacje techniczne mogą być enumerowane poprzez nieprawidłowo sformatowane dane wejściowe lub przypadki brzegowe?
- [ ] Nie — błędy są obsługiwane w sposób kontrolowany z unikalnymi identyfikatorami referencyjnymi dla celów logowania  
- [ ] Tak — ślady stosu (stack traces) lub zapytania do bazy danych **są ujawniane** w odpowiedziach  
- [ ] Tak — wewnętrzne ścieżki plików, zmienne środowiskowe lub ciągi wersji serwera **są ujawniane**  

### Czy odpowiedzi API ujawniają nadmiarowe obiekty błędów lub informacje debugowania?
- [ ] Nie — API zwracają ustandaryzowane kody błędów i oczyszczone komunikaty JSON  
- [ ] Tak — API zwracają nadmiarowe obiekty debugowania lub pełne szczegóły wyjątków w treści odpowiedzi  
- [ ] Tak — ślady stosu (stack traces) są zawarte w polach `details` lub `exception` odpowiedzi JSON  

### Czy aplikacja zachowuje się inaczej (pod kątem czasu odpowiedzi lub treści) w przypadku wystąpienia błędów?
- [ ] Nie — sygnatury odpowiedzi i czasy ich trwania są spójne niezależnie od typu błędu  
- [ ] Tak — zróżnicowane odpowiedzi **mogą** być wykorzystane do enumeracji poprawnych i niepoprawnych stanów (np. User Enumeration)  

---