## WSTG-INFO-06 — Identyfikacja punktów wejścia do aplikacji

Identyfikacja punktów wejścia do aplikacji polega na mapowaniu całej powierzchni ataku (attack surface) poprzez enumerację wszystkich możliwych sposobów, w jakie użytkownik lub system zewnętrzny może wchodzić w interakcję z aplikacją. Proces ten obejmuje wykrywanie wszystkich adresów URL, punktów końcowych API (endpoints), parametrów (GET, POST, opartych na plikach cookie), nagłówków HTTP oraz specjalistycznych protokołów, takich jak WebSockets czy gRPC. Dla testera penetracyjnego etap ten jest kluczowy, ponieważ podatności często znajdują się w nieudokumentowanych "shadow" API, punktach końcowych typu legacy lub złożonych strukturach parametrów, które zostały pominięte podczas programowania. Dokładna enumeracja gwarantuje, że żaden komponent aplikacji nie pozostanie nietestowaną „czarną skrzynką” (black box), co zapobiega znalezieniu przez atakujących nie monitorowanych ścieżek dostępu do systemu.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Informacyjny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Czy wszystkie parametry GET i POST zostały wyliczone w całej aplikacji?
- [ ] Tak — kompleksowa enumeracja poprzez automatyczny i ręczny crawling **została zakończona**  
- [ ] Tak — zidentyfikowano tylko popularne parametry poprzez standardowy crawling  
- [ ] Nie — parametry są w dużej mierze **niezidentyfikowane** lub nietestowane  

### Czy poszukiwano nieudokumentowanych lub ukrytych parametrów (np. debug, admin) za pomocą techniki fuzzing?
- [ ] Nie — odkrywanie ukrytych parametrów **nie jest konieczne** dla tego konkretnego zakresu (scope)  
- [ ] Tak — przeprowadzono fuzzing parametrów (np. przy użyciu `Arjun`) i nie znaleziono żadnych wyników  
- [ ] Tak — ukryte parametry **zostały wykryte** i zmapowane do dalszych testów  
- [ ] Nie — discovery ukrytych parametrów **nie zostało** przeprowadzone  

### Czy dla każdego punktu końcowego zidentyfikowano wszystkie obsługiwane metody HTTP (PUT, DELETE, PATCH itp.)?
- [ ] Tak — wykrywanie metod **zastosowano** dla wszystkich odkrytych punktów końcowych  
- [ ] Tak — wykrywanie metod **zastosowano** tylko dla punktów końcowych o wysokim priorytecie lub wrażliwym charakterze  
- [ ] Nie — zidentyfikowano tylko standardowe metody GET i POST  

### Czy zmapowano niestandardowe punkty wejścia, takie jak WebSockets, gRPC lub niestandardowe nagłówki (custom headers)?
- [ ] Nie — aplikacja **nie wykorzystuje** niestandardowych protokołów ani niestandardowych nagłówków  
- [ ] Tak — wszystkie specyficzne dla protokołów punkty wejścia i niestandardowe nagłówki **zostały zidentyfikowane**  
- [ ] Nie — niestandardowe punkty wejścia **istnieją**, ale nie zostały zmapowane  

### Czy powierzchnia API (w tym wersjonowane punkty końcowe, takie jak /v1/ lub /v2/) została w pełni zmapowana?
- [ ] Nie — aplikacja **nie udostępnia** API  
- [ ] Tak — wszystkie wersje API i punkty końcowe **zostały zidentyfikowane** i udokumentowane  
- [ ] Tak — zidentyfikowano tylko aktualną wersję produkcyjną API  
- [ ] Nie — punkty końcowe API **pozostają niezmapowane** lub zostały odkryte tylko częściowo  

---