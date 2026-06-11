## WSTG-INPV-20 — Mass Assignment

Mass Assignment, znane również jako Overposting lub niebezpieczna deserializacja parametrów, występuje, gdy aplikacja webowa automatycznie wiąże przychodzące parametry żądania HTTP z wewnętrznymi właściwościami obiektów bez odpowiedniego filtrowania atrybutów. Podatność ta jest powszechna w nowoczesnych frameworkach MVC oraz API typu RESTful, gdzie dane w formacie JSON, XML lub dane zakodowane w formularzu są bezpośrednio deserializowane do modeli danych po stronie aplikacji. Atakujący wykorzystują to zachowanie, wstrzykując do żądania dodatkowe, niewymienione parametry — takie jak `isAdmin`, `role` czy `account_balance` — w celu zmodyfikowania wrażliwych pól, których programiści nie zamierzali udostępniać pod kontrolę użytkownika. Skuteczna eksploatacja zazwyczaj skutkuje eskalacją uprawnień (Privilege Escalation), nieautoryzowaną modyfikacją danych lub obejściem krytycznych procesów logiki biznesowej.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom dotkliwości** | Wysoki / Średni* |

> *Poziom dotkliwości staje się Wysoki, jeśli możliwe jest zmodyfikowanie atrybutów administracyjnych, poziomów uprawnień lub pól rozliczeniowych.

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### Czy aplikacja wykorzystuje automatyczne wiązanie parametrów przychodzących żądań?
- [ ] Nie — parametry są ręcznie mapowane na konkretne zmienne lub bezpieczne obiekty  
- [ ] Tak — automatyczne wiązanie jest używane, ale wymuszane są ścisłe białe listy (**white-lists**) lub obiekty DTO (Data Transfer Objects)  
- [ ] Tak — automatyczne wiązanie jest używane i możliwe jest modyfikowanie wrażliwych atrybutów wewnętrznych  

### Czy można skutecznie wstrzyknąć atrybuty administracyjne lub związane z uprawnieniami?
- [ ] Nie — wstrzyknięte parametry są ignorowane, usuwane lub powodują błąd walidacji  
- [ ] Tak — dodatkowe parametry są akceptowane, ale nie wpływają na stan obiektu w backendzie  
- [ ] Tak — wstrzykiwanie parametrów takich jak `is_admin`, `role` lub `permissions` skutkuje pomyślną eskalacją uprawnień *(Krytyczny)*  

### Czy możliwe jest zmodyfikowanie wrażliwej logiki biznesowej lub pól finansowych poprzez Overposting?
- [ ] Nie — pola takie jak `price`, `balance` czy `status` są chronione przed Mass Assignment  
- [ ] Tak — wrażliwe atrybuty biznesowe mogą być modyfikowane poprzez dodanie ich do treści żądania JSON lub formularza  

### Czy aplikacja ujawnia wewnętrzne struktury obiektów, co ułatwia odgadywanie parametrów?
- [ ] Nie — odpowiedzi zawierają tylko zamierzone pola publiczne, a dokumentacja jest ograniczona  
- [ ] Tak — wewnętrzne pola obiektów są widoczne w odpowiedziach GET, co umożliwia odkrywanie parametrów  
- [ ] Tak — dokumentacja API (Swagger/OpenAPI) wyraźnie wymienia właściwości wewnętrzne, które mogą być przypisane  

---