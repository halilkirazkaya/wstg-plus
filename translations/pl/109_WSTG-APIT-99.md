## WSTG-APIT-99 — Testowanie GraphQL

GraphQL to język zapytań dla API, który umożliwia klientom żądanie określonych struktur danych, jednak błędy konfiguracji często prowadzą do istotnych zagrożeń bezpieczeństwa, w tym ujawnienia informacji i wyczerpania zasobów. Podatności zazwyczaj wynikają z włączonej introspekcji (introspection), braku limitów głębokości (depth) i złożoności (complexity) zapytań oraz błędów autoryzacji na poziomie obiektu (Broken Object-Level Authorization - BOLA) w funkcjach resolverów. Atakujący wykorzystują te punkty końcowe (endpoints), używając introspekcji do zmapowania całego schematu, wykorzystując cykliczne fragmenty do wywołania odmowy usługi (Denial of Service - DoS) lub omijając autoryzację poprzez dostęp do nieautoryzowanych pól w zagnieżdżonych zapytaniach. Testowanie koncentruje się na punktach końcowych `/graphql`, `/v1/graphql` lub `/graphiql`, aby upewnić się, że implementacja wymusza rygorystyczną kontrolę dostępu i zarządzanie zasobami.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Status testu** | Nie przeprowadzono |
| **Poziom zagrożenia** | Wysoki / Krytyczny* |

> *Poziom zagrożenia staje się krytyczny, jeśli błędy autoryzacji na poziomie obiektu (BOLA) umożliwiają nieautoryzowany dostęp do danych osobowych (PII) lub mutacji (mutations) administracyjnych.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Narzędzia:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### Czy system introspekcji (introspection) GraphQL jest włączony?
- [ ] Nie — introspekcja jest **wyłączona** i nie można zmapować schematu  
- [ ] Tak — introspekcja jest **włączona**, ale wymaga uwierzytelnienia administracyjnego  
- [ ] Tak — introspekcja jest **włączona** i publicznie dostępna *(Wysoki)*  

### Czy na zapytania nałożone są limity zasobów (głębokość i złożoność)?
- [ ] Tak — rygorystyczne limity głębokości (depth) i złożoności (complexity) zapytań są **stosowane** i wymuszane  
- [ ] Tak — limity są **stosowane**, ale można je ominąć za pomocą aliasów lub fragmentów  
- [ ] Nie — nie zastosowano żadnych limitów, co umożliwia zapytania rekurencyjne i ataki DoS  

### Czy w resolverach występuje Broken Object-Level Authorization (BOLA)?
- [ ] Nie — autoryzacja jest **stosowana** na poziomie pola i obiektu dla każdego zapytania  
- [ ] Tak — autoryzacja jest **stosowana** do niektórych pól, ale wrażliwe obiekty są ujawnione  
- [ ] Tak — autoryzacja **nie jest stosowana**, co umożliwia dostęp do dowolnego obiektu poprzez manipulację identyfikatorem (ID)  

### Czy mutacje (mutations) GraphQL są odpowiednio chronione przed nieautoryzowanym użyciem?
- [ ] Nie — mutacje **nie występują** w schemacie  
- [ ] Tak — mutacje wymagają ważnego uwierzytelnienia i rygorystycznych kontroli autoryzacji  
- [ ] Tak — mutacje są **możliwe** dla nieuwierzytelnionych użytkowników lub brakuje w nich autoryzacji  

### Czy komunikaty o błędach wyciekają wrażliwe szczegóły implementacji lub schematu?
- [ ] Nie — komunikaty o błędach są ogólne i **nie ujawniają** wewnętrznej logiki  
- [ ] Tak — komunikaty o błędach ujawniają typy bazodanowe lub sugestie via „Did you mean...?”  
- [ ] Tak — pełne ślady stosu (stack traces) i wrażliwe dane konfiguracyjne są **ujawniane** w odpowiedziach  

---