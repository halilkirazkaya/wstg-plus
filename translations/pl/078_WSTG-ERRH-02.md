## WSTG-ERRH-02 — Testowanie pod kątem Stack Traces

Ślady stosu wywołań (Stack traces) są generowane, gdy aplikacja nie radzi sobie z obsługą wyjątku w sposób kontrolowany, co skutkuje ujawnieniem użytkownikowi końcowemu wewnętrznego stanu wykonania oraz hierarchii wywołań. Dla testera penetracyjnego ślady te są bezcenne, ponieważ ujawniają framework aplikacji, wersje bibliotek, wewnętrzne ścieżki systemu plików oraz przepływ logiki, co znacząco redukuje nakład pracy wymagany do przeprowadzenia rekonesansu. Eksploatacja polega na celowym wywoływaniu błędów poprzez nieprawidłowo sformatowane dane wejściowe (malformed input), nieoczekiwane typy danych lub naruszenia warunków brzegowych, aby wymusić niestabilny stan aplikacji i dokonać eksfiltracji informacji o bazowym stosie technologicznym. Wyciek ten często dostarcza kontekstu niezbędnego do zidentyfikowania podatnych komponentów firm trzecich lub przygotowania specyficznych exploitów dla błędów logicznych wykrytych w ujawnionych ścieżkach kodu.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Niski / Średni* |

> *Poziom krytyczności wzrasta do średniego, jeśli ślad stosu ujawnia wrażliwe szczegóły architektoniczne, zapytania do bazy danych lub wewnętrzne parametry konfiguracji.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Czy do klienta zwracane są pełne ślady stosu wywołań (stack traces) w przypadku błędów aplikacji?
- [ ] Nie — aplikacja zwraca generyczne strony błędów lub niestandardowe komunikaty o błędach  
- [ ] Tak — ślady stosu są **włączone**, ale tylko dla określonych, niewrażliwych modułów  
- [ ] Tak — pełne ślady stosu są **włączone** i widoczne w treści odpowiedzi HTTP  

### Czy komunikaty o błędach ujawniają wrażliwe informacje o środowisku?
- [ ] Nie — komunikaty o błędach nie zawierają żadnych szczegółów technicznych ani środowiskowych  
- [ ] Tak — komunikaty ujawniają **wewnętrzne ścieżki plików** lub **struktury katalogów** po stronie serwera  
- [ ] Tak — komunikaty ujawniają **schematy baz danych**, **zapytania SQL** lub **wersje bibliotek firm trzecich**  

### Czy ślady stosu wywołań mogą być wywołane poprzez manipulację parametrami wejściowymi?
- [ ] Nie — nieprawidłowo sformatowane dane wejściowe są obsługiwane w sposób kontrolowany lub odrzucane przez walidację  
- [ ] Tak — ślady mogą zostać wywołane przez **nieoczekiwane typy danych** (np. przesłanie tablicy w miejscu, gdzie oczekiwany jest ciąg znaków)  
- [ ] Tak — ślady są wywoływane przez **bajty zerowe (null bytes)**, **znaki specjalne** lub **wartości brzegowe**  

### Czy w całej aplikacji stosowana jest spójna, globalna obsługa błędów?
- [ ] Tak — globalny mechanizm obsługi wyjątków jest **spójnie stosowany** we wszystkich testowanych punktach końcowych (endpoints)  
- [ ] Tak — mechanizm obsługi jest obecny, ale **nie został zastosowany** do systemów spuścizny (legacy), API lub nowo dodanych punktów końcowych  
- [ ] Nie — nie wykryto globalnego mechanizmu obsługi błędów, co skutkuje domyślnymi błędami serwera  

---