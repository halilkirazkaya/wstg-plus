## WSTG-INPV-09 — Testowanie pod kątem XPath Injection

XPath Injection występuje, gdy aplikacja wykorzystuje informacje dostarczone przez użytkownika do konstruowania zapytania XPath dla danych XML, co pozwala atakującemu na ingerencję w logikę zapytania. Poprzez wstrzykiwanie specyficznej składni, takiej jak pojedyncze cudzysłowy, nawiasy i operatory logiczne, atakujący może poruszać się po strukturze dokumentu XML, omijać uwierzytelnianie lub eksfiltrować wrażliwe węzły danych. Podatność ta jest powszechnie spotykana w usługach sieciowych (web services), interfejsach zarządzania konfiguracją oraz systemach typu legacy, które opierają się na magazynach danych XML zamiast tradycyjnych baz danych SQL. Z perspektywy atakującego, błąd ten jest często eksploatowany przy użyciu technik boolean-based lub error-based w celu systematycznego mapowania drzewa XML i pobierania ukrytych wartości z backendu.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Status testu** | Nie wykonano |
| **Krytyczność** | Wysoka / Krytyczna |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Narzędzia:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Czy dane wejściowe użytkownika używane do konstruowania zapytań XPath są odpowiednio sanityzowane lub parametryzowane?
- [ ] Tak — dane wejściowe **nie są** używane w zapytaniach XPath lub są ściśle parametryzowane  
- [ ] Tak — **zastosowano** solidną walidację/kodowanie danych wejściowych i obejście **nie jest możliwe**  
- [ ] Tak — **zastosowano** pewną walidację, ale obejście **jest możliwe** przy użyciu zakodowanych znaków  
- [ ] Nie — dane wejściowe użytkownika są bezpośrednio konkatenowane z zapytaniami XPath *(Krytyczne)*  

### Czy aplikacja ujawnia szczegóły strukturalne XML/XPath poprzez komunikaty o błędach?
- [ ] Nie — wyświetlane są ogólne strony błędów i **żadne** szczegóły XML nie wyciekają  
- [ ] Tak — komunikaty o błędach ujawniają obecność przetwarzania XML, ale **brak** szczegółów ścieżek  
- [ ] Tak — szczegółowe komunikaty o błędach ujawniają składnię XPath, nazwy węzłów lub ścieżki do plików  

### Czy możliwe jest obejście logiki uwierzytelniania lub autoryzacji przy użyciu logiki XPath?
- [ ] Nie — logika uwierzytelniania **nie opiera się** na XML/XPath lub jest zaimplementowana w sposób bezpieczny  
- [ ] Tak — kontrole logowania lub uprawnień można obejść za pomocą ładunków (Payload) opartych na logice, takich jak `' or 1=1 or '`  

### Czy struktura dokumentu XML lub dane mogą być wyliczane poprzez blind injection?
- [ ] Nie — aplikacja **nie jest** podatna na wnioskowanie typu boolean-based lub time-based  
- [ ] Tak — nazwy węzłów i dane **mogą** być eksfiltrowane przy użyciu wnioskowania typu boolean-based  
- [ ] Tak — pełne przechodzenie drzewa XML (traversal) i ekstrakcja danych **są możliwe**  

---