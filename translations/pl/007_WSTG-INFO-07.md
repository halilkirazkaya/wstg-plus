## WSTG-INFO-07 — Mapowanie ścieżek wykonania aplikacji

Mapowanie ścieżek wykonania obejmuje systematyczną identyfikację wszystkich osiągalnych punktów końcowych (endpoints), przepływów funkcjonalnych oraz gałęzi decyzyjnych w ramach aplikacji internetowej. Proces ten jest kluczowy dla upewnienia się, że żadna „ukryta” funkcjonalność – taka jak przestarzały kod (legacy code), interfejsy debugowania czy nieudokumentowane trasy API – nie pozostaje poza kontrolą bezpieczeństwa, ponieważ często brakuje w nich nowoczesnych mechanizmów zabezpieczających. Atakujący wykorzystują kombinację automatycznego skanowania (spidering) i manualnej eksploracji do wizualizacji struktury aplikacji, często odkrywając przy tym podatności, takie jak nieautoryzowany dostęp administracyjny lub błędy logiki biznesowej. Poprzez korelację danych wejściowych z konkretnymi odpowiedziami serwera, testerzy mogą precyzyjnie wskazać wrażliwą logikę przetwarzania, która wymaga ukierunkowanych, pogłębionych testów w celu zapewnienia pełnego pokrycia.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Informacyjny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### Czy aplikacja została w pełni zindeksowana przy użyciu automatycznego i manualnego crawlingu?
- [ ] Tak — kompleksowa mapa wszystkich widocznych i powiązanych zasobów **została ukończona**  
- [ ] Tak — automatyczny crawling **został ukończony**, ale manualna eksploracja jest w toku  
- [ ] Nie — aplikacja **nie** została zindeksowana  

### Czy zidentyfikowano ukryte lub nieudokumentowane punkty końcowe poprzez analizę JS lub brute-forcing katalogów?
- [ ] Nie — nie wykryto nieudokumentowanych ścieżek za pomocą analizy kanałami bocznymi (side-channel analysis)  
- [ ] Tak — wykryto ukryte ścieżki, ale **nie można** uzyskać do nich dostępu bez autoryzacji  
- [ ] Tak — nieudokumentowane punkty końcowe (endpoints) **są** dostępne i udostępniają wrażliwą funkcjonalność *(Krytyczny / Wysoki)*  

### Czy ścieżki wykonania mogą być zmieniane za pomocą niestandardowych parametrów lub nagłówków?
- [ ] Nie — parametry takie jak `debug`, `test` lub `admin` **nie** wpływają na przepływ wykonania  
- [ ] Tak — parametry zmieniają dane wyjściowe, ale **nie pozwalają** na obejście mechanizmów kontroli bezpieczeństwa  
- [ ] Tak — parametry zmieniające ścieżkę **są stosowane** i pozwalają na obejście zamierzonej logiki  

### Czy przepływ wieloetapowej logiki biznesowej (np. uwierzytelnianie wieloskładnikowe, proces zakupu) został w pełni zmapowany?
- [ ] Tak — wszystkie stany i przejścia w procesach wieloetapowych **zostały zidentyfikowane**  
- [ ] Nie — złożone przejścia maszyn stanów **nie mogą** zostać w pełni zmapowane  

### Czy pliki dokumentacji API (Swagger/OpenAPI) lub mapy po stronie klienta (Source Maps) są ujawnione?
- [ ] Nie — żadne mapy architektury ani pliki dokumentacji nie są publicznie ujawnione  
- [ ] Tak — dokumentacja istnieje, ale **jest chroniona** przez uwierzytelnianie  
- [ ] Tak — Swagger UI, OpenAPI JSON lub JS Source Maps **są włączone** i publicznie dostępne  

---