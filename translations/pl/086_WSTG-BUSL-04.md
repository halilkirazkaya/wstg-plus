## WSTG-BUSL-04 — Testowanie pod kątem czasu trwania procesów (Process Timing)

Ataki oparte na analizie czasu trwania procesów (Process Timing) polegają na mierzeniu czasu potrzebnego aplikacji na przetworzenie konkretnych żądań w celu wywnioskowania wrażliwych informacji o stanach wewnętrznych lub istnieniu danych. Poprzez analizę rozbieżności w czasie odpowiedzi na poziomie milisekund — często spowodowanych logiką warunkową typu „early-exit”, zapytaniami do bazy danych lub operacjami porównywania kryptograficznego o zmiennym czasie trwania (non-constant-time) — atakujący mogą przeprowadzić analizę kanałem bocznym (side-channel analysis) w celu enumeracji prawidłowych nazw użytkowników, weryfikacji fragmentów haseł lub potwierdzenia istnienia konkretnych rekordów. Podatności te występują zazwyczaj w punktach końcowych (endpoints) uwierzytelniania, formularzach resetowania hasła oraz obciążających zasoby filtrach API, gdzie logika backendowa rozgałęzia się w zależności od poprawności danych wejściowych. Z perspektywy atakującego, te różnice czasowe służą jako „wyrocznia boolowska” (boolean oracle), która ujawnia fakty dotyczące danych w backendzie, nawet jeśli aplikacja zwraca użytkownikowi identyczne, ogólne komunikaty o błędach.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Status testu** | Nie przeprowadzono |
| **Krytyczność** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Narzędzia:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### Czy aplikacja wykazuje zróżnicowany czas odpowiedzi dla prawidłowych i nieprawidłowych żądań zasobów?
- [ ] Nie — czasy odpowiedzi są identyczne niezależnie od poprawności danych wejściowych  
- [ ] Tak — istnieją nieznaczne różnice czasowe, ale **nie** są one istotne statystycznie  
- [ ] Tak — spójne różnice czasowe są **obserwowalne** i stanowią wiarygodną wyrocznię  

### Czy zaimplementowano funkcje porównywania o stałym czasie trwania (constant-time) lub sztuczne opóźnienia w celu normalizacji czasów odpowiedzi?
- [ ] Tak — algorytmy o stałym czasie trwania są **stosowane** we wszystkich wrażliwych operacjach porównywania *(Najbardziej bezpieczne)*  
- [ ] Tak — sztuczne opóźnienia lub fluktuacje (jitter) są **włączone**, ale podstawowa logika pozostaje zmienna w czasie  
- [ ] Nie — nie **zastosowano** żadnych mechanizmów mitygacji, co pozwala na bezpośredni pomiar gałęzi logiki  

### Czy atakujący może wykorzystać analizę statystyczną do wyizolowania sygnału czasowego z szumu sieciowego?
- [ ] Nie — fluktuacje sieciowe (jitter) i szumy aplikacji sprawiają, że analiza czasowa jest **niemożliwa**  
- [ ] Tak — przy odpowiednio dużej próbie, sygnał czasowy **może** zostać wyizolowany z szumu  
- [ ] Tak — różnica czasowa (delta) jest na tyle duża, że normalizacja statystyczna **nie** jest wymagana  

### Czy możliwa jest enumeracja kont za pomocą funkcji logowania lub resetowania hasła?
- [ ] Nie — nie można określić istnienia konta na podstawie czasu odpowiedzi  
- [ ] Tak — rozbieżności czasowe pozwalają zdalnemu atakującemu na **przeprowadzenie enumeracji** prawidłowych nazw użytkowników lub adresów e-mail  

### Czy operacje obciążające zasoby (np. przetwarzanie plików, złożone wyszukiwania) ujawniają istnienie danych poprzez analizę czasu?
- [ ] Nie — zużycie zasobów jest jednolite, niezależnie od tego, czy rekord zostanie odnaleziony  
- [ ] Tak — istnienie wrażliwych rekordów **może** zostać wywnioskowane poprzez pomiar czasu przetwarzania po stronie backendu  

---