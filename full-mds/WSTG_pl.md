## WSTG-INFO-01 — Wykrywanie i rekonesans w wyszukiwarkach pod kątem wycieku informacji

Wykrywanie informacji w wyszukiwarkach polega na wykorzystywaniu publicznych wyszukiwarek, stron w pamięci podręcznej (cached pages) oraz usług indeksowania w celu zidentyfikowania danych, które organizacja docelowa nieumyślnie udostępniła. Atakujący wykorzystują zaawansowane operatory wyszukiwania (Google Dorks) oraz usługi indeksowania stron trzecich do wykrywania wrażliwych plików, katalogów, portali logowania, komunikatów o błędach oraz metadanych, które nie powinny być publicznie dostępne. Ta faza rekonesansu (reconnaissance) jest krytyczna, ponieważ nie wymaga bezpośredniej interakcji z celem, co czyni ją praktycznie niewykrywalną, a jednocześnie pozwala na potencjalne ujawnienie wysokiej wartości punktów wejścia, takich jak publicznie dostępne panele administracyjne, pliki konfiguracyjne, zrzuty baz danych (database dumps) czy dokumentacja wewnętrzna.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Narzędzia:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Czy wrażliwe pliki lub katalogi są indeksowane przez wyszukiwarki?
- [ ] Nie — nie znaleziono wrażliwych treści w wynikach wyszukiwania  
- [ ] Tak — pliki konfiguracyjne, kopie zapasowe (backups) lub dokumenty wewnętrzne **są** indeksowane *(Krytyczne)*  

### Czy Google Dorking ujawnia publicznie dostępne interfejsy administracyjne lub panele logowania?
- [ ] Nie — panele administracyjne i strony logowania **nie są** indeksowane  
- [ ] Tak — panele administracyjne **są** indeksowane, ale wymagają silnego uwierzytelniania wieloskładnikowego (multi-factor authentication)  
- [ ] Tak — panele administracyjne **są** indeksowane i publicznie dostępne **bez** wystarczających mechanizmów kontroli  

### Czy wersje stron w pamięci podręcznej (cache) ujawniają wrażliwe dane, które zostały usunięte?
- [ ] Nie — nie znaleziono wrażliwych danych w migawkach pamięci podręcznej  
- [ ] Tak — wersje cache zawierają poświadczenia (credentials), wewnętrzne adresy IP lub wrażliwe informacje  

### Czy plik `robots.txt` nieumyślnie ujawnia wrażliwe ścieżki?
- [ ] Nie — `robots.txt` **nie** ujawnia wrażliwych struktur katalogów  
- [ ] Tak — `robots.txt` zawiera listę wrażliwych ścieżek, które ułatwiają rekonesans atakującemu  
- [ ] Plik `robots.txt` nie istnieje  

### Czy usługi zewnętrzne (Shodan, Censys, Wayback Machine) ujawniają historyczne lub obecne punkty ekspozycji?
- [ ] Nie — brak istotnych znalezisk w zewnętrznych usługach indeksowania  
- [ ] Tak — historyczne migawki lub skany usług ujawniają wrażliwe metadane lub banery usług (service banners)

---

## WSTG-INFO-02 — Fingerprinting serwera WWW

Fingerprinting serwera WWW to proces identyfikacji konkretnego typu oprogramowania, wersji oraz bazowego systemu operacyjnego docelowego serwera WWW. Atakujący przeprowadzają ten rekonesans w celu zidentyfikowania znanych podatności, takich jak niezałatane luki CWE/CVE lub błędy konfiguracji, które są specyficzne dla określonych wersji serwerów Apache, Nginx, IIS lub innych technologii serwerowych. Cel ten osiąga się zazwyczaj poprzez analizę nagłówków odpowiedzi HTTP (np. `Server`, `X-Powered-By`), badanie unikalnych zachowań protokołów lub obserwowanie informacji wyciekających przez domyślne strony błędów i pliki. Skuteczny fingerprinting pozwala atakującemu na dostosowanie strategii Exploit, przechodząc od ogólnego skanowania do ataków o wysokim prawdopodobieństwie sukcesu wymierzonych w znane wady architektoniczne.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Informacyjny / Niski* |

> *Poziom istotności staje się Wysoki, jeśli fingerprinting ujawni nieaktualną lub niewspieraną (EOL) wersję serwera ze znanymi krytycznymi podatnościami.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Narzędzia:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Czy w nagłówkach odpowiedzi HTTP znajdują się ciągi identyfikujące?
- [ ] Nie — nagłówki `Server` oraz `X-Powered-By` **nie są obecne** lub zawierają wartości ogólne  
- [ ] Tak — nagłówki ujawniają oprogramowanie serwera, ale **nie** podają konkretnych numerów wersji  
- [ ] Tak — nagłówki ujawniają konkretne oprogramowanie serwera oraz **dokładne** numery wersji  

### Czy domyślne strony błędów lub odpowiedzi generowane przez system ujawniają szczegóły serwera?
- [ ] Nie — stosowane są własne strony błędów, które **nie ujawniają** informacji o serwerze  
- [ ] Tak — domyślne strony błędów ujawniają nazwę oprogramowania serwera i/lub wersję  
- [ ] Tak — strony błędów ujawniają szczegóły serwera, wersję oraz **bazowy system operacyjny**  

### Czy wersja serwera jest ujawniana poprzez domyślne pliki lub strukturę katalogów?
- [ ] Nie — domyślne pliki instalacyjne, instrukcje i skrypty testowe zostały **usunięte**  
- [ ] Tak — domyślne pliki (np. `info.php`, `manual/`, `test.html`) są **obecne** i ujawniają dane o wersji  

### Czy typ serwera można trafnie wywnioskować na podstawie analizy unikalnych zachowań?
- [ ] Nie — odpowiedzi serwera są znormalizowane i fingerprinting oparty na zachowaniu **nie jest możliwy**  
- [ ] Tak — unikalne zachowania (np. kolejność nagłówków, specyficzne kody błędów lub cechy stosu TCP/IP) **pozwalają** na identyfikację serwera  

### Czy zidentyfikowana wersja serwera jest obecnie wspierana i posiada poprawki?
- [ ] Tak — zidentyfikowana wersja jest aktualna i **wspierana** przez dostawcę  
- [ ] Nie — zidentyfikowana wersja jest **nieaktualna** lub ma status **end-of-life (EOL)** i posiada znane podatności

---

## WSTG-INFO-03 — Przegląd plików meta serwera WWW pod kątem wycieku informacji

Pliki meta serwera WWW, takie jak `robots.txt`, `sitemap.xml` oraz `security.txt`, mają za zadanie instruować roboty indeksujące i dostarczać metadane administracyjne, jednak często nieumyślnie ujawniają wrażliwe struktury katalogów lub ukryte punkty końcowe (Endpoints). Atakujący analizują te pliki w celu enumeracji powierzchni ataku aplikacji, identyfikując dyrektywy "Disallow", które często wskazują na niepodlinkowane panele administracyjne, katalogi kopii zapasowych lub środowiska programistyczne. Oprócz instrukcji dla wyszukiwarek, pliki w katalogu `.well-known/` lub pliki takie jak `humans.txt` mogą ujawniać stosy technologiczne (Technology Stacks), informacje o programistach lub organizacyjne kontakty ds. bezpieczeństwa. Systematyczny przegląd tych plików pozwala testerowi uzyskać wgląd strukturalny i techniczny bez konieczności przeprowadzania agresywnego odkrywania metodą Brute Force.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom istotności** | Niski / Informacyjny* |

> *Poziom istotności staje się Średni, jeśli pliki meta ujawniają nieuwierzytelnione interfejsy administracyjne lub wrażliwe kopie zapasowe konfiguracji.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Czy plik `robots.txt` istnieje i ujawnia wrażliwe katalogi?
- [ ] Nie — `robots.txt` nie istnieje  
- [ ] Tak — `robots.txt` istnieje, ale zawiera tylko standardowe ścieżki publiczne  
- [ ] Tak — `robots.txt` ujawnia wrażliwe ścieżki do katalogów lub interfejsy administracyjne  
- [ ] Tak — `robots.txt` ujawnia poświadczenia lub konkretne wersje oprogramowania w komentarzach  

### Czy plik `sitemap.xml` jest obecny i ujawnia ukrytą strukturę aplikacji?
- [ ] Nie — `sitemap.xml` nie istnieje  
- [ ] Tak — `sitemap.xml` jest obecny, ale zawiera tylko publicznie dostępne adresy URL  
- [ ] Tak — `sitemap.xml` zawiera niepodlinkowane punkty końcowe (Endpoints) lub zasoby wewnętrzne, do których można uzyskać dostęp  

### Czy pliki meta dotyczące bezpieczeństwa i organizacji ujawniają szczegóły techniczne?
- [ ] Nie — nie znaleziono plików metadanych w katalogu głównym ani w `.well-known/`  
- [ ] Tak — pliki `security.txt` lub `humans.txt` są obecne, ale zawierają standardowe informacje  
- [ ] Tak — pliki meta ujawniają wewnętrzne nazwy hostów, tożsamości programistów lub szczegóły stosu technologicznego, które mogą pomóc w socjotechnice (Social Engineering) lub dalszych atakach  

### Czy obecne są pliki meta specyficzne dla frameworków lub pozostałości po starszych wersjach?
- [ ] Nie — nie znaleziono plików specyficznych dla frameworków (np. `info.php`, `composer.json`, `package.json`)  
- [ ] Tak — pliki takie jak `README.md`, `CHANGELOG.txt` lub `.DS_Store` są obecne, ale zostały oczyszczone  
- [ ] Tak — obecne są pliki specyficzne dla frameworków lub wersji, które pozwalają na precyzyjny Exploit lub Fingerprinting technologii

---

## WSTG-INFO-04 — Enumeracja aplikacji na serwerze WWW

Enumeracja aplikacji to proces identyfikacji wszystkich aplikacji internetowych i usług hostowanych na pojedynczym serwerze WWW lub adresie IP. Ponieważ nowoczesne serwery WWW często hostują wiele wirtualnych hostów (vhosts), starsze środowiska stagingowe lub interfejsy administracyjne na niestandardowych portach i ścieżkach, niepowodzenie w identyfikacji tych ukrytych zasobów pozostawia znaczną część powierzchni ataku (attack surface) nieprzetestowaną. Atakujący wykorzystują techniki takie jak transfery stref DNS, Brute Force wirtualnych hostów oraz Reverse IP Lookups, aby odkryć te zapomniane lub ukryte punkty wejścia, które często są mniej bezpieczne niż główna aplikacja produkcyjna.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom istotności** | Informacyjny / Niski |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Narzędzia:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Czy z infrastrukturą docelową powiązanych jest wiele adresów IP lub aliasów DNS?
- [ ] Nie — istnieje tylko pojedynczy adres IP i rekord A  
- [ ] Tak — zidentyfikowano wiele adresów IP lub aliasów, co rozszerza zakres  

### Czy serwer WWW hostuje wiele wirtualnych hostów (vhosts) na tym samym adresie IP?
- [ ] Nie — serwer obsługuje tylko domenę główną  
- [ ] Tak — zidentyfikowano dodatkowe wirtualne hosty, ale dostęp **nie jest możliwy** (np. 403 Forbidden)  
- [ ] Tak — zidentyfikowano i uzyskano dostęp do ukrytych, deweloperskich lub stagingowych hostów wirtualnych  

### Czy aplikacje są hostowane na niestandardowych portach?
- [ ] Nie — otwarte są tylko standardowe porty (80/443)  
- [ ] Tak — niestandardowe porty są otwarte, ale **nie** hostują aplikacji internetowych  
- [ ] Tak — zidentyfikowano dodatkowe aplikacje internetowe na niestandardowych portach (np. 8080, 8443, 9000)  

### Czy odrębne aplikacje są przypisane do określonych ścieżek URI?
- [ ] Nie — pojedyncza aplikacja obsługuje cały katalog główny  
- [ ] Tak — wykryto wiele aplikacji poprzez enumerację katalogów (np. `/api`, `/portal`, `/backup`)  

### Czy zewnętrzne usługi rozpoznawcze (reconnaissance) ujawniają historyczne lub pomocnicze aplikacje?
- [ ] Nie — brak istotnych wyników z `Shodan`, `Censys` lub `Wayback Machine`  
- [ ] Tak — historyczne migawki lub skany usług ujawniają aplikacje, które **są** obecnie aktywne, ale niepodlinkowane

---

## WSTG-INFO-05 — Przegląd zawartości stron internetowych pod kątem wycieku informacji

Przegląd zawartości strony internetowej obejmuje manualną i zautomatyzowaną inspekcję odpowiedzi aplikacji, w tym plików HTML, JavaScript oraz CSS, w celu zidentyfikowania wrażliwych informacji nieumyślnie udostępnionych użytkownikom. Atakujący badają kod źródłowy po stronie klienta (client-side source code) pod kątem komentarzy deweloperskich, ukrytych pól formularzy, wewnętrznych ścieżek serwera, zahardkodowanych (hardcoded) kluczy API oraz informacji diagnostycznych (debugging information), które mogą pomóc w dalszych fazach eksploatacji. Wycieki te często występują, gdy deweloperzy zapomną usunąć metadane lub kod debugujący przed przeniesieniem aplikacji na środowisko produkcyjne, co potencjalnie ujawnia błędy logiczne lub szczegóły infrastruktury backendowej. Z perspektywy atakującego stanowi to niskonakładową metodę mapowania wewnętrznej struktury aplikacji i odkrywania pominiętych punktów wejścia (entry points) bez wyzwalania alertów bezpieczeństwa czy interakcji z logiką backendową serwera.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Niski / Średni* |

> *Poziom zagrożenia staje się wysoki (High), jeśli zostaną wykryte wrażliwe zahardkodowane poświadczenia, klucze prywatne lub wewnętrzne zmienne środowiskowe.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Narzędzia:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Czy w kodzie źródłowym znajdują się komentarze deweloperskie zawierające wrażliwe informacje?
- [ ] Nie — nie znaleziono komentarzy lub znaleziono tylko ogólne, niewrażliwe komentarze  
- [ ] Tak — komentarze istnieją, ale **nie** zawierają wrażliwej logiki ani danych  
- [ ] Tak — komentarze **ujawniają** wewnętrzne ścieżki, zapytania SQL lub instrukcje administracyjne  
- [ ] Tak — komentarze **ujawniają** poświadczenia lub wrażliwe notatki deweloperskie *(High)*  

### Czy ukryte pola wejściowe HTML zawierają wrażliwe dane lub informacje o stanie?
- [ ] Nie — nie znaleziono ukrytych pól lub zawierają one tylko niewrażliwe tokeny  
- [ ] Tak — ukryte pola są używane do utrzymania stanu, ale są **zaszyfrowane** lub **podpisane**  
- [ ] Tak — ukryte pola zawierają hasła w formacie **plaintext**, role użytkowników lub wewnętrzne identyfikatory  

### Czy w plikach JavaScript znajdują się zahardkodowane (hardcoded) sekrety, klucze API lub wewnętrzne adresy IP?
- [ ] Nie — w plikach JS nie znaleziono wrażliwych ciągów znaków ani sekretów  
- [ ] Tak — pliki JS zawierają publiczne klucze API z **odpowiednimi** ograniczeniami  
- [ ] Tak — pliki JS zawierają **niezabezpieczone** klucze API, wewnętrzne adresy IP lub prywatne punkty końcowe (endpoints)  
- [ ] Tak — pliki JS zawierają **zahardkodowane** poświadczenia administracyjne lub klucze prywatne *(Critical)*  

### Czy aplikacja ujawnia wersje frameworków lub wewnętrzne ścieżki do plików w kodzie źródłowym?
- [ ] Nie — informacje o frameworkach i ścieżki do plików są **zminifikowane** (minified) lub **zaciemnione** (obfuscated)  
- [ ] Tak — nazwy i wersje frameworków są **widoczne**, ale posiadają poprawki (patched)  
- [ ] Tak — wewnętrzne ścieżki do plików lub bezwzględne ścieżki serwera są **ujawnione** w kodzie źródłowym  

### Czy kod źródłowy jest podatny na wycieki z powodu braku minifikacji lub zaciemniania (obfuscation)?
- [ ] Nie — kod produkcyjny jest **w pełni zminifikowany** i pozbawiony komentarzy  
- [ ] Tak — kod jest zminifikowany, ale komentarze **pozostają** w źródle  
- [ ] Tak — mapy źródłowe (pliki `.map`) są **publicznie dostępne**, co pozwala na pełną rekonstrukcję kodu źródłowego

---

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

## WSTG-INFO-08 — Identyfikacja frameworka aplikacji webowej (Fingerprinting)

Identyfikacja frameworka (fingerprinting) aplikacji webowej polega na rozpoznaniu konkretnego stosu technologicznego oraz wersji oprogramowania użytych do budowy i serwowania aplikacji. Atakujący analizują nagłówki odpowiedzi HTTP, pliki cookie specyficzne dla frameworka, przewidywalne struktury plików oraz unikalne artefakty JavaScript, aby określić, czy cel wykorzystuje platformy takie jak React, Angular, Django czy Spring. Ta faza rekonesansu jest kluczowa, ponieważ pozwala testerowi zawęzić powierzchnię ataku do znanych podatności (CVE) oraz typowych błędów konfiguracyjnych specyficznych dla danego frameworka. Dzięki zrozumieniu podstawowej architektury, atakujący może przejść od testów ogólnych do wysoce ukierunkowanych strategii eksploatacji (Exploit).

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Informacyjny (Informational) |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Czy nagłówki odpowiedzi HTTP ujawniają nazwę lub wersję frameworka?
- [ ] Nie — nagłówki takie jak `X-Powered-By` lub `Server` nie są obecne lub zostały oczyszczone (sanitized)  
- [ ] Tak — nazwa frameworka jest obecna, ale wersja jest ukryta  
- [ ] Tak — zarówno nazwa frameworka, jak i jego wersja są ujawnione  

### Czy pliki cookie lub struktury katalogów specyficzne dla frameworka są identyfikowalne?
- [ ] Nie — powszechne artefakty frameworka (np. ścieżki `.js`, nazwy plików cookie) są zaciemnione (obfuscated) lub zmienione  
- [ ] Tak — pliki cookie specyficzne dla frameworka (np. `JSESSIONID`, `PHPSESSID`, `csrftoken`) są identyfikowalne  
- [ ] Tak — struktury katalogów (np. `/wp-content/`, `_next/static/`) są widoczne i potwierdzają użyty framework  

### Czy komunikaty o błędach lub komentarze w kodzie źródłowym ujawniają szczegóły dotyczące frameworka?
- [ ] Nie — obsługa błędów jest ogólna, a komentarze zostały usunięte  
- [ ] Tak — zrzuty stosu (stack traces) specyficzne dla frameworka są włączone i widoczne w odpowiedziach  
- [ ] Tak — kod źródłowy HTML zawiera komentarze lub tagi metadanych identyfikujące framework  

### Czy ze zidentyfikowaną wersją frameworka powiązane są znane podatności?
- [ ] Nie — zidentyfikowany framework jest aktualny i nie posiada znanych publicznych podatności  
- [ ] Tak — wersja frameworka jest nieaktualna i można zidentyfikować konkretne podatności CVE

---

## WSTG-INFO-09 — Identyfikacja technologii (Fingerprinting) aplikacji internetowej

Fingerprinting aplikacji internetowej to proces rozpoznawania konkretnego frameworka, systemu zarządzania treścią (CMS) lub stosu technologicznego wraz z powiązanymi wersjami. Ta faza rekonesansu jest kluczowa, ponieważ pozwala atakującemu dopasować zidentyfikowane komponenty do znanych podatności (CVE) i publicznych exploitów, znacząco zawężając powierzchnię ataku. Informacje są zazwyczaj pozyskiwane z nagłówków odpowiedzi HTTP, unikalnych ścieżek plików, nazw plików cookie, metadanych kodu źródłowego HTML oraz skrótów kryptograficznych (hashy) zasobów statycznych. Poprzez dokładne określenie stosu technologicznego, atakujący może przejść od generycznego automatycznego skanowania do wysoce ukierunkowanej eksploatacji luk specyficznych dla danej wersji.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Niski / Informacyjny |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Czy nagłówki odpowiedzi HTTP ujawniają stos technologiczny lub numery wersji?
- [ ] Nie — wrażliwe nagłówki są usuwane lub zawierają generyczne wartości  
- [ ] Tak — domyślne nagłówki takie jak `X-Powered-By`, `Server` lub `X-AspNet-Version` **są** obecne  
- [ ] Tak — nagłówki ujawniają konkretne, przestarzałe wersje serwera WWW lub frameworka aplikacji  

### Czy framework aplikacji lub CMS jest możliwy do zidentyfikowania poprzez unikalne ścieżki plików lub konwencje nazewnictwa?
- [ ] Nie — ścieżki plików są zaciemnione (obfuscated) i **nie** ujawniają bazowej technologii  
- [ ] Tak — powszechnie znane struktury katalogów (np. `/wp-content/`, `/_next/`, `/node_modules/`) **są** widoczne  
- [ ] Tak — unikalne pliki takie jak `robots.txt`, `humans.txt` lub `sitemap.xml` zawierają metadane specyficzne dla danej technologii  

### Czy konkretne numery wersji są ujawnione w kodzie źródłowym HTML lub zasobach statycznych?
- [ ] Nie — nie znaleziono informacji o wersji w komentarzach ani parametrach zasobów  
- [ ] Tak — komentarze HTML lub tagi meta (np. `<meta name="generator" content="...">`) **są** obecne  
- [ ] Tak — ciągi znaków wersji są dołączane do nazw plików CSS/JS (np. `jquery.js?v=1.12.4`) i **nie mogą** zostać wyłączone  

### Czy domyślne strony błędów lub interfejsy zarządzania ujawniają środowisko oprogramowania?
- [ ] Nie — aplikacja używa własnych, generycznych stron błędów dla wszystkich kodów statusu  
- [ ] Tak — domyślne strony błędów serwera WWW lub frameworka **są** włączone i ujawniają dane o wersji  
- [ ] Tak — administracyjne portale logowania (np. `/wp-admin`, `/phpmyadmin`) **są** dostępne i możliwe do zidentyfikowania  

### Czy pliki cookie ujawniają framework zarządzania sesją?
- [ ] Nie — pliki cookie sesji używają generycznych lub własnych nazw  
- [ ] Tak — używane **są** nazwy plików cookie specyficzne dla technologii (np. `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`)

---

## WSTG-INFO-10 — Mapowanie architektury aplikacji

Mapowanie architektury aplikacji polega na identyfikacji technologii bazowych, komponentów infrastruktury oraz ścieżek przepływu danych, które wspierają aplikację internetową. Analizując nagłówki odpowiedzi HTTP, komunikaty o błędach, formaty ciasteczek (cookies) i rozszerzenia plików, testerzy mogą zrekonstruować schemat serwerów WWW, serwerów aplikacji, baz danych oraz integracji z usługami zewnętrznymi (third-party). Wiedza ta jest kluczowa dla dostosowania dalszych ataków, ponieważ pozwala na wybór exploitów specyficznych dla danej platformy oraz identyfikację potencjalnych wąskich gardeł lub błędnie skonfigurowanych urządzeń pośredniczących, takich jak load balancery i systemy WAF. Atakujący wykorzystuje taką mapę strukturalną, aby przejść od ogólnego skanowania do ukierunkowanej eksploatacji konkretnego stosu technologicznego (software stack) i jego połączonych komponentów.


| Pole | Wartość |
|---|---|
| **ID WSTG** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Informacyjny / Niski* |

> *Poziom istotności wzrasta do Średniego, jeśli ujawniona zostanie wewnętrzna topologia sieci lub prywatne adresy IP.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Czy technologie serwera WWW i serwera aplikacji mogą zostać precyzyzyjnie zidentyfikowane?
- [ ] Nie — brak możliwości identyfikacji ze względu na skuteczne utwardzanie (hardening) lub obfuskację  
- [ ] Tak — typ technologii jest znany, ale nie można określić konkretnych wersji  
- [ ] Tak — technologia i konkretne wersje są w pełni ujawnione poprzez nagłówki lub sygnatury plików *(Informacyjny)*  

### Czy urządzenia pośredniczące (WAF, Load Balancery, Proxy) są wykrywalne w ścieżce komunikacji?
- [ ] Nie — nie wykryto urządzeń pośredniczących lub są one w pełni transparentne  
- [ ] Tak — podejrzewa się ich istnienie na podstawie opóźnień (timing) lub zachowania, ale tożsamość nie została potwierdzona  
- [ ] Tak — konkretne produkty (np. Cloudflare, Nginx, F5) są zidentyfikowane poprzez unikalne nagłówki lub zachowanie  

### Czy aplikacja ujawnia informacje o swojej wewnętrznej strukturze katalogów lub topologii sieci?
- [ ] Nie — wewnętrzne ścieżki i adresy IP nie są ujawniane  
- [ ] Tak — wewnętrzne ścieżki wyciekają w komunikatach o błędach lub zrzutach stosu (stack traces), ale wewnętrzne adresy IP nie są ujawniane  
- [ ] Tak — zarówno wewnętrzne struktury katalogów, jak i prywatne adresy IP są ujawnione  

### Czy typ i wersja bazodanowego backendu mogą zostać wywnioskowane z zachowania aplikacji?
- [ ] Nie — szczegóły bazy danych nie mogą zostać określone  
- [ ] Tak — typ bazy danych (np. PostgreSQL, MSSQL) został wywnioskowany z komunikatów o błędach lub zachowania  
- [ ] Tak — typ i wersja bazy danych są jawnie ujawnione w treści odpowiedzi lub nagłówkach  

### Czy aplikacja jest hostowana u możliwych do zidentyfikowania dostawców usług chmurowych lub w konkretnych sieciach CDN firm trzecich?
- [ ] Nie — infrastruktura hostingowa nie jest możliwa do zidentyfikowania  
- [ ] Tak — dostawca chmury (AWS, Azure, GCP) został zidentyfikowany, ale konkretne usługi nie  
- [ ] Tak — konkretne usługi chmurowe (np. kontenery S3, Lambda, Azure Functions) zostały zmapowane i są dostępne

---

## WSTG-CONF-01 — Testowanie konfiguracji infrastruktury sieciowej

Testowanie konfiguracji infrastruktury sieciowej polega na identyfikowaniu słabości w bazowych komponentach serwerowych i sieciowych obsługujących aplikację internetową. Atakujący biorą na cel błędnie skonfigurowane usługi, przestarzałe protokoły i niepotrzebnie otwarte porty, aby uzyskać punkt wejścia, przemieszczać się bocznie lub przeprowadzić rekonesans. Częste znaleziska obejmują niebezpieczne wersje TLS, szczegółowe banery usług (verbose banners) oraz wystawione interfejsy administracyjne, które powinny być ograniczone wyłącznie do sieci wewnętrznych. Wykorzystując te wady na poziomie infrastruktury, przeciwnik może naruszyć poufność danych w tranzycie lub uzyskać nieautoryzowany dostęp do systemu operacyjnego hosta.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Niski / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Narzędzia:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Czy na hoście aplikacji znajdują się niepotrzebne otwarte porty lub wystawione usługi?
- [ ] Nie — dostępne są tylko wymagane porty (np. 443)  
- [ ] Tak — dodatkowe usługi są wystawione, ale posiadają bezpieczną konfigurację  
- [ ] Tak — zbędne usługi (np. FTP, Telnet, SMB) są wystawione publicznie  

### Czy banery usług lub nagłówki ujawniają wrażliwe informacje o wersjach?
- [ ] Nie — banery są usunięte lub ogólne  
- [ ] Tak — banery ujawniają oprogramowanie serwera (np. Apache/nginx), ale nie numery wersji  
- [ ] Tak — szczegółowe banery ujawniają konkretne wersje oprogramowania, ułatwiając eksploatację (Exploit)  

### Czy konfiguracja TLS jest zgodna ze współczesnymi najlepszymi praktykami bezpieczeństwa?
- [ ] Tak — włączone są wyłącznie protokoły TLS 1.2/1.3 z silnymi zestawami szyfrów (cipher suites)  
- [ ] Tak — starsze protokoły (TLS 1.0/1.1) są włączone  
- [ ] Nie — słabe szyfry lub niebezpieczne protokoły (SSLv2/v3) są włączone  

### Czy interfejsy administracyjne lub do zarządzania są ograniczone do autoryzowanych sieci?
- [ ] Tak — interfejsy (np. SSH, RDP, panele sterowania) nie są dostępne z publicznego Internetu  
- [ ] Nie — interfejsy są publicznie dostępne, ale wymagają uwierzytelniania wieloskładnikowego (MFA)  
- [ ] Nie — interfejsy są publicznie dostępne przy użyciu uwierzytelniania jednoskładnikowego lub domyślnych poświadczeń

---

## WSTG-CONF-02 — Testowanie konfiguracji platformy aplikacji

Testowanie konfiguracji platformy aplikacji obejmuje audyt bazowego serwera WWW, serwera aplikacji oraz ustawień frameworka, aby upewnić się, że są one utwardzone (hardened) przeciwko powszechnym exploitom. Atakujący aktywnie poszukują domyślnych danych uwierzytelniających, niezałatanych podatności platformy oraz wystawionych interfejsów administracyjnych, aby uzyskać nieautoryzowany dostęp lub wykonać kod. Błędne konfiguracje często skutkują ujawnieniem wrażliwych zmiennych środowiskowych, wewnętrznych ścieżek systemowych lub obecnością niepotrzebnych przykładowych aplikacji, które zwiększają powierzchnię ataku (attack surface). Z profesjonalnej perspektywy, słabo skonfigurowana platforma służy jako punkt wejścia o wysokim prawdopodobieństwie uzyskania początkowego dostępu do docelowego środowiska.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Średni / Wysoki* |

> *Poziom krytyczności staje się wysoki, jeśli interfejsy administracyjne są dostępne przy użyciu domyślnych danych uwierzytelniających lub jeśli przykładowe skrypty pozwalają na Remote Code Execution.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Czy na platformie aktywne są domyślne dane uwierzytelniające lub konta?
- [ ] Nie — wszystkie domyślne konta są **wyłączone** lub hasła zostały zmienione  
- [ ] Tak — domyślne konta istnieją, ale **nie są dostępne** z sieci zewnętrznej  
- [ ] Tak — domyślne konta są **aktywne** i dostępne przez Internet *(Krytyczne)*  

### Czy platforma ujawnia wrażliwe informacje o wersji poprzez nagłówki lub strony błędów?
- [ ] Nie — ciągi znaków dotyczące wersji są **ukryte** lub generyczne  
- [ ] Tak — szczegółowe informacje o wersji **są ujawniane** w nagłówkach HTTP (np. `Server`, `X-Powered-By`)  
- [ ] Tak — pełne ślady stosu (stack traces) i wewnętrzne ścieżki systemowe **są eksponowane** w szczegółowych komunikatach o błędach  

### Czy interfejsy administracyjne lub zarządzające są odpowiednio ograniczone?
- [ ] Nie — interfejsy administracyjne **nie są wystawione**  
- [ ] Tak — interfejsy istnieją, ale są **ograniczone** przez listę dozwolonych adresów IP (allowlisting) lub uwierzytelnianie wieloskładnikowe (Multi-Factor Authentication)  
- [ ] Tak — interfejsy (np. `/admin`, `/manager`, `/console`) są **publicznie dostępne** z uwierzytelnianiem jednoskładnikowym  

### Czy na serwerze produkcyjnym znajdują się przykładowe pliki, skrypty lub dokumentacja?
- [ ] Nie — wszystkie nieistotne pliki zostały **usunięte**  
- [ ] Tak — przykładowe pliki lub dokumentacja **są obecne**, ale nie powodują wycieku wrażliwych danych  
- [ ] Tak — przykładowe skrypty (np. `info.php`, `examples/`) **są obecne** i stanowią bezpośredni wektor ataku  

### Czy serwer jest skonfigurowany do obsługi niebezpiecznych metod HTTP?
- [ ] Nie — tylko `GET` i `POST` są **włączone**  
- [ ] Tak — potencjalnie ryzykowne metody, takie jak `PUT`, `DELETE` lub `TRACE`, są **włączone**, ale ograniczone  
- [ ] Tak — ryzykowne metody są **włączone** i pozwalają na nieautoryzowaną modyfikację plików lub kradzież danych uwierzytelniających

---

## WSTG-CONF-03 — Testowanie obsługi rozszerzeń plików pod kątem informacji wrażliwych

Testowanie obsługi rozszerzeń plików polega na identyfikacji sytuacji, w których serwer WWW lub serwer aplikacji ujawnia informacje wrażliwe, udostępniając pliki, które powinny być zastrzeżone lub wykonywane. Atakujący często poszukują plików kopii zapasowych (backup), plików konfiguracyjnych oraz fragmentów kodu źródłowego (np. `.bak`, `.old`, `.env`, `.inc`), które mogą zostać pozostawione w katalogu głównym serwera (web root) i udostępnione jako czysty tekst (plain text) z powodu braku odpowiednich mapowań procedur obsługi (handler mappings). Podatność ta jest istotna, ponieważ może prowadzić do ujawnienia poświadczeń bazy danych, kluczy API oraz wewnętrznej logiki biznesowej, ułatwiając dalszą eksploatację (exploit) infrastruktury. Takie wycieki zazwyczaj występują w katalogach publicznych, folderach przesyłania plików (upload) lub wynikają z błędnej konfiguracji typów MIME (MIME-type) na serwerze.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się Wysoki, jeśli dostępne są pliki konfiguracyjne zawierające poświadczenia lub pełny kod źródłowy.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Czy wrażliwe rozszerzenia kopii zapasowych lub plików tymczasowych (np. `.bak`, `.old`, `.swp`, `~`) są dostępne?
- [ ] Nie — serwer zwraca 403 Forbidden lub 404 Not Found dla powszechnych rozszerzeń kopii zapasowych  
- [ ] Tak — serwer pozwala na pobieranie plików kopii zapasowych, ale **nie zawierają** one danych wrażliwych  
- [ ] Tak — serwer pozwala na pobieranie plików kopii zapasowych zawierających dane wrażliwe lub kod źródłowy *(Wysoki)*  

### Czy serwer ujawnia pliki konfiguracyjne środowiska lub systemu (np. `.env`, `.config`, `.yml`, `.ini`)?
- [ ] Nie — dostęp do zastrzeżonych plików konfiguracyjnych **nie jest możliwy** i zwracane jest 403/404  
- [ ] Tak — wrażliwe pliki konfiguracyjne **są** dostępne i ujawniają wewnętrzne tajemnice lub poświadczenia  

### Czy kod źródłowy można pobrać poprzez dodanie alternatywnych rozszerzeń (np. `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] Nie — kod aplikacji **nie jest** renderowany jako czysty tekst, niezależnie od manipulacji rozszerzeniem  
- [ ] Tak — kod źródłowy jest ujawniany, ponieważ serwer **nie posiada** procedury obsługi (handler) dla dodanego rozszerzenia  

### Czy katalogi administracyjne lub metadane (np. `.git/`, `.svn/`, `.DS_Store`) są zablokowane dla dostępu publicznego?
- [ ] Nie — te katalogi/pliki **nie istnieją** w katalogu głównym serwera  
- [ ] Tak — dostęp **nie jest możliwy** ze względu na reguły przepisania (rewrite rules) lub uprawnienia po stronie serwera  
- [ ] Tak — katalogi metadanych **są** dostępne i pozwalają na pełną rekonstrukcję kodu źródłowego  

### Jak serwer obsługuje żądania dotyczące plików z wieloma rozszerzeniami (np. `file.php.jpg`)?
- [ ] Nie — serwer poprawnie priorytetyzuje bezpieczeństwo wewnętrznego rozszerzenia lub blokuje żądanie  
- [ ] Tak — serwer przetwarza plik zgodnie z pierwszym rozszerzeniem, ale obejście (bypass) w celu wykonania kodu **nie jest możliwe**  
- [ ] Tak — serwer ignoruje końcowe rozszerzenie, co **umożliwia** wykonanie kodu lub ujawnienie źródła

---

## WSTG-CONF-04 — Przegląd starych kopii zapasowych i plików niepowiązanych pod kątem wrażliwych informacji

Przegląd starych kopii zapasowych i plików niepowiązanych polega na identyfikacji zapomnianych, tymczasowych lub ukrytych plików na serwerze WWW, które nie są przeznaczone do publicznego dostępu. Pliki te często obejmują kopie zapasowe kodu źródłowego (`.zip`, `.bak`), pliki wymiany edytorów tekstu (swap files: `.swp`, `~`) lub metadane systemów kontroli wersji (`.git`, `.svn`), które mogą prowadzić do wycieku wrażliwych informacji. Atakujący wykorzystują zautomatyzowane listy słów (wordlists) oraz narzędzia do wykrywania zasobów, aby metodą Brute Force odgadnąć popularne konwencje nazewnictwa, dążąc do eksfiltracji danych uwierzytelniających do baz danych, zakodowanych na sztywno kluczy API lub logiki ułatwiającej dalszą eksploatację. Podatność ta zazwyczaj wynika z ręcznej konserwacji serwera lub wadliwych potoków CI/CD (Pipelines), które nie oczyszczają katalogu głównego serwera (web root) w środowisku produkcyjnym.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się Wysoki, jeśli odkryte zostaną pliki konfiguracyjne, archiwa kodu źródłowego lub dane uwierzytelniające.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Czy listowanie zawartości katalogów jest włączone na serwerze WWW?
- [ ] Nie — listowanie katalogów (directory listing) jest **wyłączone** i zwraca błąd 403 Forbidden lub niestandardową stronę błędu  
- [ ] Tak — listowanie katalogów jest **włączone** w katalogach niezawierających wrażliwych danych  
- [ ] Tak — listowanie katalogów jest **włączone** w katalogach zawierających kod źródłowy lub wrażliwe pliki *(Krytyczny)*  

### Czy pliki kopii zapasowych z popularnymi rozszerzeniami (np. .bak, .old, .save) są wykrywalne?
- [ ] Nie — powszechne rozszerzenia kopii zapasowych **nie zostały odnalezione** lub są blokowane przez konfigurację serwera  
- [ ] Tak — pliki kopii zapasowych istnieją, ale **nie** zawierają wrażliwych informacji  
- [ ] Tak — pliki kopii zapasowych konfiguracji lub kodu źródłowego **są** dostępne *(Wysoki)*  

### Czy katalogi systemów kontroli wersji (np. .git, .svn) lub metadane są ujawnione?
- [ ] Nie — metadane systemów kontroli wersji **nie występują** lub są poprawnie blokowane  
- [ ] Tak — metadane istnieją, ale pełne repozytorium **nie może** zostać odtworzone  
- [ ] Tak — całe repozytorium kodu źródłowego **może** zostać wyeksfiltrowane poprzez ujawnione metadane  

### Czy w katalogu głównym aplikacji (web root) znajdują się skompresowane archiwa (np. .zip, .tar.gz, .rar)?
- [ ] Nie — nie wykryto wrażliwych plików archiwów za pomocą ataków brute-force ani enumeracji  
- [ ] Tak — znaleziono archiwa, ale są one chronione hasłem lub zawierają publiczne zasoby  
- [ ] Tak — niezaszyfrowane archiwa zawierające kod źródłowy lub dane aplikacji **są** publicznie dostępne  

### Czy aplikacja ujawnia pliki tymczasowe utworzone przez edytory tekstu lub środowiska IDE?
- [ ] Nie — pliki tymczasowe takie jak `.swp`, `~` lub `.DS_Store` **nie występują**  
- [ ] Tak — pliki tymczasowe niezawierające wrażliwych danych **są obecne**  
- [ ] Tak — pliki tymczasowe ujawniają fragmenty kodu źródłowego lub ścieżki wewnętrzne

---

## WSTG-CONF-05 — Enumeracja interfejsów administracyjnych infrastruktury i aplikacji

Interfejsy administracyjne stanowią warstwę zarządzania (management control plane) aplikacji oraz jej podstawowej infrastruktury, często przyznając uprawniony dostęp do konfiguracji systemu, danych użytkowników i operacji po stronie serwera. Atakujący priorytetyzują enumerację tych interfejsów poprzez ataki typu Brute Force na katalogi, analizę pliku `robots.txt` lub obserwację przewidywalnych wzorców URL, aby znaleźć punkty wejścia, które mogą nie posiadać solidnego uwierzytelniania lub opierać się na domyślnych danych uwierzytelniających. Wykrycie wyeksponowanego panelu administracyjnego znacząco zwiększa ryzyko całkowitej kompromitacji systemu, nieautoryzowanej modyfikacji danych lub zakłócenia usług. Interfejsy te często znajdują się na niestandardowych portach lub subdomenach, co czyni je głównymi celami dla zautomatyzowanych skanerów i rekonesansu manualnego.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Status testu** | Nie przeprowadzono |
| **Dotkliwość** | Średnia / Wysoka* |

> *Dotkliwość staje się wysoka, jeśli interfejs pozwala na zmiany konfiguracji w skali całego systemu, zarządzanie użytkownikami lub eksfiltrację danych bez uwierzytelniania wieloskładnikowego (MFA).

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Czy interfejsy administracyjne są możliwe do wykrycia poprzez fuzzing katalogów lub rekonesans?
- [ ] Nie — żadne interfejsy administracyjne nie są wyeksponowane ani możliwe do wykrycia  
- [ ] Tak — interfejsy są możliwe do wykrycia, ale dostęp **jest ograniczony** do wewnętrznych zakresów adresów IP  
- [ ] Tak — interfejsy **są** możliwe do wykrycia i publicznie dostępne  

### Czy na wykrytych portalach administracyjnych wymuszane jest uwierzytelnianie?
- [ ] Tak — uwierzytelnianie wieloskładnikowe (MFA) **jest wymuszane**  
- [ ] Tak — uwierzytelnianie jednoskładnikowe **jest wymuszane**  
- [ ] Nie — uwierzytelnianie nie jest wymagane do uzyskania dostępu do interfejsu *(Krytyczne)*  

### Czy ścieżki administracyjne są ukryte lub niestandardowe, aby zapobiec wykryciu?
- [ ] Tak — ścieżki wykorzystują niestandardowe, zrandomizowane lub nieprzewidywalne nazewnictwo  
- [ ] Nie — ścieżki wykorzystują powszechne konwencje nazewnictwa (np. `/admin`, `/manager`, `/console`) i **mogą** być łatwo odgadnięte  

### Czy interfejs zapewnia dostęp do wrażliwych funkcjonalności systemu lub aplikacji?
- [ ] Nie — interfejs jest panelem statusu typu "read-only" o minimalnym wpływie  
- [ ] Tak — interfejs **może** modyfikować konfigurację aplikacji lub dane użytkowników  
- [ ] Tak — interfejs **może** wykonywać polecenia na poziomie systemu lub zarządzać bazą danych

---

## WSTG-CONF-06 — Testowanie metod HTTP

Testowanie metod HTTP polega na identyfikacji czasowników (verbs) obsługiwanych przez serwer WWW i aplikację, aby upewnić się, że udostępniona jest wyłącznie niezbędna funkcjonalność. Poza standardowymi metodami `GET` i `POST`, serwery mogą nieumyślnie włączać niebezpieczne metody, takie jak `PUT`, `DELETE` lub `TRACE`, które mogą pozwolić atakującym na przesyłanie złośliwych plików, usuwanie istniejącej zawartości lub eksfiltrację ciasteczek sesyjnych poprzez Cross-Site Tracing (XST). Pentesterzy badają nagłówki odpowiedzi, takie jak `Allow` lub `Public`, i próbują ominąć ograniczenia, stosując techniki Method Overriding lub Verb Tampering w celu uzyskania dostępu do nieautoryzowanych funkcjonalności. Ten test jest kluczowy dla utwardzania powierzchni ataku i zapobiegania błędom konfiguracji, które mogłyby prowadzić do przejęcia serwera lub nieautoryzowanej manipulacji danymi.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Niski / Średni* |

> *Poziom krytyczności staje się wysoki (High), jeśli `PUT` lub `DELETE` pozwalają na nieautoryzowaną manipulację plikami lub jeśli `TRACE` ułatwia eksfiltrację tokenów sesyjnych.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Czy niebezpieczne metody HTTP, takie jak `PUT` lub `DELETE`, są wyłączone w całej aplikacji?
- [ ] Tak — włączone są wyłącznie bezpieczne metody (GET, POST, HEAD)  
- [ ] Nie — metody `PUT` lub `DELETE` **są włączone**, ale wymagają poprawnego uwierzytelnienia  
- [ ] Nie — metody `PUT` lub `DELETE` **są włączone** i dostępne bez uwierzytelnienia *(Krytyczny)*  

### Czy metoda `TRACE` jest wyłączona, aby zapobiec Cross-Site Tracing (XST)?
- [ ] Tak — metoda `TRACE` **jest wyłączona** lub zwraca kod 405 Method Not Allowed  
- [ ] Nie — metoda `TRACE` **jest włączona**, ale nie zwraca wrażliwych nagłówków  
- [ ] Nie — metoda `TRACE` **jest włączona** i zwraca nagłówki `Cookie` lub `Authorization` *(Średni)*  

### Czy serwer obsługuje nadpisywanie metod HTTP (np. `X-HTTP-Method-Override`)?
- [ ] Nie — serwer **nie** przetwarza nagłówków nadpisywania metod  
- [ ] Tak — serwer przetwarza nagłówki nadpisywania, ale mechanizmy kontroli dostępu **są egzekwowane**  
- [ ] Tak — serwer przetwarza nagłówki nadpisywania i możliwe jest **obejście kontroli dostępu**  

### Czy serwer odpowiada w bezpieczny sposób na dowolne lub błędnie sformułowane metody HTTP?
- [ ] Tak — serwer zwraca kod 405 Method Not Allowed lub 501 Not Implemented  
- [ ] Nie — serwer zwraca kod 200 OK lub 500 Error dla dowolnych metod, takich jak `JEFF` lub `TEST`  
- [ ] Nie — użycie dowolnych metod pozwala na obejście filtrów uwierzytelniania lub autoryzacji  

### Czy metoda `OPTIONS` jest ograniczona lub skonfigurowana tak, aby zminimalizować ujawnianie informacji?
- [ ] Tak — metoda `OPTIONS` jest wyłączona lub zwraca minimalny nagłówek `Allow`  
- [ ] Nie — metoda `OPTIONS` ujawnia szeroki zakres włączonych metod, w tym czasowniki DAV lub debugowania

---

## WSTG-CONF-07 — Testowanie mechanizmu HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) to mechanizm bezpieczeństwa, który pozwala serwerowi WWW poinformować przeglądarki, że dostęp do niego powinien odbywać się wyłącznie przez protokół HTTPS, co zapobiega atakom typu protocol downgrade oraz przejmowaniu ciasteczek (cookie hijacking). Wymuszając szyfrowane połączenie, HSTS mityguje ataki typu Man-in-the-Middle (MitM), takie jak SSLStrip, w których atakujący przechwytuje początkowe nieszyfrowane żądanie HTTP, aby zapobiec przejściu na protokół TLS. Z perspektywy atakującego, brak tego nagłówka lub krótki czas trwania `max-age` stwarza okazję do przechwycenia wrażliwych tokenów sesyjnych lub poświadczeń podczas początkowego uścisku dłoni (handshake) przesyłanego tekstem jawnym (cleartext). Niniejszy test koncentruje się na weryfikacji obecności, czasu trwania oraz zakresu nagłówka `Strict-Transport-Security` we wszystkich wrażliwych punktach końcowych (endpoints) aplikacji.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Niski / Średni* |

> *Poziom krytyczności staje się Średni, jeśli aplikacja przetwarza dane osobowe (PII), tokeny sesyjne lub poświadczenia, ponieważ brak HSTS zwiększa szansę powodzenia ataków MitM.

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### Czy nagłówek `Strict-Transport-Security` jest obecny w odpowiedziach HTTPS?
- [ ] Tak — nagłówek **jest obecny** we wszystkich wrażliwych punktach końcowych  
- [ ] Tak — nagłówek **jest obecny**, ale tylko na stronie głównej  
- [ ] Nie — w aplikacji całkowicie **brakuje** nagłówka  

### Czy dyrektywa `max-age` jest ustawiona na wystarczający czas trwania?
- [ ] Tak — `max-age` jest ustawiony na rok lub dłużej (np. `31536000`)  
- [ ] Tak — `max-age` jest ustawiony, ale jest **zbyt krótki** (np. mniej niż 6 miesięcy)  
- [ ] Nie — dyrektywy `max-age` **brakuje** lub jest ustawiona na `0` *(Krytyczny)*  

### Czy polityka HSTS obejmuje wszystkie subdomeny?
- [ ] Tak — dyrektywa `includeSubDomains` **została zastosowana**  
- [ ] Nie — dyrektywy `includeSubDomains` **brakuje**, co naraża subdomeny na ataki typu downgrade  
- [ ] Nie — funkcja nie ma zastosowania, ponieważ subdomeny nie istnieją  

### Czy aplikacja jest zarejestrowana w mechanizmie HSTS Preloading?
- [ ] Tak — dyrektywa `preload` **jest obecna**, a domena znajduje się na liście HSTS preload  
- [ ] Tak — dyrektywa `preload` **jest obecna**, ale domena **nie znajduje się** jeszcze na liście preload  
- [ ] Nie — dyrektywy `preload` **brakuje**, co pozostawia lukę dla ataku MitM podczas pierwszej wizyty  

### Czy nagłówek HSTS jest błędnie przesyłany przez nieszyfrowany protokół HTTP?
- [ ] Nie — nagłówek jest przesyłany **wyłącznie** przez bezpieczne połączenia HTTPS  
- [ ] Tak — nagłówek **jest przesyłany** przez HTTP, co jest ignorowane przez przeglądarki i może ujawniać szczegóły konfiguracji

---

## WSTG-CONF-08 — Testowanie polityki Cross-Domain w aplikacjach RIA

Polityki cross-domain w aplikacjach typu Rich Internet Application (RIA) obejmują pliki takie jak `crossdomain.xml` oraz `clientaccesspolicy.xml`, które definiują, w jaki sposób klienci webowi — w szczególności Adobe Flash i Microsoft Silverlight — obsługują żądania cross-origin. Pliki te są kluczowe dla bezpieczeństwa, ponieważ jawnie nadpisują mechanizm Same-Origin Policy (SOP), określając, które domeny zewnętrzne mają zezwolenie na interakcję z aplikacją i odczytywanie jej odpowiedzi. Zbyt liberalne konfiguracje, takie jak użycie symboli wieloznacznych (wildcards) w atrybucie `domain`, pozwalają atakującym na umieszczenie złośliwych obiektów RIA w zewnętrznej witrynie, co może umożliwić eksfiltrację wrażliwych danych lub wykonywanie działań w imieniu uwierzytelnionych użytkowników. Pentesterzy zazwyczaj lokalizują te pliki w katalogu głównym serwera (web root), aby ocenić poziom zaufania przyznany zewnętrznym źródłom (origins) i zidentyfikować potencjalne wycieki danych typu cross-origin.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się Wysoki, jeśli aplikacja przetwarza wrażliwe dane użytkowników lub identyfikatory sesji, które mogą zostać eksfiltrowane za pomocą zbyt liberalnej polityki.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Czy pliki polityki cross-domain (`crossdomain.xml` lub `clientaccesspolicy.xml`) znajdują się w katalogu głównym?
- [ ] Nie — nie odnaleziono plików w katalogu głównym (root)
- [ ] Tak — plik `crossdomain.xml` (Flash) jest obecny
- [ ] Tak — plik `clientaccesspolicy.xml` (Silverlight) jest obecny
- [ ] Tak — oba pliki polityki RIA są obecne

### Czy atrybut domeny `allow-access-from` jest ograniczony do zaufanych źródeł?
- [ ] Nie — pliki istnieją, ale nie zawierają wpisów `allow-access-from`
- [ ] Tak — konkretne, zaufane domeny są jawnie dopuszczone (whitelisted) i obejście nie jest możliwe
- [ ] Tak — domeny są na białej liście, ale obejmują niezaufane podmioty trzecie lub subdomeny
- [ ] Tak — użyto symbolu wieloznacznego `*`, co pozwala na dostęp z dowolnego źródła (Krytyczne)

### Czy polityka zezwala na niezabezpieczoną komunikację przez HTTP dla zasobów HTTPS?
- [ ] Nie — ustawiono `secure="true"` (lub jest to wartość domyślna), co uniemożliwia źródłom HTTP dostęp do danych HTTPS
- [ ] Tak — jawnie ustawiono `secure="false"`, co umożliwia atakującym typu MITM eksfiltrację bezpiecznych danych

### Czy nagłówki HTTP są skonfigurowane zbyt liberalnie w polityce cross-domain?
- [ ] Nie — `allow-http-request-headers-from` jest ograniczone lub nie zostało włączone
- [ ] Tak — określone nagłówki są dozwolone dla zaufanych domen
- [ ] Tak — użyto symbolu wieloznacznego `*` dla nagłówków, co pozwala atakującym na wysyłanie niestandardowych nagłówków z dowolnej domeny

### Czy konfiguracja `site-control` (master-policy) jest restrykcyjna?
- [ ] Nie — `permitted-cross-domain-policies` jest ustawione na `none` lub `master-only`
- [ ] Tak — polityka zezwala innym plikom na serwerze na definiowanie własnych reguł (`all`)

---

## WSTG-CONF-09 — Testowanie uprawnień do plików

Testowanie uprawnień do plików polega na audycie poziomów kontroli dostępu przypisanych do plików i katalogów na serwerze WWW, aby upewnić się, że wrażliwe zasoby nie są nadmiernie wystawione na działanie nieautoryzowanych użytkowników lub procesów. Nieprawidłowo skonfigurowane uprawnienia mogą umożliwić atakującym odczyt wrażliwych plików konfiguracyjnych zawierających dane uwierzytelniające do bazy danych, przeglądanie zastrzeżonego kodu źródłowego lub modyfikację skryptów po stronie serwera w celu uzyskania zdalnego wykonania kodu (Remote Code Execution). Podatność ta występuje zazwyczaj w fazie wdrażania, gdy domyślne flagi „world-readable” lub „world-writable” zostaną pozostawione w krytycznych katalogach aplikacji, takich jak ścieżki konfiguracyjne, foldery logów lub repozytoria przesyłanych plików. Z perspektywy atakującego, odkrycie niewłaściwie zabezpieczonego pliku często służy jako główny katalizator eskalacji uprawnień (Privilege Escalation) lub pełnego przejęcia systemu.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się wysoki, jeśli wrażliwe pliki konfiguracyjne (np. `.env`, `web.config`) są czytelne lub jeśli możliwy jest dostęp do zapisu w katalogu głównym serwera WWW (web root).

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Narzędzia:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Czy wrażliwe pliki konfiguracyjne aplikacji są chronione przed nieautoryzowanym dostępem?
- [ ] Tak — pliki konfiguracyjne (np. `.env`, `config.php`) **nie są dostępne** poprzez żądania WWW  
- [ ] Tak — pliki istnieją, ale dostęp jest **ograniczony** wyłącznie do uprawnionych użytkowników lokalnych  
- [ ] Nie — wrażliwe pliki konfiguracyjne **są dostępne** i ujawniają dane uwierzytelniające lub sekrety  

### Czy atakujący może modyfikować pliki lub katalogi wewnątrz katalogu głównego (web root)?
- [ ] Nie — wszystkie katalogi w web root są **tylko do odczytu** dla użytkownika serwera WWW  
- [ ] Tak — istnieją katalogi z uprawnieniami do zapisu, ale wykonywanie skryptów jest **wyłączone**  
- [ ] Tak — istnieją katalogi typu world-writable i **pozwalają** na przesyłanie oraz wykonywanie skryptów *(Krytyczne)*  

### Czy metadane systemów kontroli wersji lub pliki kopii zapasowych są ujawnione z powodu zbyt pobłażliwych uprawnień?
- [ ] Nie — `.git`, `.svn` oraz pliki kopii zapasowych (np. `.bak`, `~`) **nie występują** lub są chronione  
- [ ] Tak — katalogi metadanych istnieją, ale listowanie zawartości katalogów (directory listing) jest **wyłączone**  
- [ ] Tak — wrażliwe metadane lub kopie zapasowe są **w pełni dostępne**, co pozwala na odzyskanie kodu źródłowego  

### Czy użytkownik serwera WWW działa zgodnie z zasadą najmniejszych uprawnień (principle of least privilege)?
- [ ] Tak — proces serwera WWW działa jako **dedykowany użytkownik o niskich uprawnieniach**  
- [ ] Nie — proces serwera WWW działa z **niepotrzebnymi uprawnieniami** (np. `root` lub `Administrator`)  

### Czy pliki logów są zabezpieczone przed nieautoryzowanym odczytem lub modyfikacją?
- [ ] Tak — dostęp do plików logów jest **ograniczony** do użytkowników administracyjnych  
- [ ] Tak — pliki logów są czytelne, ale **nie zawierają** tokenów sesyjnych ani danych osobowych (PII)  
- [ ] Nie — pliki logów są **publicznie dostępne do odczytu** i ujawniają wrażliwe dane transakcyjne lub informacje o użytkownikach

---

## WSTG-CONF-10 — Subdomain Takeover

Przejęcie subdomeny (Subdomain Takeover) występuje, gdy wpis DNS (zazwyczaj CNAME, ale sporadycznie rekordy A lub MX) wskazuje na wycofany z eksploatacji lub nieistniejący zasób u zewnętrznego dostawcy usług chmurowych lub hostingowych. Atakujący identyfikują te „wiszące” (dangling) rekordy DNS, szukając konkretnych komunikatów o błędach od dostawców takich jak AWS, GitHub lub Azure, które wskazują, że zasób nie jest już zajęty. Poprzez zarejestrowanie porzuconej nazwy zasobu na platformie dostawcy, atakujący uzyskuje pełną kontrolę nad subdomeną, co pozwala mu na hostowanie złośliwych treści, eksfiltrację ciasteczek sesyjnych (session cookies) przypisanych do domeny głównej oraz obchodzenie zabezpieczeń Content Security Policy (CSP) lub CORS. Podatność ta jest szczególnie niebezpieczna, ponieważ wykorzystuje zaufanie związane z legalną strukturą domenową organizacji w celu ułatwienia wysokiej skali ataków phishingowych i kradzieży danych.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Status testu** | Nie przeprowadzono |
| **Poziom zagrożenia** | Wysoki / Krytyczny* |

> *Poziom zagrożenia staje się krytyczny, jeśli subdomena może zostać użyta do przejęcia ciasteczek sesyjnych z domeny głównej lub obejścia przekierowań SSO/OAuth.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Narzędzia:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### Czy aplikacja korzysta z zewnętrznych usług hostingowych lub chmurowych za pośrednictwem subdomen?
- [ ] Nie — wszystkie subdomeny wskazują na przestrzeń adresową IP kontrolowaną przez organizację  
- [ ] Tak — subdomeny wskazują na zewnętrznych dostawców (S3, GitHub Pages, Heroku itp.)  

### Czy istnieją jakiekolwiek „wiszące” (dangling) rekordy DNS wskazujące na nieistniejące zasoby?
- [ ] Nie — wszystkie zidentyfikowane rekordy DNS wskazują na aktywne, kontrolowane zasoby  
- [ ] Tak — rekordy CNAME lub A istnieją dla zasobów, które **już nie istnieją** u dostawcy  
- [ ] Tak — rekordy DNS zwracają status NXDOMAIN lub strony błędów specyficzne dla dostawcy *(np. „NoSuchBucket”)*  

### Czy zidentyfikowany wiszący zasób może zostać pomyślnie zajęty?
- [ ] Nie — dostawca posiada zabezpieczenia przed zajmowaniem porzuconych nazw lub wymagana jest weryfikacja konta  
- [ ] Tak — nazwa zasobu **może** zostać zarejestrowana na platformie zewnętrznej przez dowolnego użytkownika  

### Czy domena główna używa ciasteczek o zasięgu „Domain”, które mogłyby zostać przejęte?
- [ ] Nie — ciasteczka mają ściśle określony zasięg dla konkretnych subdomen lub używają flag `Host-Only`  
- [ ] Tak — wrażliwe ciasteczka (identyfikatory sesji, JWT) są przypisane do domeny nadrzędnej i **mogą** zostać wyeksfiltrowane za pośrednictwem przejętej subdomeny  

### Czy subdomena może zostać użyta do obejścia nagłówków bezpieczeństwa lub procesów uwierzytelniania?
- [ ] Nie — brak zależności bezpieczeństwa od zidentyfikowanych subdomen  
- [ ] Tak — subdomena znajduje się na białej liście w dyrektywach CSP `script-src` lub `connect-src`  
- [ ] Tak — subdomena jest prawidłowym URI przekierowania (redirect URI) dla konfiguracji OAuth lub SSO

---

## WSTG-CONF-11 — Testowanie przechowywania w chmurze (Cloud Storage)

Testowanie pod kątem błędnych konfiguracji przechowywania w chmurze (Cloud Storage) obejmuje identyfikację i audyt publicznie dostępnych usług przechowywania obiektów (Object Storage), takich jak Amazon S3, Azure Blobs lub Google Cloud Storage. Zasoby te często zawierają wrażliwe dane, w tym kopie zapasowe, kod źródłowy, dane osobowe (PII) lub pliki konfiguracyjne, które są narażone z powodu nadmiernie permisywnych list kontroli dostępu (ACL) lub polityk zarządzania tożsamością i dostępem (IAM). Atakujący enumerują nazwy kontenerów (Buckets) poprzez rekonesans plików JavaScript, źródła HTML oraz techniki Brute Force w celu znalezienia punktów wejścia do eksfiltracji danych lub nieautoryzowanej modyfikacji plików. Eksploitacja może prowadzić do znaczących skutków, w tym pełnych wycieków danych lub możliwości serwowania złośliwych plików z zaufanej domeny.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Status testu** | Nie przeprowadzono |
| **Poziom zagrożenia** | Wysoki / Krytyczny* |

> *Poziom zagrożenia staje się krytyczny, jeśli dostępne są dane osobowe (PII), dane uwierzytelniające lub produkcyjne kopie zapasowe, lub jeśli włączony jest nieuwierzytelniony dostęp do zapisu.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Czy punkty końcowe przechowywania w chmurze (Buckets/Containers) zostały zidentyfikowane i wyenumerowane?
- [ ] Nie — nie użyto żadnych punktów końcowych przechowywania w chmurze lub nie są one wykrywalne  
- [ ] Tak — punkty końcowe zostały zidentyfikowane poprzez analizę kodu lub monitorowanie ruchu  
- [ ] Tak — punkty końcowe zostały odkryte za pomocą techniki Brute Force opartej na konwencji nazewnictwa  

### Czy nieuwierzytelnieni użytkownicy mogą wyświetlić zawartość magazynu chmurowego (Listing)?
- [ ] Nie — listowanie kontenera (Bucket Listing) jest **wyłączone** i zwraca 403 Forbidden lub 404 Not Found  
- [ ] Tak — listowanie jest **włączone**, ale nie ma wrażliwych plików  
- [ ] Tak — listowanie jest **włączone**, a wrażliwe nazwy plików/metadane są widoczne  

### Czy możliwe jest pobieranie wrażliwych plików ze zidentyfikowanych kontenerów?
- [ ] Nie — pliki są chronione przez polityki IAM lub wymagane jest prawidłowe uwierzytelnienie  
- [ ] Tak — pliki są dostępne, ale wymagają podpisanego adresu URL (Signed URL), którego **nie można** łatwo odgadnąć  
- [ ] Tak — wrażliwe pliki (kopie zapasowe, `.env`, PII) są **publicznie dostępne** do pobrania *(Krytyczny)*  

### Czy dozwolone jest nieuwierzytelnione przesyłanie lub modyfikowanie plików?
- [ ] Nie — nieuwierzytelnione przesyłanie (Upload) lub modyfikacje **nie są możliwe**  
- [ ] Tak — wymagane jest uwierzytelnione przesyłanie, ale obejście (Bypass) **jest możliwe** ze względu na słabe warunki polityki  
- [ ] Tak — nieuwierzytelnione zapisywanie, nadpisywanie lub usuwanie plików **jest możliwe** *(Krytyczny)*  

### Czy polityki współdzielenia zasobów między źródłami (CORS) w punkcie końcowym magazynu są restrykcyjne?
- [ ] Tak — CORS jest **wyłączony** lub ograniczony do określonych, zaufanych źródeł (Origins)  
- [ ] Nie — CORS jest **włączony** z symbolem wieloznacznym (Wildcard) `*`, co pozwala na nieautoryzowany dostęp z poziomu przeglądarki

---

## WSTG-CONF-12 — Content Security Policy (CSP)

Content Security Policy (CSP) jest krytycznym mechanizmem głębokiej obrony (defense-in-depth) wdrażanym za pomocą nagłówków odpowiedzi HTTP w celu mitygacji ryzyk takich jak Cross-Site Scripting (XSS), clickjacking oraz ataki typu data injection. Poprzez definiowanie, które zasoby dynamiczne mogą być ładowane, dobrze skonfigurowana polityka CSP ogranicza zdolność atakującego do wykonywania nieautoryzowanych skryptów lub eksfiltracji wrażliwych danych do zewnętrznych domen. Pentesterzy oceniają politykę pod kątem nadmiernie permisywnych dyrektyw, użycia niebezpiecznych słów kluczowych, takich jak `'unsafe-inline'`, oraz polegania na niezabezpieczonych sieciach CDN, które mogą ułatwiać obejścia (bypass). Słaba lub brakująca polityka CSP nie tworzy bezpośrednio podatności, ale znacząco zwiększa wpływ udanych ataków typu injection poprzez brak zapewnienia drugorzędnej warstwy ochrony.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Niski / Średni |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Narzędzia:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Czy nagłówek Content Security Policy (CSP) jest obecny w odpowiedziach aplikacji?
- [ ] Tak — nagłówek `Content-Security-Policy` jest **obecny** i **wymuszany**  
- [ ] Tak — nagłówek `Content-Security-Policy-Report-Only` jest **obecny** w celach testowych  
- [ ] Nie — nagłówek CSP nie jest **obecny** w odpowiedziach  

### Czy dyrektywy `script-src` lub `default-src` są odpowiednio ograniczone?
- [ ] Tak — dyrektywy wykorzystują rygorystyczne białe listy, wartości nonce lub skróty (hash), a obejście (bypass) **nie jest możliwe**  
- [ ] Tak — dyrektywy są **prawidłowo skonfigurowane**, ale biała lista zawiera znane sieci CDN, które można obejść  
- [ ] Nie — dyrektywy używają symboli wieloznacznych (`*`) lub schematów `data:`, co sprawia, że obejście jest **możliwe**  

### Czy polityka zezwala na wykonywanie niebezpiecznych skryptów wbudowanych (inline) lub funkcji eval?
- [ ] Nie — `'unsafe-inline'` oraz `'unsafe-eval'` **nie występują**  
- [ ] Tak — `'unsafe-inline'` jest **obecny**, ale chroniony przez wartość `nonce` lub `hash`  
- [ ] Tak — `'unsafe-inline'` lub `'unsafe-eval'` są **włączone** bez dodatkowych zabezpieczeń *(Krytyczne)*  

### Czy aplikacja jest chroniona przed atakami clickjacking za pomocą dyrektywy `frame-ancestors`?
- [ ] Tak — dyrektywa `frame-ancestors` jest ustawiona na `'none'` lub `'self'`  
- [ ] Tak — dyrektywa `frame-ancestors` jest **włączona**, ale zezwala na określone, zaufane domeny stron trzecich  
- [ ] Nie — dyrektywa `frame-ancestors` jest **nieobecna**, aplikacja polega na przestarzałym nagłówku `X-Frame-Options` lub brakuje ochrony  

### Czy istnieje mechanizm raportowania naruszeń CSP?
- [ ] Tak — dyrektywa `report-uri` lub `report-to` jest **skonfigurowana** i aktywna  
- [ ] Nie — raportowanie naruszeń jest **wyłączone** lub **nieskonfigurowane**

---

## WSTG-CONF-13 — Path Confusion

Path Confusion (pomieszanie ścieżek) powstaje w wyniku rozbieżności w sposobie, w jaki różne komponenty internetowe, takie jak serwery reverse proxy, load balancery i serwery aplikacji backendowej, parsują i interpretują ścieżki URL. Atakujący wykorzystują te niespójności, wstrzykując określone znaki, takie jak średniki, zakodowane ukośniki (encoded slashes) lub dot-segments, aby oszukać filtry bezpieczeństwa i uzyskać dostęp do zastrzeżonych punktów końcowych (endpoints) lub zasobów statycznych. Podatność ta może prowadzić do krytycznych błędów bezpieczeństwa, w tym ominięcia uwierzytelniania (authentication bypass), nieautoryzowanego dostępu do danych oraz zatruwania pamięci podręcznej WWW (Web Cache Poisoning), ponieważ warstwa front-end może stosować reguły bezpieczeństwa do ścieżki, którą back-end interpretuje inaczej. Skuteczna eksploatacja zazwyczaj następuje na granicy architektonicznej, gdzie logika routingu i normalizacji żądań różni się w obrębie stosu technologicznego.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom krytyczności** | Średni / Wysoki* |

> *Poziom krytyczności staje się wysoki, jeśli path confusion skutkuje ominięciem uwierzytelniania lub kontroli dostępu do wrażliwych punktów końcowych administracyjnych.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Czy różne warstwy architektury interpretują separatory ścieżek (np. `;`, `#`, `?`) w sposób spójny?
- [ ] Tak — wszystkie warstwy normalizują i interpretują separatory ścieżek identycznie  
- [ ] Nie — istnieją niespójności, ale **nie mogą** one zostać wykorzystane do uzyskania dostępu do zastrzeżonych zasobów  
- [ ] Nie — rozbieżności pozwalają atakującemu na ominięcie filtrów front-end i dotarcie do logiki back-end **jest możliwe**  

### Czy mechanizmy kontroli dostępu mogą zostać pominięte przy użyciu sekwencji path traversal lub zakodowanych znaków?
- [ ] Nie — normalizacja jest stosowana spójnie przed sprawdzeniem zabezpieczeń  
- [ ] Tak — ominięcie jest **możliwe** poprzez sekwencje dot-segment (np. `/admin/..;/`)  
- [ ] Tak — ominięcie jest **możliwe** poprzez znaki zakodowane w adresie URL (np. `%2f`, `%2e%2e%2f`)  

### Czy aplikacja jest podatna na Web Cache Poisoning poprzez path confusion?
- [ ] Nie — logika buforowania nie jest podatna na niejednoznaczność ścieżek ani dodatkowe informacje o ścieżce  
- [ ] Tak — złośliwa treść **może** zostać umieszczona w pamięci podręcznej dla legalnych ścieżek z powodu rozbieżności w mapowaniu  
- [ ] Tak — wrażliwe informacje **mogą** być buforowane w katalogach publicznych poprzez techniki `RCD` (Relative Path Overwrite)  

### Czy serwer bezpiecznie obsługuje „dodatkowe informacje o ścieżce” (PathInfo)?
- [ ] Nie — funkcja jest **wyłączona** lub nie istnieje  
- [ ] Tak — `PathInfo` jest obsługiwane i **nie zakłóca** działania filtrów bezpieczeństwa  
- [ ] Nie — `PathInfo` pozwala atakującemu na dołączenie danych, które dezorientują logikę routingu aplikacji

---

## WSTG-CONF-14 — Testowanie błędnych konfiguracji pozostałych nagłówków bezpieczeństwa HTTP

Testowanie błędnych konfiguracji nagłówków bezpieczeństwa HTTP polega na weryfikacji obecności i poprawnej implementacji nagłówków obronnych, które zapewniają niezbędną ochronę przed typowymi wektorami ataków internetowych. Chociaż nagłówki te nie naprawiają podatności w samym kodzie źródłowym, ich brak znacząco zwiększa ryzyko ataków typu man-in-the-middle (MITM), eksploitacji mechanizmu MIME-sniffing oraz niezamierzonego wycieku danych cross-origin poprzez nagłówki referrer. Z perspektywy atakującego, brakujące nagłówki bezpieczeństwa są wyraźnymi wskaźnikami słabo zabezpieczonego środowiska, co często ułatwia bardziej złożone łańcuchy ataków, takie jak session hijacking lub eksfiltracja informacji. Konfiguracje te są zazwyczaj oceniane na poziomie serwera i powinny być konsekwentnie stosowane we wszystkich typach odpowiedzi, w tym w punktach końcowych API i zasobach statycznych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Status testu** | Nie przeprowadzono |
| **Dotkliwość** | Niska / Średnia |

**Materiały źródłowe:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audyt obecności nagłówków bezpieczeństwa
| Nagłówek | Obecny | Brak | Zalecana konfiguracja |
|----------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` lub `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Zależnie od funkcji, np. `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### W jaki sposób nagłówki bezpieczeństwa są wymuszane w całym zakresie aplikacji?
- [ ] Tak — nagłówki są stosowane globalnie za pomocą centralnego mechanizmu load balancer lub reverse proxy i **nie mogą** zostać nadpisane  
- [ ] Tak — nagłówki są stosowane w odpowiedziach dla głównych dokumentów, ale **brak** ich w odpowiedziach API lub zasobach statycznych  
- [ ] Nie — nagłówki są stosowane niespójnie lub ich **brakuje** w całej domenie aplikacji  

### Czy nagłówek Strict-Transport-Security (HSTS) jest poprawnie skonfigurowany?
- [ ] Tak — parametr `max-age` jest ustawiony na długi czas (np. 2 lata), a `includeSubDomains` jest **włączony**  
- [ ] Tak — parametr `max-age` jest ustawiony, ale `includeSubDomains` jest **wyłączony**, co pozostawia subdomeny podatnymi na ataki  
- [ ] Nie — nagłówek HSTS **nie występuje** lub `max-age` jest ustawiony na 0, co pozwala na ataki typu protocol downgrade  

### Czy Referrer-Policy zapobiega wyciekowi wrażliwych parametrów URL?
- [ ] Tak — polityka jest ustawiona na `no-referrer` lub `same-origin` *(Najbardziej bezpieczne)*  
- [ ] Tak — polityka jest ustawiona na `strict-origin-when-cross-origin`  
- [ ] Nie — polityka **nie jest ustawiona** lub jest ustawiona na `unsafe-url`, co może prowadzić do wycieku tokenów lub wrażliwych danych PII w nagłówkach referer  

### Czy nagłówek X-Content-Type-Options zapobiega mechanizmowi MIME-sniffing?
- [ ] Tak — dyrektywa `nosniff` jest **obecna** i poprawnie zaimplementowana  
- [ ] Nie — nagłówka **brakuje**, co potencjalnie pozwala przeglądarce na wykonywanie plików niewykonywalnych (np. obrazów) jako skryptów

---

## WSTG-IDNT-01 — Testowanie definicji ról

Testowanie definicji ról polega na identyfikacji i dokumentowaniu różnych ról użytkowników oraz poziomów uprawnień zdefiniowanych w aplikacji, aby upewnić się, że są one logicznie odseparowane i zgodne z zasadą najniższych uprawnień (principle of least privilege). Atakujący analizuje aplikację w celu zmapowania ról o wysokich uprawnieniach w zestawieniu ze standardowymi rolami użytkowników, szukając wszelkich niejasności w granicach uprawnień lub nieudokumentowanych funkcji administracyjnych. Identyfikacja tych ról jest pierwszym krokiem do odkrycia ścieżek eskalacji uprawnień (privilege escalation), ponieważ niespójności w definicjach ról często prowadzą do nieautoryzowanego dostępu do wrażliwych danych lub funkcji administracyjnych. Faza ta zazwyczaj odbywa się podczas wstępnego rekonesansu i mapowania logiki biznesowej, koncentrując się na profilach użytkowników, panelach administracyjnych oraz strukturach uprawnień API.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Status testu** | Nieprzeprowadzony |
| **Dotkliwość** | Niski / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Czy role są wyraźnie zdefiniowane i udokumentowane w aplikacji lub jej dokumentacji?
- [ ] Tak — role są w pełni udokumentowane i są zgodne z zasadą **najniższych uprawnień (least privilege)**  
- [ ] Tak — role są udokumentowane, ale zawierają **nakładające się uprawnienia**  
- [ ] Nie — nie istnieje formalna dokumentacja ani definicje ról  

### Czy istnieje wyraźny rozdział między funkcjami administracyjnymi a funkcjami standardowego użytkownika?
- [ ] Tak — funkcje administracyjne są **całkowicie odizolowane** od widoków standardowego użytkownika  
- [ ] Tak — rozdział istnieje, ale punkty końcowe (endpoints) administracji są **przewidywalne lub możliwe do odgadnięcia**  
- [ ] Nie — funkcje administracyjne i standardowe są **wymieszane** w ramach tych samych interfejsów  

### Czy aplikacja wykorzystuje scentralizowany mechanizm kontroli dostępu opartej na rolach (RBAC)?
- [ ] Tak — scentralizowany mechanizm RBAC lub ABAC jest **zaimplementowany** i konsekwentnie egzekwowany  
- [ ] Tak — istnieją zdecentralizowane mechanizmy kontrolne, ale są **niekonsekwentnie stosowane** w różnych modułach  
- [ ] Nie — logika kontroli dostępu jest **zakodowana na sztywno (hardcoded)** w poszczególnych stronach lub komponentach  

### Czy w systemie występują nieudokumentowane lub ukryte role?
- [ ] Nie — wszystkie wykryte role odpowiadają udokumentowanej funkcjonalności  
- [ ] Tak — ukryte role lub role typu "shadow" (np. `super_admin`, `support`, `test`) **są obecne**  

### Czy użytkownik o niskich uprawnieniach może wyliczyć (enumerować) uprawnienia lub role innych użytkowników?
- [ ] Nie — użytkownicy **nie mogą** przeglądać ani wyliczać ról/uprawnień innych podmiotów  
- [ ] Tak — użytkownicy **mogą** wyliczać role poprzez strony profilowe, metadane lub odpowiedzi API *(Średni)*

---

## WSTG-IDNT-02 — Testowanie procesu rejestracji użytkownika

Testowanie procesu rejestracji użytkownika polega na ocenie przepływu pracy (workflow), za pomocą którego tworzone są nowe tożsamości, w celu upewnienia się, że nie może on zostać nadużyty do nieautoryzowanego dostępu lub automatycznej eksploatacji. Podatności w tym procesie często objawiają się brakiem weryfikacji tożsamości (e-mail/SMS), podatnością na masowe tworzenie kont przez boty lub ujawnianiem informacji poprzez zbyt szczegółowe komunikaty o błędach, które zdradzają istniejące nazwy użytkowników. Atakujący wykorzystują te wady do przeprowadzania enumeracji użytkowników (User Enumeration), prowadzenia kampanii spamowych na dużą skalę lub manipulowania parametrami rejestracji w celu uzyskania podwyższonych uprawnień na etapie tworzenia konta. Test ten jest kluczowy dla ochrony integralności aplikacji i zapobiegania zapełnianiu systemu przez atakujących fałszywymi lub złośliwymi kontami.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom dotkliwości** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Czy wymagana jest weryfikacja tożsamości przed aktywacją nowego konta?
- [ ] Tak — rejestracja wymaga unikalnego, ograniczonego czasowo tokenu wysyłanego przez e-mail lub SMS  
- [ ] Tak — weryfikacja jest wymagana, ale tokeny są **słabe** lub **przewidywalne**  
- [ ] Nie — konta są aktywowane **natychmiastowo** bez żadnego kroku weryfikacji  

### Czy proces rejestracji zapobiega enumeracji użytkowników (User Enumeration)?
- [ ] Tak — aplikacja zwraca **identyczne** odpowiedzi zarówno dla nowych, jak i istniejących adresów e-mail/nazw użytkowników  
- [ ] Nie — komunikaty o błędach (np. „Email już w użyciu”) **pozwalają** atakującemu na mapowanie zarejestrowanych użytkowników  
- [ ] Nie — różnice w czasie odpowiedzi serwera (timing differences) **ujawniają**, czy dany użytkownik istnieje  

### Czy wdrożono mechanizmy chroniące przed automatyzacją, aby zapobiec masowej rejestracji?
- [ ] Tak — mechanizmy CAPTCHA lub proof-of-work są **obecne** i **skuteczne**  
- [ ] Tak — mechanizmy istnieją, ale możliwe jest ich **obejście** (Bypass) poprzez punkty końcowe API lub manipulację nagłówkami  
- [ ] Nie — do punktu końcowego rejestracji nie zastosowano mechanizmów `Rate Limiting` ani CAPTCHA  

### Czy parametry rejestracji mogą być manipulowane w celu eskalacji uprawnień?
- [ ] Nie — role użytkowników są przypisywane **wyłącznie** przez logikę po stronie serwera  
- [ ] Tak — parametry takie jak `role`, `admin` lub `group_id` **mogą** być modyfikowane w żądaniu w celu uzyskania wyższego poziomu dostępu  

### Czy polityka haseł jest egzekwowana podczas fazy rejestracji?
- [ ] Tak — silne wymagania dotyczące haseł są **wymuszane** po stronie serwera  
- [ ] Nie — słabe hasła (np. „password123”) są **akceptowane** przez system

---

## WSTG-IDNT-03 — Testowanie procesu aprowizacji kont (Account Provisioning Process)

Proces aprowizacji kont (Account Provisioning) zarządza sposobem tworzenia, weryfikacji i przypisywania uprawnień nowym tożsamościom w środowisku aplikacji. Atakujący biorą ten proces za cel, aby przeprowadzać zautomatyzowane masowe rejestracje w celu rozsyłania spamu lub ataków Denial-of-Service (DoS), omijać etapy weryfikacji tożsamości w celu tworzenia fałszywych kont lub manipulować parametrami, aby przyznać sobie podwyższone uprawnienia podczas fazy rejestracji. Podatności często objawiają się w formularzach samodzielnej rejestracji (Self-service), systemach dostępnych tylko na zaproszenie lub administracyjnych konsolach aprowizacji, gdzie błędy logiki biznesowej (Business Logic Flaws) pozwalają na nieautoryzowane tworzenie kont lub eskalację uprawnień (Privilege Escalation). Z perspektywy atakującego, skompromitowanie procesu aprowizacji zapewnia legalny punkt zaczepienia, który może zostać wykorzystany jako baza do głębszej eksploatacji, eksfiltracji danych lub ataków socjotechnicznych (Social Engineering).


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Status testu** | Nie wykonano |
| **Poziom ważności** | Średni / Wysoki* |

> *Poziom ważności staje się krytyczny (Critical), jeśli atakujący może utworzyć konta administracyjne lub obejść globalne ograniczenia rejestracji.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### Czy samodzielna rejestracja jest ściśle kontrolowana lub ograniczona do autoryzowanych użytkowników?
- [ ] Tak — rejestracja jest **wyłączona** lub wymaga ręcznego zatwierdzenia przez administratora  
- [ ] Tak — rejestracja jest otwarta, ale ograniczona do **autoryzowanych** domen e-mail lub tokenów zaproszeń  
- [ ] Nie — rejestracja jest **otwarta** dla każdego użytkownika bez ograniczeń dotyczących domen lub tokenów  

### Czy wymagany jest solidny mechanizm weryfikacji tożsamości (np. e-mail/SMS) przed aktywacją konta?
- [ ] Tak — weryfikacja jest **wymagana** i **nie może** zostać pominięta  
- [ ] Tak — weryfikacja jest **wymagana**, ale **można** ją pominąć poprzez manipulację parametrami lub przewidywalne tokeny  
- [ ] Nie — konta są **aktywne natychmiast** po przesłaniu formularza bez żadnej weryfikacji  

### Czy użytkownik może manipulować parametrami ról lub uprawnień podczas żądania rejestracji?
- [ ] Nie — role są **przypisywane po stronie serwera (Server-side)** i nie istnieją parametry po stronie klienta (Client-side)  
- [ ] Tak — parametry ról istnieją, ale manipulacja nimi jest **niemożliwa** ze względu na walidację po stronie serwera  
- [ ] Tak — parametry ról/uprawnień (np. `role=admin`, `is_staff=true`) **mogą** być modyfikowane w celu uzyskania podwyższonych uprawnień *(Krytyczne)*  

### Czy wdrożono limity zapytań (Rate Limiting) lub mechanizmy przeciwko automatyzacji, aby zapobiec masowemu tworzeniu kont?
- [ ] Tak — limity Rate Limiting i mechanizmy CAPTCHA są **aktywne** i skuteczne  
- [ ] Tak — mechanizmy kontrolne istnieją, ale ich **obejście jest możliwe** poprzez spoofing nagłówków lub usługi rozwiązywania CAPTCHA  
- [ ] Nie — **nie zastosowano** żadnych limitów Rate Limiting ani mechanizmów przeciwko automatyzacji  

### Czy proces aprowizacji wycieka informacje o istniejących kontach (Enumeracja użytkowników — User Enumeration)?
- [ ] Nie — komunikaty odpowiedzi są **identyczne** dla istniejących i nieistniejących użytkowników  
- [ ] Tak — różne odpowiedzi lub różnice w czasie odpowiedzi (Timing Variations) **pozwalają** na enumerację istniejących użytkowników

---

## WSTG-IDNT-04 — Testowanie pod kątem wyliczania kont i zgadywalnych kont użytkowników

Wyliczanie kont (Account Enumeration) występuje, gdy aplikacja ujawnia istnienie lub brak konta użytkownika poprzez różnice w odpowiedziach HTTP, komunikatach o błędach lub mierzalnych różnicach czasowych. Atakujący wykorzystują to zachowanie, systematycznie przesyłając listy nazw użytkowników w celu zidentyfikowania prawidłowych kont, które następnie stają się celem ataków typu Credential Stuffing, Brute Force lub socjotechniki. Ta podatność powszechnie występuje w portalach logowania, funkcjach resetowania hasła, formularzach rejestracji oraz punktach końcowych API obsługujących identyfikatory specyficzne dla użytkownika. Z perspektywy atakującego, skuteczne wyliczenie (enumeration) znacząco zawęża powierzchnię ataku, przekształcając próbę ataku typu "blind Brute Force" w ukierunkowaną eksploatację znanych, poprawnych tożsamości.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Status testu** | Niewykonano |
| **Poziom zagrożenia** | Niski / Średni |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Czy komunikaty o błędach uwierzytelniania są spójne we wszystkich scenariuszach?
- [ ] Tak — ogólne komunikaty o błędach są używane zarówno dla nieprawidłowych nazw użytkowników, jak i nieprawidłowych haseł  
- [ ] Tak — komunikaty są podobne, ale **występują** niewielkie różnice w interpunkcji lub wielkości liter  
- [ ] Nie — komunikaty o błędach jawnie informują: „Użytkownik nie został znaleziony” lub „Nieprawidłowe hasło” *(Podatność)*  

### Czy proces resetowania hasła lub funkcja „zapomniałem hasła” ujawnia istnienie konta?
- [ ] Nie — aplikacja zwraca ogólny komunikat niezależnie od tego, czy adres e-mail/nazwa użytkownika istnieje  
- [ ] Tak — zabezpieczenia są wdrożone, ale obejście (bypass) **jest możliwe** poprzez analizę długości odpowiedzi lub kodów statusu  
- [ ] Tak — aplikacja jawnie potwierdza, czy wysłano wiadomość e-mail z resetowaniem hasła, czy też konto **nie istnieje**  

### Czy proces rejestracji jest chroniony przed wyliczaniem użytkowników?
- [ ] Nie — rejestracja nie ujawnia informacji lub wykorzystuje CAPTCHA/Rate Limiting w celu zapobiegania masowym sprawdzeniom  
- [ ] Tak — zabezpieczenia są wdrożone, ale obejście (bypass) **jest możliwe** poprzez analizę czasu odpowiedzi lub różnic w odpowiedziach API  
- [ ] Tak — aplikacja natychmiast powiadamia użytkownika, jeśli nazwa użytkownika lub adres e-mail **są już zajęte**  

### Czy różnice czasowe są mierzalne podczas porównywania poprawnych i niepoprawnych nazw użytkowników?
- [ ] Nie — czasy odpowiedzi są spójne niezależnie od poprawności konta dzięki zastosowaniu porównań o stałym czasie (constant-time)  
- [ ] Tak — różnice czasowe istnieją, ale są zbyt małe, aby można je było wiarygodnie zmierzyć przez sieć  
- [ ] Tak — mierzalne rozbieżności czasowe **są obecne** (np. z powodu wykonywania hashowania haseł tylko dla poprawnych użytkowników)  

### Czy aplikacja stosuje przewidywalne lub łatwe do odgadnięcia konwencje nazewnictwa kont?
- [ ] Nie — identyfikatory kont są losowe (UUID) lub zdefiniowane przez użytkownika i złożone  
- [ ] Tak — identyfikatory kont są zgodne ze wzorcem (np. `emp001`, `emp002`) i wyliczanie (enumeration) **jest możliwe**  
- [ ] Tak — identyfikatory są kolejnymi liczbami całkowitymi, co sprawia, że pełne wyliczenie bazy danych **jest możliwe**

---

## WSTG-IDNT-05 — Testowanie pod kątem słabej lub niewymuszonej polityki nazw użytkowników

Testowanie pod kątem słabej lub niewymuszonej polityki nazw użytkowników koncentruje się na regułach rządzących tworzeniem identyfikatorów kont oraz ich podatności na enumerację (Username Enumeration) lub podszywanie się (Spoofing). Słabe polityki często zezwalają na przewidywalne sekwencje, powszechne słowa słownikowe lub identyfikatory odzwierciedlające wewnętrzne identyfikatory pracowników, co znacząco zmniejsza przestrzeń poszukiwań dla ataków typu Brute Force i Credential Stuffing. Z perspektywy atakującego, identyfikacja tych wzorców jest pierwszym krokiem w masowym wykrywaniu kont, co często osiąga się poprzez analizę komunikatów o błędach rejestracji, zachowania funkcji resetowania hasła lub metadanych w profilach publicznych. Zapewnienie solidnej polityki uniemożliwia atakującym systematyczne mapowanie bazy użytkowników i ułatwia obronę przed ukierunkowanym Phishingiem oraz zautomatyzowanymi próbami obejścia uwierzytelniania.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Niski / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Czy nazwy użytkowników opierają się na wysoce przewidywalnych lub sekwencyjnych wzorcach?
- [ ] Nie — nazwy użytkowników są losowe, zdefiniowane przez użytkownika lub są ciągami o wysokiej entropii  
- [ ] Tak — nazwy użytkowników są zgodne z przewidywalnym formatem (np. `user1001`, `user1002`), ale uwierzytelnianie jest solidne  
- [ ] Tak — nazwy użytkowników są **ściśle sekwencyjne** lub są zgodne ze znanym formatem korporacyjnym, co ułatwia enumerację  

### Czy aplikacja wymusza wymagania dotyczące minimalnej długości i złożoności nazw użytkowników?
- [ ] Tak — podczas rejestracji **wymuszane są** rygorystyczne polityki dotyczące długości i zestawu znaków  
- [ ] Tak — polityki istnieją, ale pozwalają na niezwykle krótkie (np. 1-2 znaki) lub zbyt proste nazwy użytkowników  
- [ ] Nie — **nie jest wymuszana żadna polityka** dotycząca długości lub typów znaków w nazwach użytkowników  

### Czy poprawne nazwy użytkowników mogą zostać wyliczone poprzez odpowiedzi aplikacji?
- [ ] Nie — aplikacja zwraca ogólne komunikaty o błędach i wykazuje spójność czasową (Timing) dla wszystkich prób  
- [ ] Tak — aplikacja zwraca odrębne komunikaty o błędach (np. „Nazwa użytkownika jest już zajęta”), ale stosowane jest **Rate Limiting**  
- [ ] Tak — aplikacja **ujawnia** poprawne nazwy użytkowników poprzez błędy rejestracji, resetowanie hasła lub różnice w czasie odpowiedzi  

### Czy polityka zapobiega rejestracji powszechnych lub zarezerwowanych administracyjnych nazw użytkowników?
- [ ] Tak — aplikacja blokuje powszechne nazwy (np. `admin`, `root`, `support`) oraz terminy zarezerwowane systemowo  
- [ ] Nie — użytkownicy **mogą** rejestrować konta o nazwach wrażliwych lub administracyjnych  

### Czy polityka nazw użytkowników jest spójna we wszystkich punktach wejścia (API, Mobile, Web)?
- [ ] Tak — polityki **są stosowane** spójnie we wszystkich interfejsach i wersjach  
- [ ] Nie — starsze punkty końcowe (Legacy Endpoints) lub określone wersje API **nie wymuszają** tych samych ograniczeń nazw użytkowników

---

## WSTG-ATHN-01 — Testowanie przesyłania poświadczeń przez kanał szyfrowany

Testowanie przesyłania poświadczeń przez kanał szyfrowany ma na celu upewnienie się, że wrażliwe dane uwierzytelniające, takie jak nazwy użytkowników, hasła i tokeny sesyjne, są chronione przed przechwyceniem podczas transmisji. Atakujący znajdujący się w sieci — na przykład poprzez ataki Man-in-the-Middle (MitM) w publicznych sieciach Wi-Fi lub na przejętych sieciach wewnętrznych — mogą używać narzędzi do sniffing pakietów w celu przechwycenia poświadczeń przesyłanych otwartym tekstem, jeśli protokół TLS/SSL nie jest prawidłowo wymuszony. Podatność ta występuje zazwyczaj w punktach końcowych (endpoints) logowania, formularzach resetowania hasła oraz nagłówkach uwierzytelniania API, w których aplikacja nie wymusza HTTPS lub nieprawidłowo przekierowuje z HTTP. Udana eksploatacja zapewnia atakującemu pełny dostęp do kont użytkowników, prowadząc do całkowitego naruszenia danych użytkownika i potencjalnego ruchu bocznego (Lateral Movement) wewnątrz aplikacji.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Narzędzia:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Czy protokół HTTPS jest wymuszany w całym procesie uwierzytelniania?
- [ ] Tak — HSTS jest **włączony**, a cały ruch jest rygorystycznie wymuszany przez HTTPS  
- [ ] Tak — następuje przekierowanie do HTTPS, ale HSTS **nie jest włączony**  
- [ ] Nie — poświadczenia **mogą** być przesyłane przez nieszyfrowany protokół HTTP  

### Czy strony logowania i formularze są serwowane przez kanał szyfrowany?
- [ ] Tak — strona zawierająca formularz logowania jest dostarczana przez HTTPS  
- [ ] Nie — strona logowania jest serwowana przez HTTP, co pozwala na przejęcie akcji formularza (form-action hijacking) lub sniffing poświadczeń  

### Czy flaga `Secure` jest stosowana do wszystkich wrażliwych plików cookie sesji?
- [ ] Tak — flaga `Secure` jest **stosowana** do wszystkich plików cookie uwierzytelniania i sesji  
- [ ] Tak — flaga `Secure` jest **stosowana** tylko do niektórych plików cookie  
- [ ] Nie — flaga `Secure` **nie jest stosowana**, co pozwala na eksfiltrację plików cookie poprzez nieszyfrowane żądania  

### Czy serwer wspiera słabe wersje TLS lub niebezpieczne zestawy szyfrów?
- [ ] Nie — tylko nowoczesne wersje TLS (1.2+) i silne szyfry są **włączone**  
- [ ] Tak — starsze wersje (TLS 1.0/1.1) lub słabe szyfry (RC4, DES, CBC) są **obsługiwane**  

### Czy aplikacja zapobiega przesyłaniu poświadczeń do domen zewnętrznych przez kanały nieszyfrowane?
- [ ] Tak — żadne wrażliwe dane nie są wysyłane do domen zewnętrznych lub wszystkie połączenia zewnętrzne są wymuszane przez HTTPS  
- [ ] Nie — nagłówki uwierzytelniania lub poświadczenia są wysyłane do zewnętrznych punktów końcowych (np. analityka lub SSO) przez HTTP

---

## WSTG-ATHN-02 — Testowanie pod kątem domyślnych danych uwierzytelniających

Testowanie pod kątem domyślnych danych uwierzytelniających (Default Credentials) polega na identyfikacji interfejsów administracyjnych, usług lub komponentów aplikacji, które nadal korzystają z fabrycznie ustawionych lub dostarczonych przez dostawcę nazw użytkowników i haseł. Ta podatność jest celem o wysokim priorytecie dla atakujących, ponieważ często zapewnia bezpośrednią ścieżkę do pełnego przejęcia systemu, dostępu administracyjnego lub zdalnego wykonania kodu (Remote Code Execution) przy minimalnym wysiłku. Zazwyczaj występuje w gotowym oprogramowaniu (off-the-shelf software), platformach CMS, narzędziach do zarządzania bazami danych oraz zintegrowanym sprzęcie, takim jak urządzenia IoT lub urządzenia sieciowe. Eksploatacja (Exploit) jest zazwyczaj przeprowadzana poprzez porównanie zidentyfikowanych wersji oprogramowania z publicznymi bazami domyślnych haseł lub przy użyciu zautomatyzowanych narzędzi do Credential Stuffing podczas początkowych faz rozpoznania i eksploatacji.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Krytyczny / Wysoki |

**Odnośniki:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Narzędzia:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Czy interfejsy administracyjne lub do zarządzania są wystawione do Internetu lub niezaufanych segmentów sieci?
- [ ] Nie — żadne interfejsy zarządzania nie są dostępne  
- [ ] Tak — interfejsy są obecne, ale ograniczone przez mechanizmy kontroli na poziomie sieci (np. VPN, IP Allowlist)  
- [ ] Tak — interfejsy **są** publicznie dostępne i wymagają uwierzytelnienia  

### Czy domyślne dane uwierzytelniające dostarczone przez dostawcę zostały zmienione dla wszystkich zidentyfikowanych komponentów?
- [ ] Tak — wszystkie domyślne dane uwierzytelniające **zostały zmienione** we wszystkich komponentach *(Najbezpieczniejsze)*  
- [ ] Tak — dane uwierzytelniające **zostały zmienione** dla głównej aplikacji, ale komponenty poboczne (np. CMS, DB) pozostają domyślne  
- [ ] Nie — domyślne dane uwierzytelniające **są aktywne** na jednym lub większej liczbie możliwych do zidentyfikowania interfejsów *(Krytyczne)*  

### Czy aplikacja wymusza zmianę hasła przy pierwszym logowaniu dla nowych lub administracyjnych kont?
- [ ] Tak — zmiana hasła **jest obowiązkowa** przed podjęciem jakichkolwiek działań administracyjnych  
- [ ] Nie — aplikacja **pozwala** na korzystanie z domyślnych danych uwierzytelniających bezterminowo  

### Czy mechanizmy blokady konta lub ograniczania liczby żądań (Rate Limiting) są aktywne, aby zapobiec zautomatyzowanemu testowaniu domyślnych danych uwierzytelniających?
- [ ] Tak — stosowane jest rygorystyczne Rate Limiting lub blokada konta (Lockout)  
- [ ] Tak — mechanizmy kontrolne są obecne, ale możliwe jest ich **obejście** poprzez manipulację nagłówkami lub rotację adresów IP  
- [ ] Nie — żadne mechanizmy Rate Limiting ani blokady konta **nie są włączone**  

### Czy wtyczki podmiotów trzecich, oprogramowanie pośredniczące (Middleware) lub narzędzia administracyjne (np. phpMyAdmin, Tomcat Manager) używają unikalnych danych uwierzytelniających?
- [ ] Nie — komponenty podmiotów trzecich nie występują w środowisku  
- [ ] Tak — wszystkie komponenty podmiotów trzecich używają unikalnych, niedomyślnych danych uwierzytelniających  
- [ ] Nie — co najmniej jeden komponent podmiotu trzeciego **jest dostępny** przy użyciu znanych domyślnych danych uwierzytelniających

---

## WSTG-ATHN-03 — Testowanie pod kątem słabych mechanizmów blokady konta

Mechanizm blokady konta jest krytycznym elementem strategii obrony w głąb (defense-in-depth), zaprojektowanym w celu mitygacji ataków typu Brute Force oraz Credential Stuffing poprzez dezaktywację konta po określonej liczbie nieudanych prób uwierzytelnienia. Bez solidnej polityki blokad, napastnicy mogą używać zautomatyzowanych narzędzi do systematycznego odgadywania haseł dla wielu kont (Password Spraying) lub atakować pojedyncze konto o wysokiej wartości aż do skutku. Podatność ta zazwyczaj występuje w portalach logowania, formularzach resetowania hasła oraz endpointach API, które nie posiadają Rate Limiting lub ochrony na poziomie konta. Z perspektywy atakującego, słaby lub nieobecny mechanizm blokady pozwala na nieskończoną liczbę prób łamania hasła, podczas gdy zbyt agresywny mechanizm może zostać wykorzystany do wywołania rozproszonej odmowy usługi (DoS) przeciwko legalnym użytkownikom poprzez celowe blokowanie ich kont.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się Wysoki, jeśli aplikacja przetwarza wrażliwe dane osobowe (PII), dane finansowe lub jeśli konta administracyjne są podatne na ataki Brute Force bez żadnych dodatkowych mechanizmów kontrolnych.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Narzędzia:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Czy mechanizm blokady konta jest wymuszany po określonej liczbie nieudanych prób?
- [ ] Tak — konto jest blokowane po zdefiniowanym, rozsądnym progu (np. 5-10 prób)  
- [ ] Tak — konto jest blokowane, ale dopiero po nadmiernie wysokiej liczbie prób  
- [ ] Nie — konto **nie może** zostać zablokowane, co pozwala na nieograniczony atak Brute Force  

### Czy mechanizm blokady można pominąć za pomocą powszechnych technik unikania zabezpieczeń?
- [ ] Nie — blokada jest wymuszana po stronie serwera (server-side) i zmiany po stronie klienta (client-side) **nie mają** na nią wpływu  
- [ ] Tak — blokadę **można** pominąć poprzez rotację adresów IP lub użycie puli serwerów proxy  
- [ ] Tak — blokadę **można** pominąć poprzez manipulację nagłówkami HTTP, takimi jak `X-Forwarded-For` lub `User-Agent`  
- [ ] Tak — blokadę **można** pominąć poprzez czyszczenie plików cookie lub rozpoczynanie nowych sesji  

### Czy mechanizm blokady zapewnia ścieżkę odzyskiwania konta lub wygaśnięcia blokady?
- [ ] Tak — konto jest automatycznie odblokowywane po określonym czasie lub poprzez weryfikację e-mail  
- [ ] Tak — odblokowanie konta wymaga interwencji administratora  
- [ ] Nie — blokada jest stała i nie posiada jasnej ścieżki odzyskiwania, co zwiększa ryzyko DoS  

### Czy aplikacja rozróżnia prawidłową i nieprawidłową nazwę użytkownika podczas blokady?
- [ ] Nie — odpowiedź aplikacji jest identyczna zarówno dla istniejących, jak i nieistniejących użytkowników  
- [ ] Tak — aplikacja ujawnia, czy konto jest zablokowane, co pozwala na enumerację użytkowników (Username Enumeration)  

### Czy mechanizm blokady jest podatny na atak odmowy usługi (DoS)?
- [ ] Nie — mechanizmy kontrolne, takie jak CAPTCHA lub Rate Limiting oparty na IP, zapobiegają masowemu blokowaniu kont  
- [ ] Tak — atakujący **może** systematycznie blokować wszystkich znanych użytkowników, podając błędne hasła

---

## WSTG-ATHN-04 — Testowanie pod kątem obejścia schematu uwierzytelniania

Obejście schematu uwierzytelniania polega na identyfikacji i wykorzystaniu błędów w logice aplikacji, które pozwalają atakującemu na uzyskanie dostępu do chronionych zasobów bez podania poprawnych danych uwierzytelniających. Podatność ta występuje zazwyczaj, gdy aplikacja polega na kontrolach po stronie klienta (client-side), nie wymusza autoryzacji po stronie serwera przy każdym żądaniu lub pozostawia administracyjne „tylne furtki” (backdoory) i punkty końcowe debugowania (debug endpoints) dostępne w środowisku produkcyjnym. Atakujący wykorzystują te słabości poprzez wymuszone przeglądanie (forced browsing) wrażliwych adresów URL, manipulowanie parametrami HTTP, takimi jak `authenticated=true`, lub podszywanie się pod nagłówki (spoofing headers), aby oszukać aplikację i sprawić, by uznała sesję za już ustanowioną. Skuteczna eksploatacja skutkuje całkowitym przełamaniem obwodu uwierzytelniania, co potencjalnie prowadzi do nieautoryzowanego wycieku danych (data exfiltration) lub pełnego przejęcia systemu.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Status testu** | Nie wykonano |
| **Krytyczność** | Krytyczny |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Narzędzia:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Czy do wrażliwych stron wewnętrznych można uzyskać bezpośredni dostęp bez aktywnej sesji?
- [ ] Nie — bezpośredni dostęp skutkuje przekierowaniem do logowania lub odpowiedzią 403/404  
- [ ] Tak — niektóre wrażliwe strony są dostępne, ale dostarczają ograniczone lub niewrażliwe dane  
- [ ] Tak — wrażliwe strony administracyjne lub specyficzne dla użytkownika **są** w pełni dostępne bez uwierzytelnienia *(Krytyczny)*  

### Czy aplikacja polega na flagach lub parametrach po stronie klienta (client-side) w celu określenia statusu uwierzytelnienia?
- [ ] Nie — status uwierzytelnienia jest zarządzany wyłącznie poprzez stan sesji po stronie serwera  
- [ ] Tak — istnieją flagi po stronie klienta, ale ich modyfikacja **nie** przyznaje dostępu do obszarów chronionych  
- [ ] Tak — modyfikacja parametrów (np. `is_authenticated=true`, `user_role=admin`) **pozwala** na obejście uwierzytelniania  

### Czy uwierzytelnienie można obejść poprzez podszywanie się pod określone nagłówki HTTP lub ich manipulację?
- [ ] Nie — nagłówki takie jak `X-Forwarded-For`, `Referer` lub nagłówki niestandardowe nie mają wpływu na uwierzytelnianie  
- [ ] Tak — manipulowanie nagłówkami (np. `X-Custom-IP-Authorization`, `X-Remote-User`) **jest możliwe** i przyznaje nieautoryzowany dostęp  

### Czy istnieją alternatywne ścieżki lub artefakty programistyczne, które omijają standardowy przepływ logowania?
- [ ] Nie — wszystkie chronione zasoby są zabezpieczone przez scentralizowaną, gotową do użycia w produkcji kontrolę uwierzytelniania  
- [ ] Tak — przestarzałe punkty końcowe (legacy endpoints), trasy debugowania (np. `/debug/login`) lub zapomniane wersje API **są** dostępne bez poświadczeń  

### Czy aplikacja nie wymaga ponownego uwierzytelnienia lub weryfikacji sesji dla działań o wysokich uprawnieniach?
- [ ] Nie — działania o wysokich uprawnieniach lub wrażliwe wymagają świeżego lub ważnego tokenu sesji  
- [ ] Tak — po znalezieniu obejścia w jednym punkcie końcowym, **można** je wykorzystać do wykonywania działań administracyjnych w całej aplikacji

---

## WSTG-ATHN-05 — Testowanie pod kątem podatności funkcji "Zapamiętaj hasło"

Funkcjonalność „Zapamiętaj mnie” (Remember Me) lub trwałe logowanie ma na celu utrzymanie stanu uwierzytelnienia użytkownika po zrestartowaniu przeglądarki poprzez przechowywanie tokena lub poświadczeń po stronie klienta. Jeśli ta funkcja jest źle zaimplementowana — na przykład poprzez przechowywanie haseł otwartym tekstem, słabych skrótów (hashes) lub tokenów o niskiej entropii w plikach cookie — naraża to użytkownika na przejęcie konta poprzez Cross-Site Scripting (XSS), dostęp fizyczny lub przechwycenie ruchu sieciowego. Pentesterzy oceniają entropię tych trwałych tokenów, flagi bezpieczeństwa stosowane w mechanizmie przechowywania oraz cykl życia sesji, aby upewnić się, że wygoda trwałego dostępu nie omija krytycznych mechanizmów kontrolnych, takich jak uwierzytelnianie wieloskładnikowe (MFA) lub unieważnianie sesji (session invalidation). Eksploatacja często polega na pozyskiwaniu tych długożyjących tokenów w celu uzyskania nieautoryzowanego dostępu do kont bez konieczności podawania oryginalnych poświadczeń.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Status testu** | Nie wykonano |
| **Poziom dotkliwości** | Średni / Wysoki* |

> *Poziom dotkliwości staje się Wysoki, jeśli poświadczenia otwartym tekstem lub niesolone skróty (unsalted hashes) są przechowywane w trwałym pliku cookie lub jeśli token omija uwierzytelnianie wieloskładnikowe (MFA).

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Narzędzia:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### Czy aplikacja implementuje funkcję „Zapamiętaj mnie” lub „Pozostań zalogowany”?
- [ ] Nie — funkcja **nie istnieje** w aplikacji  
- [ ] Tak — funkcja istnieje i wykorzystuje kryptograficznie silne, nieprzewidywalne tokeny  
- [ ] Tak — funkcja istnieje, ale używa przewidywalnych tokenów lub tokenów o niskiej entropii *(Średni)*  
- [ ] Tak — funkcja istnieje i przechowuje poświadczenia otwartym tekstem lub hasła zakodowane w base64 *(Wysoki)*  

### Czy flagi bezpieczeństwa są poprawnie nałożone na trwały plik cookie?
- [ ] Tak — flagi `HttpOnly`, `Secure` oraz `SameSite` są **nałożone**  
- [ ] Tak — niektóre flagi są nałożone, ale brakuje flagi `HttpOnly`  
- [ ] Tak — niektóre flagi są nałożone, ale brakuje flagi `Secure` przy połączeniu HTTPS  
- [ ] Nie — żadne flagi bezpieczeństwa **nie są nałożone** na trwały plik cookie  

### Czy token „Zapamiętaj mnie” pozostaje ważny po ręcznym wylogowaniu?
- [ ] Nie — token jest unieważniany po stronie serwera natychmiast po wylogowaniu  
- [ ] Tak — token pozostaje ważny, ale wymaga hasła do wykonania wrażliwych operacji  
- [ ] Tak — token pozostaje ważny i pozwala na pełny dostęp do konta **bez** ponownego uwierzytelnienia *(Średni)*  

### Czy trwała sesja jest przerywana po zmianie hasła?
- [ ] Tak — wszystkie trwałe tokeny są unieważniane, gdy użytkownik zmienia hasło  
- [ ] Nie — token „Zapamiętaj mnie” pozostaje ważny po tym, jak **przeprowadzono** reset/zmianę hasła *(Wysoki)*  

### Czy trwały token omija uwierzytelnianie wieloskładnikowe (MFA) przy kolejnych wizytach?
- [ ] Nie — MFA jest wymagane nawet w obecności tokena „Zapamiętaj mnie”  
- [ ] Tak — MFA jest omijane, ale token jest powiązany ze specyficznym odciskiem palca urządzenia (device fingerprint)  
- [ ] Tak — MFA jest **całkowicie omijane** przy użyciu wyłącznie trwałego pliku cookie z dowolnego urządzenia *(Wysoki)*

---

## WSTG-ATHN-06 — Testowanie pod kątem słabości pamięci podręcznej przeglądarki

Słabość pamięci podręcznej przeglądarki (Browser cache weakness) występuje, gdy wrażliwe informacje są przechowywane w lokalnej pamięci podręcznej przeglądarki i mogą zostać odzyskane przez nieuprawnionego użytkownika mającego dostęp do tej samej maszyny fizycznej. Brak odpowiednich nagłówków kontroli pamięci podręcznej (Cache-Control headers) pozwala na przetrzymywanie potencjalnie wrażliwych danych, takich jak informacje osobowe, szczegóły konta lub identyfikatory sesji, po wylogowaniu użytkownika lub zamknięciu sesji. Atakujący lub kolejni użytkownicy współdzielonych lub publicznych terminali mogą to wykorzystać poprzez nawigowanie w historii przeglądarki, użycie przycisku „Wstecz” lub sprawdzanie lokalnych plików pamięci podręcznej w celu eksfiltracji (exfiltrate) zbuforowanych odpowiedzi. Zapewnienie implementacji rygorystycznych dyrektyw, takich jak `Cache-Control: no-store` i `Pragma: no-cache`, jest kluczowe dla każdej aplikacji obsługującej uwierzytelnione treści, aby zagwarantować, że wrażliwe dane nigdy nie zostaną zapisane na dysku.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Niski / Średni* |

> *Poziom zagrożenia wzrasta do Średniego, jeśli aplikacja jest często używana na publicznych kioskach lub zawiera wysoce wrażliwe dane PII/PHI.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Narzędzia:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Czy na stronach uwierzytelnionych lub wrażliwych obecne są odpowiednie nagłówki cache-control?
- [ ] Tak — nagłówki `Cache-Control: no-store, no-cache` oraz `Pragma: no-cache` są **obecne** i **poprawnie skonfigurowane**  
- [ ] Tak — niektóre nagłówki są obecne, ale brakuje dyrektywy `no-store`, co pozwala na zapisywanie danych na dysku (disk caching)  
- [ ] Nie — nie zastosowano żadnych nagłówków związanych z pamięcią podręczną, polegając na domyślnym zachowaniu przeglądarki  

### Czy aplikacja używa dyrektywy `private` dla danych specyficznych dla użytkownika?
- [ ] Tak — dyrektywa `Cache-Control: private` jest **włączona** dla wszystkich spersonalizowanych treści, aby zapobiec buforowaniu przez serwery proxy (proxy caching)  
- [ ] Nie — treść jest oznaczona jako `public` lub brakuje dyrektywy `private`, co umożliwia jej **buforowanie** przez pośredniczące serwery proxy  

### Czy wrażliwe informacje są dostępne za pomocą przycisku „Wstecz” przeglądarki po poprawnym wylogowaniu?
- [ ] Nie — przeglądarka prosi o ponowne uwierzytelnienie lub strona **nie jest ładowana** z lokalnej pamięci podręcznej  
- [ ] Tak — wrażliwe informacje są **nadal widoczne** za pomocą przycisku wstecz po zakończeniu sesji  

### Czy wrażliwe pliki niebędące dokumentami HTML (np. PDF-y, eksporty CSV, odpowiedzi API JSON) są chronione przed buforowaniem?
- [ ] Nie — aplikacja nie generuje ani nie obsługuje wrażliwych plików niebędących dokumentami HTML  
- [ ] Tak — rygorystyczne nagłówki cache-control są **zastosowane** do wszystkich pobieranych plików wrażliwych oraz punktów końcowych API  
- [ ] Nie — wrażliwe pliki do pobrania lub odpowiedzi API są **przechowywane** w lokalnej pamięci podręcznej przeglądarki lub w katalogach tymczasowych  

### Czy aplikacja używa nagłówka `Expires: 0` lub daty z przeszłości, aby zapobiec użyciu nieaktualnych danych?
- [ ] Tak — nagłówek `Expires` jest ustawiony na `0` lub datę historyczną, **zapewniając**, że przeglądarka traktuje treść jako natychmiastowo wygasłą  
- [ ] Nie — brakuje nagłówka `Expires` lub jest on ustawiony na przyszłą datę dla stron wrażliwych

---

## WSTG-ATHN-07 — Testowanie pod kątem słabej polityki haseł

Testowanie pod kątem słabych polityk haseł polega na ocenie wymagań dotyczących złożoności, długości i entropii wymuszanych przez aplikację podczas tworzenia konta i aktualizacji poświadczeń. Niewystarczające polityki bezpośrednio zagrażają poufności kont użytkowników, czyniąc je podatnymi na zautomatyzowane ataki słownikowe, próby Brute Force oraz Credential Stuffing. Podatności te zazwyczaj występują w punktach końcowych rejestracji, procesach resetowania hasła oraz administracyjnych panelach zarządzania użytkownikami. Z perspektywy atakującego, pobłażliwa polityka znacząco redukuje koszt obliczeniowy i czas wymagany do skutecznego przejęcia poświadczeń przy użyciu popularnych list słów lub łamania opartego na wzorcach.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Średni / Niski* |

> *Poziom krytyczności staje się Wysoki (High), jeśli w aplikacji brakuje mechanizmów blokady konta (Account Lockout) lub ograniczania liczby żądań (Rate Limiting) w celu zapobiegania automatycznemu zgadywaniu haseł.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Narzędzia:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Czy aplikacja wymusza minimalną długość hasła?
- [ ] Tak — minimalna długość to 12+ znaków *(Najbardziej bezpieczne)*  
- [ ] Tak — minimalna długość wynosi od 8 do 11 znaków  
- [ ] Tak — minimalna długość jest **mniejsza niż** 8 znaków  
- [ ] Nie — minimalna długość nie jest wymuszana  

### Czy wymuszane są wymagania dotyczące złożoności (wielkie litery, małe litery, cyfry, symbole)?
- [ ] Tak — wiele klas znaków jest obowiązkowych, a ich obejście **nie jest możliwe**  
- [ ] Tak — klasy znaków są sugerowane, ale **nie są** wymuszane  
- [ ] Nie — nie stosuje się żadnych wymagań dotyczących złożoności  

### Czy aplikacja blokuje popularne/słabe hasła lub hasła zawierające dane specyficzne dla użytkownika?
- [ ] Tak — znane słabe hasła (np. "password123") oraz hasła oparte na nazwie użytkownika **nie mogą** być użyte  
- [ ] Tak — popularne hasła są blokowane, ale dane specyficzne dla użytkownika (np. nazwa użytkownika, e-mail) **mogą** być użyte  
- [ ] Nie — każdy ciąg znaków spełniający wymagania dotyczące długości/złożoności jest akceptowany  

### Czy wymagania dotyczące złożoności lub długości mogą zostać pominięte poprzez modyfikację po stronie klienta?
- [ ] Nie — polityki są rygorystycznie wymuszane po **stronie serwera (server-side)**  
- [ ] Tak — polityki są wymuszane przez JavaScript i **mogą** zostać pominięte poprzez przechwycenie żądania  

### Czy polityka haseł jest jasno komunikowana użytkownikowi bez ujawniania szczegółów implementacji?
- [ ] Tak — polityka jest widoczna podczas wprowadzania danych i wyświetla ogólne komunikaty o błędach  
- [ ] Tak — polityka jest widoczna, ale komunikaty o błędach ujawniają konkretne wyrażenia regularne (regex) lub luki w logice  
- [ ] Nie — polityka **nie jest** widoczna, co wymusza jej odkrywanie metodą prób i błędów

---

## WSTG-ATHN-08 — Testowanie pod kątem słabych odpowiedzi na pytania pomocnicze

Testowanie pod kątem słabych odpowiedzi na pytania pomocnicze (security questions) polega na ocenie mechanizmów uwierzytelniania opartego na wiedzy (KBA — Knowledge-Based Authentication) stosowanych do weryfikacji tożsamości użytkownika, zazwyczaj podczas procesów odzyskiwania hasła lub w ramach przepływów uwierzytelniania wieloskładnikowego. Ma to istotne znaczenie, ponieważ pytania pomocnicze często opierają się na statycznych, niejawnych informacjach, które można łatwo odkryć za pomocą białego wywiadu (OSINT) lub socjotechniki (Social Engineering), co prowadzi do nieautoryzowanego przejęcia konta. Podatności te występują zazwyczaj w modułach „Zapomniałem hasła” lub „Odblokowanie konta”, gdzie użytkownicy udzielają odpowiedzi na wcześniej ustalone pytania. Z perspektywy atakującego jest to doskonały cel dla zautomatyzowanych ataków typu Brute Force lub celowanego rozpoznania, ponieważ wielu użytkowników udziela przewidywalnych lub publicznie weryfikowalnych odpowiedzi na powszechne pytania, takie jak „Jakie jest nazwisko panieńskie matki?” lub „Do jakiej szkoły średniej uczęszczałeś?”.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Średni / Wysoki* |

> *Poziom krytyczności staje się wysoki (High), jeśli pytanie pomocnicze jest jedynym wymogiem do pełnego zresetowania hasła i przejęcia konta bez dalszej weryfikacji.

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Narzędzia:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### Czy aplikacja implementuje pytania pomocnicze do weryfikacji tożsamości lub odzyskiwania hasła?
- [ ] Nie — pytania pomocnicze **nie są używane** do uwierzytelniania ani odzyskiwania konta  
- [ ] Tak — pytania pomocnicze są **używane** jako opcjonalny drugi składnik  
- [ ] Tak — pytania pomocnicze są **używane** jako główny/jedyny mechanizm odzyskiwania *(Wysokie ryzyko)*  

### Czy pytania pomocnicze są wybierane z ustalonej listy, czy są definiowane przez użytkownika?
- [ ] Nie — pytania są całkowicie definiowane przez użytkownika i wymagają wysokiej entropii  
- [ ] Tak — pytania są ustalone, ale obejmują opcje trudne do odkrycia za pomocą OSINT  
- [ ] Tak — pytania pochodzą z ustalonej listy powszechnych tematów możliwych do odkrycia przez OSINT *(Podatność)*  

### Czy do prób udzielenia odpowiedzi na pytania pomocnicze stosowane jest ograniczenie liczby żądań (Rate Limiting) lub blokada konta?
- [ ] Tak — stosowane jest rygorystyczne Rate Limiting i blokada konta, aby zapobiec atakom Brute Force  
- [ ] Tak — stosowane jest Rate Limiting, ale możliwe jest jego **obejście** poprzez rotację IP lub manipulację nagłówkami  
- [ ] Nie — **nie wymuszono żadnego Rate Limiting** dla prób udzielenia odpowiedzi  

### Czy aplikacja wymusza złożoność lub walidację treści odpowiedzi?
- [ ] Tak — walidacja zapobiega używaniu popularnych słów i zapewnia złożoność/unikalność odpowiedzi  
- [ ] Tak — podstawowa walidacja istnieje, ale **można** ją obejść za pomocą popularnych list haseł lub ataków słownikowych  
- [ ] Nie — **nie wykonuje się żadnej walidacji**; dozwolone są proste lub jednoznakowe odpowiedzi  

### Czy wyzwanie w postaci pytania pomocniczego może zostać pominięte poprzez błędy logiczne?
- [ ] Nie — proces odzyskiwania ściśle wymaga poprawnej odpowiedzi, aby kontynuować  
- [ ] Tak — proces odzyskiwania **może** zostać pominięty poprzez manipulację kodami odpowiedzi HTTP lub parametrami sesji  
- [ ] Tak — odpowiedź jest odzwierciedlona w kodzie źródłowym lub ukrytych polach strony

---

## WSTG-ATHN-09 — Testowanie słabych mechanizmów zmiany lub resetowania hasła

Mechanizmy zmiany i resetowania hasła są krytycznymi komponentami zarządzania uwierzytelnianiem, które w przypadku niewłaściwej implementacji pozwalają atakującym na przejęcie kont użytkowników. Luki te zazwyczaj obejmują przewidywalne tokeny resetowania, brak blokad kont podczas prób Brute Force, niebezpieczne dostarczanie tokenów lub możliwość zmiany hasła bez weryfikacji bieżącego hasła. Atakujący wykorzystują te błędy, przechwytując lub odgadując linki odzyskiwania, wykonując Host Header Injection w celu przekierowania wiadomości e-mail z resetowaniem lub wykorzystując Session Fixation w celu utrzymania dostępu po zmianie hasła. Zapewnienie, że procesy te wymagają silnej weryfikacji i wykorzystują kryptograficzną losowość, jest niezbędne dla zachowania integralności konta i zapobiegania nieautoryzowanemu dostępowi.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Status testu** | Nie przeprowadzono |
| **Krytyczność** | Wysoka / Krytyczna |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Czy bieżące hasło jest wymagane do ustawienia nowego hasła podczas standardowej zmiany?
- [ ] Tak — bieżące hasło **jest wymagane** i weryfikowane  
- [ ] Tak — bieżące hasło **jest wymagane**, ale można je pominąć poprzez Parameter Pollution lub manipulację parametrami  
- [ ] Nie — bieżące hasło **nie jest** wymagane, co umożliwia przejęcie konta poprzez Session Hijacking *(Wysoki)*  

### Czy tokeny resetowania hasła są bezpieczne kryptograficznie i nieprzewidywalne?
- [ ] Tak — tokeny charakteryzują się wysoką entropią, są losowe i **nieprzewidywalne**  
- [ ] Tak — tokeny są generowane, ale wykazują niską entropię lub przewidywalne wzorce (np. oparte na znaczniku czasu)  
- [ ] Nie — tokeny są **przewidywalne** lub oparte na statycznych danych użytkownika, takich jak adres e-mail/ID zakodowane w Base64 *(Krytyczny)*  

### Czy link do resetowania hasła może zostać przekierowany poprzez Host Header Injection?
- [ ] Nie — aplikacja ignoruje lub waliduje nagłówek `Host` podczas generowania linku  
- [ ] Tak — aplikacja używa nagłówka `Host` lub `X-Forwarded-Host` do konstruowania linków, ale walidacja **jest obecna**  
- [ ] Tak — linki **mogą** zostać przekierowane do domeny kontrolowanej przez atakującego poprzez manipulację nagłówkami *(Wysoki)*  

### Czy token resetowania ma ograniczony czas życia i jest natychmiast unieważniany?
- [ ] Tak — tokeny wygasają szybko i są unieważniane **natychmiast** po użyciu lub zmianie hasła  
- [ ] Tak — tokeny wygasają po długim czasie (np. 24+ godziny) lub pozwalają na **wielokrotne użycie**  
- [ ] Nie — tokeny **nigdy nie wygasają** lub pozostają ważne po pomyślnej zmianie hasła  

### Czy punkty końcowe resetowania i zmiany hasła posiadają mechanizmy Rate Limiting lub blokady?
- [ ] Tak — rygorystyczny Rate Limiting oraz mechanizmy CAPTCHA **są stosowane**, aby zapobiec enumeracji i atakom Brute Force  
- [ ] Tak — istnieją ograniczone mechanizmy kontrolne, ale ich **ominięcie jest możliwe** poprzez rotację adresów IP lub Header Spoofing  
- [ ] Nie — Rate Limiting **nie jest stosowany**, co pozwala na masową enumerację kont lub Brute Force tokenów

---

## WSTG-ATHN-10 — Testowanie słabszego uwierzytelniania w kanałach alternatywnych

Testowanie pod kątem słabszego uwierzytelniania w kanałach alternatywnych polega na identyfikowaniu i analizowaniu drugorzędnych ścieżek dostępu do konta — takich jak API mobilne, procesy odzyskiwania haseł, interfejsy pomocy technicznej (help desk) lub starsze subdomeny — które mogą nie wymuszać rygorów bezpieczeństwa na tym samym poziomie, co główny portal webowy. Te alternatywne kanały często nie posiadają solidnego uwierzytelniania wieloskładnikowego (MFA), restrykcyjnych mechanizmów Rate Limiting lub złożonych wymagań dotyczących haseł, tworząc „najsłabsze ogniwo”, które podważa cały system uwierzytelniania. Atakujący celowo wybierają te pominięte punkty wejścia, aby przeprowadzać ataki typu Credential Stuffing lub omijać MFA, wykorzystując rozbieżności w sposobie weryfikacji tożsamości przez różne interfejsy. Zapewnienie parytetu zabezpieczeń na wszystkich powierzchniach uwierzytelniania jest niezbędne, aby zapobiec nieautoryzowanemu dostępowi oraz zachować integralność sesji i danych użytkownika.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Czy alternatywne kanały uwierzytelniania są obecne i zidentyfikowane?
- [ ] Nie — istnieje tylko pojedynczy kanał uwierzytelniania  
- [ ] Tak — zidentyfikowano wiele kanałów (np. API aplikacji mobilnej, klient desktopowy, portal legacy, usługi SOAP)  

### Czy alternatywne kanały wymuszają parytet bezpieczeństwa z kanałem głównym?
- [ ] Tak — wszystkie kanały wymuszają **identyczną** złożoność haseł i wymagania MFA  
- [ ] Tak — kanały wymuszają złożoność haseł, ale MFA **nie jest wymagane** lub **może** zostać pominięte  
- [ ] Nie — kanały alternatywne mają **znacznie słabsze** wymagania dotyczące uwierzytelniania  

### Czy mechanizmy Rate Limiting i blokady konta są spójne we wszystkich kanałach?
- [ ] Tak — Rate Limiting i blokady konta są **konsekwentnie wymuszane** we wszystkich punktach końcowych (endpoints)  
- [ ] Tak — mechanizmy kontrolne są obecne, ale ich ominięcie **jest możliwe** w określonych kanałach (np. API mobilne)  
- [ ] Nie — Rate Limiting lub blokada konta **nie są stosowane** w drugorzędnych ścieżkach uwierzytelniania  

### Czy można ominąć MFA poprzez przełączenie się na kanał alternatywny?
- [ ] Nie — MFA jest obowiązkowe i **nie może** zostać pominięte bez względu na punkt wejścia  
- [ ] Tak — MFA jest wymagane na portalu webowym, ale **nie jest wymuszane** w API mobilnym lub legacy  

### Czy procesy odzyskiwania haseł lub procedury „zapomniałem hasła” są słabsze niż standardowe logowanie?
- [ ] Nie — procesy odzyskiwania wykorzystują silną weryfikację pozapasmową (out-of-band) i nie ujawniają informacji  
- [ ] Tak — procesy odzyskiwania wykorzystują słabe pytania bezpieczeństwa, które **można** łatwo odgadnąć lub sprawdzić (OSINT)  
- [ ] Tak — procesy odzyskiwania są podatne na enumerację lub **nie wymagają** tego samego poziomu potwierdzenia tożsamości, co standardowe logowanie

---

## WSTG-ATHN-11 — Testowanie uwierzytelniania wieloskładnikowego (MFA)

Testowanie uwierzytelniania wieloskładnikowego (Multi-Factor Authentication - MFA) ocenia odporność dodatkowej warstwy bezpieczeństwa, zaprojektowanej w celu zapobiegania nieautoryzowanemu dostępowi, nawet w przypadku przejęcia głównych danych uwierzytelniających. Atakujący często próbują obejść MFA (bypass), identyfikując punkty końcowe (endpoints), w których nie jest ono konsekwentnie wymuszane, takie jak starsze wersje API, backendy aplikacji mobilnych lub przepływy pracy związane z resetowaniem hasła. Eksploatacja często obejmuje manipulowanie odpowiedziami serwera, ataki brute-force na kody o krótkim czasie trwania (OTP), czy wykorzystywanie błędów typu race condition oraz luk w zarządzaniu sesją, które pozwalają użytkownikowi przejść do stanu uwierzytelnionego bez podania drugiego składnika.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### Czy MFA jest wymuszane konsekwentnie we wszystkich portalach uwierzytelniania?
- [ ] Tak — MFA jest wymagane dla wszystkich prób logowania przez WWW, urządzenia mobilne i interfejsy API.
- [ ] Tak — ale MFA **nie jest wymuszane** na konkretnych punktach końcowych (np. przestarzałe `/api/v1/login` lub portale dedykowane dla urządzeń mobilnych).
- [ ] Nie — MFA **nie zostało zaimplementowane** w aplikacji *(Krytyczne)*.

### Czy krok weryfikacji MFA można obejść poprzez bezpośrednią nawigację do adresu URL lub manipulację odpowiedzią?
- [ ] Nie — bezpośrednia nawigacja lub manipulacja parametrami **nie pozwalają** na obejście wyzwania (challenge).
- [ ] Tak — nawigacja bezpośrednio do wewnętrznych adresów URL panelu sterowania **jest możliwa** bez ukończenia wyzwania MFA.
- [ ] Tak — manipulacja odpowiedzią serwera (np. zmiana kodu `401 Unauthorized` na `200 OK`) **jest możliwa** w celu uzyskania dostępu.

### Czy proces weryfikacji kodu MFA jest chroniony przed atakami brute-force?
- [ ] Tak — po wielu nieudanych próbach OTP stosowane jest rygorystyczne ograniczanie liczby żądań (Rate Limiting) lub blokada konta.
- [ ] Tak — ograniczanie liczby żądań istnieje, ale **można je obejść** poprzez rotację adresów IP lub manipulację nagłówkami.
- [ ] Nie — nie wdrożono ograniczania liczby żądań, a zautomatyzowane łamanie kodów metodą brute-force **jest możliwe**.

### Czy aplikacja utrzymuje bezpieczny stan sesji podczas przejścia MFA?
- [ ] Tak — tokeny sesji o wysokich uprawnieniach są wydawane **dopiero po** pomyślnym ukończeniu MFA.
- [ ] Nie — w pełni funkcjonalny plik cookie sesji jest wydawany **przed** ukończeniem MFA, co pozwala na dostęp do niektórych funkcji.
- [ ] Nie — aplikacja używa statycznego identyfikatora lub przewidywalnego przejścia sesji, które **może zostać przejęte** (Session Hijacking).

### Czy alternatywne czynniki MFA (SMS, e-mail, kody zapasowe) są podatne na eksploitację?
- [ ] Nie — wszystkie czynniki wykorzystują bezpieczne, nieprzewidywalne i ograniczone czasowo wartości.
- [ ] Tak — kody zapasowe są przewidywalne lub **mogą zostać wyliczone** (enumeration).
- [ ] Tak — funkcjonalność „Wyślij kod ponownie” (Resend Code) może zostać nadużyta do przeprowadzenia ataków SMS/Email Flooding lub ujawnienia częściowych danych kontaktowych.

---

## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Podatności typu Directory Traversal oraz File Inclusion powstają, gdy aplikacja wykorzystuje dane wejściowe kontrolowane przez użytkownika do konstruowania ścieżek do plików lub katalogów bez wystarczającej walidacji lub sanityzacji. Atakujący wykorzystują te błędy, wstrzykując sekwencje takie jak `../` w celu wyjścia poza zamierzony katalog, co potencjalnie pozwala na dostęp do wrażliwych plików systemowych, danych konfiguracyjnych lub kodu źródłowego aplikacji. W poważniejszych przypadkach, obejmujących Local File Inclusion (LFI) lub Remote File Inclusion (RFI), atakujący może doprowadzić do zdalnego wykonania kodu poprzez dołączenie złośliwych skryptów lub wykorzystanie technik takich jak log poisoning i PHP wrappers. Podatności te zazwyczaj występują w parametrach używanych do dynamicznego ładowania treści, silnikach szablonów lub punktach końcowych służących do pobierania obrazów, gdzie logika po stronie serwera operuje na ścieżkach systemu plików.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Narzędzia:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Czy parametry przyjmują nazwy plików lub ścieżki do przetwarzania po stronie serwera?
- [ ] Nie — żadne parametry nie wydają się wchodzić w interakcję z systemem plików  
- [ ] Tak — parametry istnieją, ale używają ścisłej allowlista identyfikatorów plików  
- [ ] Tak — parametry przyjmują bezpośrednie nazwy plików lub ścieżki  

### Czy dane wejściowe są poddawane sanityzacji w celu zapobiegania sekwencjom directory traversal?
- [ ] Tak — dane wejściowe są walidowane względem ścisłej allowlista i sekwencje traversal **nie są możliwe**  
- [ ] Tak — dane wejściowe są sanityzowane poprzez usuwanie sekwencji `../`, ale rekurencyjny bypass **jest możliwy**  
- [ ] Nie — żadna sanityzacja ani walidacja **nie jest stosowana** do danych wejściowych powiązanych ze ścieżkami  

### Czy możliwe jest uzyskanie dostępu do plików poza ograniczonym katalogiem za pomocą sekwencji traversal?
- [ ] Nie — aplikacja lub system operacyjny zapobiega dostępowi do plików poza zdefiniowanym zakresem  
- [ ] Tak — dostęp do plików wewnątrz katalogu głównego WWW (web root) **jest możliwy**  
- [ ] Tak — dostęp do wrażliwych plików systemowych (np. `/etc/passwd`, `C:\Windows\win.ini`) **jest możliwy** *(Krytyczny)*  

### Czy serwer pozwala na dołączanie zdalnych adresów URL (RFI)?
- [ ] Nie — zdalne dołączanie plików (RFI) jest **wyłączone** na poziomie serwera/aplikacji  
- [ ] Tak — zdalne pliki **mogą** być dołączane, ale ich wykonanie **nie jest możliwe**  
- [ ] Tak — zdalne pliki **mogą** być dołączane i wykonywane na serwerze *(Krytyczny)*  

### Czy filtry mogą zostać pominięte przy użyciu kodowania lub znaków specjalnych?
- [ ] Nie — filtry skutecznie obsługują różne rodzaje kodowania i bajty zerowe (null bytes)  
- [ ] Tak — bypass **jest możliwy** przy użyciu URL encoding, double URL encoding lub 16-bit Unicode  
- [ ] Tak — bypass **jest możliwy** przy użyciu wstrzyknięcia bajtu zerowego (`%00`) lub wrapperów systemu plików (np. `php://filter`)

---

## WSTG-ATHZ-02 — Testowanie pod kątem omijania schematów autoryzacji

Omijanie schematów autoryzacji występuje, gdy aplikacja nie wymusza mechanizmów kontroli dostępu, co umożliwia atakującemu uzyskanie dostępu do zasobów lub funkcji poza jego zamierzonymi uprawnieniami. Luka ta zazwyczaj objawia się jako pozioma eskalacja uprawnień (Horizontal Privilege Escalation), w której atakujący uzyskuje dostęp do danych należących do innego użytkownika o tym samym poziomie uprawnień, lub pionowa eskalacja uprawnień (Vertical Privilege Escalation), gdzie użytkownik o niskich uprawnieniach zyskuje możliwości administracyjne. Atakujący wykorzystują te błędy poprzez manipulowanie parametrami żądań, takimi jak identyfikatory zasobów (Resource IDs), modyfikowanie tokenów sesyjnych lub wykorzystywanie nagłówków HTTP, takich jak `X-Original-URL`, w celu obejścia ograniczeń opartych na ścieżkach. Zapewnienie solidnej autoryzacji jest kluczowe dla zachowania poufności danych i zapobiegania nieautoryzowanym działaniom administracyjnym, które mogłyby naruszyć bezpieczeństwo całego systemu.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Wysoki / Krytyczny |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Narzędzia:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Czy zablokowano poziomą eskalację uprawnień między użytkownikami o tej samej roli?
- [ ] Tak — kontrole autoryzacji **są stosowane** przy każdym żądaniu zasobu, a ich obejście **nie jest możliwe**  
- [ ] Tak — kontrole autoryzacji **są stosowane**, ale można je obejść poprzez IDOR lub manipulację parametrami  
- [ ] Nie — użytkownicy **mogą** uzyskać dostęp do danych należących do innych użytkowników poprzez zwykłą zmianę identyfikatora zasobu *(Wysoki)*  

### Czy zablokowano pionową eskalację uprawnień dla użytkowników o niskich uprawnieniach?
- [ ] Nie — aplikacja posiada tylko jeden poziom uprawnień  
- [ ] Tak — funkcje administracyjne **nie są** dostępne dla użytkowników o niskich uprawnieniach  
- [ ] Tak — funkcje administracyjne są ukryte w interfejsie użytkownika, ale **można** uzyskać do nich dostęp poprzez bezpośredni adres URL  
- [ ] Nie — użytkownicy o niskich uprawnieniach **mogą** wykonywać czynności administracyjne poprzez manipulowanie rolami lub uprawnieniami *(Krytyczny)*  

### Czy autoryzację można obejść za pomocą manipulacji metodami HTTP (HTTP verb tampering)?
- [ ] Nie — mechanizmy kontroli dostępu są wymuszane niezależnie od użytej metody HTTP  
- [ ] Tak — zmiana metody (np. z `GET` na `POST` lub `PUT`) **omija** kontrole autoryzacji  

### Czy punkty końcowe administracyjne lub o ograniczonym dostępie są dostępne bez uwierzytelnienia?
- [ ] Nie — wszystkie wrażliwe punkty końcowe wymagają poprawnej sesji i odpowiednich uprawnień  
- [ ] Tak — niektóre administracyjne punkty końcowe są dostępne, jeśli atakujący zna bezpośredni adres URL  
- [ ] Tak — nieautoryzowany dostęp do wrażliwych funkcji **jest możliwy** *(Krytyczny)*  

### Czy nagłówki HTTP pozwalają na obejście autoryzacji opartej na ścieżkach?
- [ ] Nie — aplikacja **nie jest** podatna na obejścia oparte na nagłówkach  
- [ ] Tak — nagłówki takie jak `X-Original-URL`, `X-Rewrite-URL` lub `X-Forwarded-For` **mogą** zostać użyte do obejścia mechanizmów kontroli dostępu

---

## WSTG-ATHZ-03 — Testowanie pod kątem eskalacji uprawnień

Eskalacja uprawnień (Privilege Escalation) występuje, gdy atakujący wykorzystuje podatności w celu uzyskania dostępu do zasobów lub funkcjonalności zarezerwowanych dla użytkowników o wyższym poziomie autoryzacji lub innej tożsamości. W przypadku eskalacji pionowej (vertical escalation), standardowy użytkownik próbuje uzyskać dostęp do funkcji administracyjnych, podczas gdy eskalacja pozioma (horizontal escalation) polega na dostępie do danych należących do innego użytkownika o tym samym poziomie uprawnień. Błędy te zazwyczaj objawiają się w słabo zaimplementowanych listach kontroli dostępu (ACL), podatnościach typu Insecure Direct Object References (IDOR) lub poprzez manipulację parametrami (parameter tampering) w tokenach sesji lub tożsamości. Z perspektywy atakującego cel ten jest często osiągany poprzez manipulowanie numerycznymi identyfikatorami (ID), modyfikowanie parametrów opartych na rolach w plikach cookies lub tokenach JWT, bądź bezpośrednie wywoływanie ukrytych punktów końcowych API, w których brakuje weryfikacji autoryzacji po stronie serwera.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Status testu** | Nie przeprowadzono |
| **Krytyczność** | Wysoka / Krytyczna |

**Odnośniki:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Narzędzia:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Czy aplikacja implementuje wiele poziomów uprawnień lub architekturę wielodostępną (multi-tenancy)?
- [ ] Nie — aplikacja jest systemem jedno-użytkownikowym lub z jedną rolą  
- [ ] Tak — zdefiniowano wiele ról lub dzierżawców (tenants), co wymaga przeprowadzenia testów autoryzacji  

### Czy możliwa jest pozioma eskalacja uprawnień (dostęp na tym samym poziomie)?
- [ ] Nie — weryfikacja autoryzacji **jest stosowana** do wszystkich identyfikatorów zasobów i właścicieli sesji  
- [ ] Tak — dostęp do danych innych użytkowników **jest możliwy** poprzez IDOR lub manipulację parametrami  
- [ ] Tak — wyciek danych **jest możliwy**, ale modyfikacja danych innych użytkowników **nie jest możliwa**  

### Czy możliwa jest pionowa eskalacja uprawnień (z niskiego na wysoki poziom)?
- [ ] Nie — punkty końcowe administracyjne rygorystycznie wymuszają kontrolę dostępu opartą na rolach (RBAC)  
- [ ] Tak — funkcje administracyjne **mogą** być dostępne dla użytkowników o niskich uprawnieniach poprzez bezpośredni dostęp do adresu URL  
- [ ] Tak — funkcje administracyjne **mogą** być dostępne poprzez manipulację nagłówkami związanymi z rolami, plikami cookie lub oświadczeniami (claims) w tokenie JWT  

### Czy role lub uprawnienia użytkowników mogą być modyfikowane poprzez Mass Assignment lub Parameter Pollution?
- [ ] Nie — pola oparte na rolach są wyłącznie do odczytu i **nie mogą** być modyfikowane przez użytkowników  
- [ ] Tak — uprawnienia użytkownika **mogą** zostać podniesione poprzez dołączenie ukrytych parametrów (np. `role=admin`) w procesie aktualizacji profilu lub rejestracji  

### Czy weryfikacja autoryzacji jest konsekwentnie wymuszana we wszystkich wersjach API i metodach HTTP?
- [ ] Tak — mechanizmy kontrolne **są stosowane** konsekwentnie we wszystkich wersjach i metodach  
- [ ] Nie — starsze wersje API (np. `/v1/`) lub określone metody HTTP (np. `PUT`, `DELETE`, `PATCH`) **omijają** autoryzację  
- [ ] Nie — funkcje administracyjne są ukryte w interfejsie użytkownika (UI), ale **włączone** na poziomie API dla wszystkich użytkowników

---

## WSTG-ATHZ-04 — Testowanie pod kątem podatności Insecure Direct Object References (IDOR)

Podatność Insecure Direct Object References (IDOR) występuje, gdy aplikacja zapewnia bezpośredni dostęp do obiektów na podstawie danych wejściowych dostarczonych przez użytkownika, bez przeprowadzenia weryfikacji autoryzacji w celu potwierdzenia uprawnień żądającego. Podatność ta ma znaczący wpływ na poufność i integralność, ponieważ pozwala atakującym na przeglądanie, modyfikowanie lub usuwanie danych należących do innych użytkowników lub systemu. Zazwyczaj przejawia się w parametrach URL, danych w treści żądań POST lub kluczach JSON, które odnoszą się do wewnętrznych kluczy bazy danych, nazw plików lub identyfikatorów kont. Z perspektywy atakującego, eksploatacja polega na manipulowaniu tymi identyfikatorami — często poprzez inkrementację liczb całkowitych lub Fuzzing identyfikatorów UUID — w celu uzyskania dostępu do zasobów znajdujących się poza ich autoryzowanym zakresem.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Wysoki |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Narzędzia:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### Czy aplikacja używa bezpośrednich identyfikatorów obiektów w parametrach żądania?
- [ ] Nie — aplikacja używa odniesień pośrednich lub zaszyfrowanych map *(Najbardziej bezpieczne)*  
- [ ] Tak — aplikacja używa nieprzewidywalnych identyfikatorów (np. długich UUID/hashy)  
- [ ] Tak — aplikacja używa przewidywalnych identyfikatorów (np. inkrementalnych liczb całkowitych lub nazw użytkowników)  

### Czy autoryzacja po stronie serwera jest przeprowadzana dla każdego żądania zawierającego identyfikator obiektu?
- [ ] Tak — weryfikacja po stronie serwera **nie może** zostać pominięta dla żadnego identyfikatora  
- [ ] Tak — weryfikacja po stronie serwera jest obecna, ale jej obejście **jest możliwe** poprzez Parameter Pollution lub manipulację nagłówkami  
- [ ] Nie — **nie stosuje się** żadnej weryfikacji autoryzacji w celu sprawdzenia własności żądanego obiektu  

### Czy atakujący może uzyskać dostęp lub modyfikować obiekty należące do innych użytkowników (Horizontal IDOR)?
- [ ] Nie — dostęp do danych innych użytkowników **nie jest możliwy**  
- [ ] Tak — przeglądanie danych innych użytkowników **jest możliwe**, ale modyfikacja **nie jest możliwa**  
- [ ] Tak — zarówno przeglądanie, jak i modyfikacja danych innych użytkowników **są możliwe** *(Krytyczne)*  

### Czy możliwe jest uzyskanie dostępu do obiektów na poziomie administracyjnym lub systemowym (Vertical IDOR)?
- [ ] Nie — dostęp do obiektów o wyższych uprawnieniach **nie jest możliwy**  
- [ ] Tak — dostęp do konfiguracji administracyjnych lub plików systemowych **jest możliwy**  

### Czy aplikacja ujawnia identyfikatory obiektów poprzez wyszukiwanie, metadane lub inne punkty końcowe (endpoints)?
- [ ] Nie — identyfikatory **nie są ujawniane** w sposób niezamierzony  
- [ ] Tak — identyfikatory innych użytkowników/obiektów **mogą** zostać wyliczone poprzez pomocnicze punkty końcowe lub profile publiczne

---

## WSTG-ATHZ-05 — Testowanie podatności OAuth

Podatności OAuth obejmują szereg błędów w implementacji protokołów OAuth 2.0 lub OpenID Connect (OIDC), które często prowadzą do całkowitego przejęcia konta (account takeover) lub nieautoryzowanego dostępu do danych. Luki te zazwyczaj objawiają się podczas przepływu autoryzacji (authorization flow), a konkretnie w zakresie walidacji parametru `redirect_uri`, entropii i weryfikacji parametru `state` oraz bezpiecznej obsługi tokenów dostępu (access tokens) lub tokenów ID (ID tokens). Atakujący wykorzystują te słabości, przechwytując kody autoryzacyjne, przekierowując tokeny do domen kontrolowanych przez siebie za pomocą otwartych przekierowań (open redirects) lub przeprowadzając ataki Cross-Site Request Forgery (CSRF) w celu powiązania sesji ofiary z kontem atakującego. Ponieważ OAuth często służy jako podstawowy mechanizm uwierzytelniania, pojedynczy błąd konfiguracji może zagrozić integralności całego systemu tożsamości użytkowników.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Narzędzia:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Czy parametr `state` jest zaimplementowany i weryfikowany w celu zapobiegania CSRF?
- [ ] Tak — parametr `state` jest obowiązkowy, unikalny dla każdej sesji i rygorystycznie weryfikowany po stronie serwera  
- [ ] Tak — parametr `state` jest obecny, ale jest statyczny lub przewidywalny w różnych sesjach  
- [ ] Tak — parametr `state` jest obecny, ale aplikacja **nie weryfikuje** go podczas wywołania zwrotnego (callback)  
- [ ] Nie — parametr `state` **nie jest** używany w żądaniu autoryzacji *(Krytyczny)*  

### Czy `redirect_uri` jest rygorystycznie weryfikowany względem białej listy (whitelist)?
- [ ] Tak — akceptowane są wyłącznie dokładne dopasowania do wcześniej zarejestrowanej białej listy  
- [ ] Tak — walidacja jest stosowana, ale możliwe jest jej obejście (bypass) poprzez Path Traversal lub manipulację subdomenami  
- [ ] Tak — walidacja jest stosowana, ale możliwe jest jej obejście poprzez Parameter Pollution lub Fragment Injection  
- [ ] Nie — aplikacja **zezwala** na dowolne adresy URL w parametrze `redirect_uri` *(Krytyczny)*  

### Czy można zmanipulować `response_type`, aby zmienić przepływ (flow)?
- [ ] Nie — aplikacja akceptuje tylko zamierzony przepływ (np. `code`) i odrzuca inne  
- [ ] Tak — zmiana z `code` na `token` (Implicit flow) **jest możliwa**, co powoduje wyciek tokenów w adresie URL  
- [ ] Tak — przepływy hybrydowe (hybrid flows) lub nieautoryzowane wartości `response_type` są **włączone** i przetwarzane  

### Czy tokeny dostępu lub kody autoryzacyjne wyciekają poprzez nagłówki Referer lub historię przeglądarki?
- [ ] Nie — tokeny/kody są obsługiwane w bezpieczny sposób, a wrażliwe strony używają odpowiedniej polityki `Referrer-Policy`  
- [ ] Tak — tokeny/kody **wyciekają** do domen zewnętrznych poprzez nagłówek `Referer`  
- [ ] Tak — tokeny/kody **są widoczne** w historii przeglądarki z powodu niebezpiecznego buforowania (caching) lub żądań GET  

### Czy aplikacja wymusza zasadę najniższych uprawnień (Least Privilege) dla zakresów (scopes) OAuth?
- [ ] Tak — aplikacja żąda tylko minimalnych zakresów niezbędnych do działania danej funkcjonalności  
- [ ] Nie — aplikacja domyślnie żąda nadmiarowych lub administracyjnych zakresów uprawnień  
- [ ] Nie — użytkownik **nie może** przejrzeć ani zrezygnować z określonych zakresów (scopes) podczas fazy wyrażania zgody (consent phase)

---

## WSTG-SESS-01 — Testowanie schematu zarządzania sesją

Testowanie schematu zarządzania sesją koncentruje się na analizie sposobu, w jaki aplikacja obsługuje cykl życia i strukturę identyfikatorów sesji, aby upewnić się, że są one odporne na przewidywanie i przejęcie (hijacking). Atakujący przechwytują wiele tokenów sesji w celu przeprowadzenia analizy statystycznej, szukając wzorców, niskiej entropii (entropy) lub przewidywalnych przyrostów, które pozwalają na podrobienie (forge) ważnych sesji innych użytkowników. Słabości w schemacie często objawiają się jako podatności typu Session Fixation lub użycie niewystarczająco losowych wartości, zazwyczaj spotykanych w ciasteczkach HTTP (cookies), parametrach URL lub niestandardowych implementacjach nagłówków. Naruszenie schematu sesji jest atakiem o wysokim wpływie (high-impact), który przyznaje przeciwnikowi pełny dostęp do uwierzytelnionego stanu ofiary bez konieczności posiadania jej pierwotnych danych uwierzytelniających.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Wysoki |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Narzędzia:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Czy identyfikator sesji jest wystarczająco złożony i losowy?
- [ ] Tak — token przechodzi statystyczne testy losowości (wysoka entropia), a jego obejście (bypass) **nie jest możliwe**  
- [ ] Tak — token jest długi, ale zawiera przewidywalne wzorce lub statyczne segmenty  
- [ ] Nie — token jest sekwencyjny, oparty na przewidywalnych znacznikach czasu lub posiada **krytycznie niską** entropię *(Krytyczne)*  

### Gdzie identyfikator sesji jest przesyłany i przechowywany?
- [ ] Identyfikator jest przechowywany w ciasteczku z flagami `Secure` i `HttpOnly` *(Najbardziej bezpieczne)*  
- [ ] Identyfikator jest przechowywany w ciasteczku, ale brakuje mu flag `Secure` lub `HttpOnly`  
- [ ] Identyfikator jest przesyłany **wyłącznie** za pomocą parametrów URL lub żądań `GET` *(Wysokie ryzyko)*  

### Czy aplikacja wystawia nowy identyfikator sesji po pomyślnym uwierzytelnieniu?
- [ ] Tak — nowy, unikalny identyfikator sesji (Session ID) **jest generowany** natychmiast po zalogowaniu  
- [ ] Nie — identyfikator sesji pozostaje taki sam przed i po zalogowaniu *(Możliwy Session Fixation)*  

### Czy identyfikator sesji jest powiązany z konkretnym adresem IP lub User-Agent, aby zapobiec ponownemu użyciu?
- [ ] Tak — sesja jest unieważniana, jeśli odcisk palca (fingerprint) klienta ulegnie znacznej zmianie  
- [ ] Nie — sesja **może** być użyta ponownie na różnych adresach IP lub urządzeniach bez dodatkowej weryfikacji  

### Czy identyfikatory sesji są poprawnie unieważniane po stronie serwera po wylogowaniu lub wygaśnięciu sesji?
- [ ] Tak — stan sesji po stronie serwera jest **całkowicie niszczony** po wylogowaniu  
- [ ] Nie — sesja pozostaje ważna na serwerze, nawet jeśli ciasteczko po stronie klienta zostało usunięte  
- [ ] Nie — sesja **nigdy nie wygasa** lub posiada nadmiernie długi czas wygaśnięcia (timeout)

---

## WSTG-SESS-02 — Testowanie atrybutów plików cookie

Zarządzanie sesją często opiera się na plikach cookie HTTP w celu utrzymania stanu, co sprawia, że atrybuty bezpieczeństwa tych plików są głównym celem atakujących. Pominięcie lub błędna konfiguracja atrybutów takich jak `HttpOnly`, `Secure` i `SameSite` naraża tokeny sesyjne na kradzież poprzez ataki typu Cross-Site Scripting (XSS), przechwycenie w kanałach nieszyfrowanych lub ataki Cross-Site Request Forgery (CSRF). Pentesterzy badają te flagi, aby ustalić, czy atakujący może wyekstrahować identyfikatory sesji z modelu obiektowego dokumentu (DOM) przeglądarki lub wymusić na przeglądarce ich wysłanie w żądaniach między źródłami (cross-origin). Prawidłowa konfiguracja atrybutów jest fundamentalnym środkiem strategii głębokiej obrony (defense-in-depth), zapewniającym, że wrażliwe tokeny pozostają ograniczone do autoryzowanych, bezpiecznych kontekstów.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Status testu** | Nie przeprowadzono |
| **Poziom krytyczności** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Narzędzia:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analiza implementacji flag plików cookie
| Atrybut | Obecny | Brak |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Czy flaga `HttpOnly` jest stosowana dla wrażliwych plików cookie sesji?
- [ ] Tak — zastosowano dla **wszystkich** wrażliwych plików cookie sesji *(Najbardziej bezpieczne)*  
- [ ] Tak — zastosowano tylko dla niektórych plików cookie sesji  
- [ ] Nie — flaga **nie została zastosowana**, co umożliwia dostęp poprzez skrypty po stronie klienta *(Krytyczne)*  

### Czy flaga `Secure` jest wymuszana dla plików cookie przesyłanych przez HTTPS?
- [ ] Tak — zastosowano dla **wszystkich** plików cookie przesyłanych przez TLS  
- [ ] Nie — flaga **nie została zastosowana**, co pozwala na przesyłanie plików cookie otwartym tekstem przez HTTP  

### Czy atrybut `SameSite` jest używany do mitygacji ryzyka CSRF?
- [ ] Tak — ustawiony na `Strict` lub `Lax` dla plików cookie sesji  
- [ ] Tak — ustawiony na `None`, ale z obecną flagą `Secure`  
- [ ] Nie — atrybutu **brakuje** lub jest ustawiony na `None` **bez** flagi `Secure`  

### Czy atrybuty `Domain` i `Path` są ograniczone do minimalnego niezbędnego zakresu?
- [ ] Tak — ograniczone do **konkretnej** subdomeny i ścieżki aplikacji  
- [ ] Nie — zakres jest zbyt **szeroki** (np. ustawiony na domenę nadrzędną lub root `/`), co zwiększa powierzchnię ataku  

### Czy wrażliwe pliki cookie sesji wygasają prawidłowo poprzez atrybuty `Expires` lub `Max-Age`?
- [ ] Tak — ustawiono odpowiednie daty wygaśnięcia  
- [ ] Nie — pliki cookie są trwałe (persistent) z nadmiernie **długim** czasem życia  
- [ ] Nie — pliki cookie są tylko sesyjne, ale **brakuje** wymuszenia limitu czasu (timeout) po stronie serwera

---

## WSTG-SESS-03 — Session Fixation

Utrwalanie sesji (Session Fixation) występuje, gdy aplikacja nie unieważnia lub nie rotuje identyfikatora sesji po pomyślnym uwierzytelnieniu użytkownika, co pozwala atakującemu na narzucenie ofierze znanego tokena sesji. Jeśli aplikacja zachowuje identyfikator sesji sprzed uwierzytelnienia po zalogowaniu, atakujący może dostarczyć ofierze specjalnie przygotowany link zawierający ustalony identyfikator sesji, a następnie przejąć uwierzytelnioną sesję. Podatność ta zazwyczaj objawia się w formularzach logowania lub poprzez identyfikatory sesji akceptowane w parametrach URL oraz plikach cookies. Z perspektywy atakującego, eksploatacja polega na "ustaleniu" (fixingu) sesji za pomocą złośliwego linku lub podatności typu header injection i oczekiwaniu, aż ofiara poda dane uwierzytelniające, co skutecznie eliminuje potrzebę kradzieży aktywnego tokena.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Status Testu** | Nie przeprowadzono |
| **Krytyczność** | Wysoka |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Czy identyfikator sesji zmienia się po pomyślnym uwierzytelnieniu?
- [ ] Tak — wydawany jest nowy identyfikator sesji, a stary zostaje unieważniony *(Najbezpieczniejsze)*  
- [ ] Tak — wydawany jest nowy identyfikator sesji, ale stary pozostaje ważny  
- [ ] Nie — identyfikator sesji pozostaje taki sam przed i po uwierzytelnieniu *(Krytyczne)*  

### Czy aplikacja akceptuje identyfikatory sesji przekazywane w parametrach URL?
- [ ] Nie — identyfikatory sesji są zarządzane wyłącznie (exclusively) przez pliki cookies  
- [ ] Tak — identyfikatory sesji są akceptowane w adresie URL, ale są ignorowane lub rotowane podczas logowania  
- [ ] Tak — identyfikatory sesji w adresie URL są akceptowane i zachowywane po uwierzytelnieniu  

### Czy identyfikator sesji zdefiniowany przez atakującego może zostać narzucony aplikacji?
- [ ] Nie — aplikacja odrzuca nieprawidłowe lub nieistniejące identyfikatory sesji dostarczone przez użytkownika  
- [ ] Tak — aplikacja akceptuje i inicjuje sesję przy użyciu dowolnego identyfikatora podanego w ciasteczku  
- [ ] Tak — aplikacja akceptuje i inicjuje sesję przy użyciu dowolnego identyfikatora podanego w parametrze URL  

### Czy sesje sprzed uwierzytelnienia są poprawnie kończone?
- [ ] Tak — anonimowa sesja jest w pełni niszczona (fully destroyed) podczas zdarzenia logowania  
- [ ] Nie — anonimowa sesja nie jest niszczona, co umożliwia potencjalne zbieranie sesji (session harvesting)  

### Czy ciasteczko sesyjne jest chronione przed wstrzyknięciem po stronie klienta?
- [ ] Tak — flagi `HttpOnly` i `Secure` są stosowane, aby zapobiec utrwalaniu sesji za pomocą skryptów  
- [ ] Nie — flagi nie są stosowane, co pozwala na ustawianie identyfikatorów sesji poprzez Cross-Site Scripting (XSS)

---

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

## WSTG-SESS-05 — Testowanie pod kątem Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) to podatność, w której atakujący nakłania przeglądarkę ofiary do wykonania niepożądanego działania w innej witrynie, w której ofiara jest obecnie uwierzytelniona. Exploit ten wykorzystuje mechanizm przeglądarki polegający na automatycznym dołączaniu poświadczeń kontekstowych ("ambient" credentials), takich jak pliki cookie sesji lub nagłówki Authorization, do wychodzących żądań. Atakujący zazwyczaj celują w operacje zmieniające stan (state-changing operations), takie jak zmiana haseł, aktualizacja adresów e-mail lub wykonywanie przelewów finansowych, poprzez hostowanie złośliwych skryptów lub ukrytych formularzy w witrynie osoby trzeciej. Skuteczna eksploatacja może prowadzić do pełnego przejęcia konta (account takeover) lub nieautoryzowanej modyfikacji danych bez wiedzy lub zgody użytkownika.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się wysoki (High), jeśli podatna akcja pozwala na przejęcie konta, eskalację uprawnień (privilege escalation) lub nieautoryzowane transakcje finansowe.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Narzędzia:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Czy tokeny anti-CSRF są zaimplementowane dla żądań zmieniających stan?
- [ ] Tak — unikalne, silne kryptograficznie tokeny są wymagane dla wszystkich akcji zmieniających stan  
- [ ] Tak — tokeny są obecne, ale **nie** są unikalne dla sesji lub są przewidywalne  
- [ ] Nie — tokeny anti-CSRF **nie** zostały zaimplementowane  

### Czy walidacja tokena anti-CSRF po stronie serwera jest solidna?
- [ ] Tak — serwer rygorystycznie waliduje token i obejście (bypass) **nie jest możliwe**  
- [ ] Tak — walidacja jest wykonywana, ale **można** ją obejść poprzez usunięcie parametru tokena  
- [ ] Tak — walidacja jest wykonywana, ale **można** ją obejść poprzez podanie pustego lub fikcyjnego tokena  
- [ ] Tak — walidacja jest wykonywana, ale token **nie** jest powiązany z sesją użytkownika  

### Czy aplikacja polega na łatwych do obejścia metodach ochrony przed CSRF?
- [ ] Nie — aplikacja nie polega wyłącznie na słabych nagłówkach lub samym sprawdzeniu pochodzenia (origin checks)  
- [ ] Tak — ochrona opiera się wyłącznie na nagłówku `Referer` lub `Origin`, który **może** zostać sfałszowany lub usunięty  
- [ ] Tak — ochrona opiera się na sprawdzeniu nagłówka `X-Requested-With`, który **może** zostać pominięty poprzez błędną konfigurację CORS  

### Czy pliki cookie sesji są skonfigurowane z atrybutem `SameSite`?
- [ ] Tak — atrybut `SameSite` jest ustawiony na `Strict` lub `Lax` dla wszystkich plików cookie związanych z sesją  
- [ ] Nie — atrybut `SameSite` jest **nieobecny**, co powoduje domyślne zachowanie zależne od przeglądarki  
- [ ] Nie — `SameSite` jest jawnie ustawiony na `None` bez flagi `Secure` lub dodatkowych zabezpieczeń  

### Czy ochrona przed CSRF może zostać pominięta poprzez zmianę metody żądania HTTP?
- [ ] Nie — ochrona jest egzekwowana niezależnie od użytej metody HTTP  
- [ ] Tak — tokeny są walidowane tylko w żądaniach `POST`, ale akcja **może** zostać wykonana za pomocą `GET`  
- [ ] Tak — zmiana metody (np. na `PUT` lub `DELETE`) pozwala na pominięcie sprawdzenia tokena

---

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

## WSTG-SESS-07 — Testowanie limitu czasu sesji (Session Timeout)

Testowanie limitu czasu sesji ocenia, czy aplikacja skutecznie kończy sesję użytkownika po uprzednio zdefiniowanym okresie bezczynności lub całkowitym czasie jej trwania. Mechanizm ten jest kluczowy dla mitygacji ryzyka Session Hijacking, szczególnie na współdzielonych stacjach roboczych lub w scenariuszach, w których atakujący przechwytuje identyfikator sesji poprzez przechwytywanie ruchu sieciowego lub Cross-Site Scripting (XSS). Pentesterzy analizują zarówno Idle Timeout (limit czasu bezczynności), który aktywuje się po okresie braku aktywności, jak i Absolute Timeout (bezwzględny limit czasu), który ogranicza całkowity czas życia sesji bez względu na aktywność. Z perspektywy atakującego, nieokreślone lub nadmiernie długie sesje zapewniają wydłużone okno czasowe na utrzymanie nieautoryzowanego dostępu i pozwalają uniknąć konieczności ponownego uwierzytelnienia.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom ważności** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Czy aplikacja implementuje limit czasu bezczynności sesji (idle session timeout)?
- [ ] Tak — sesja wygaśnie po krótkim okresie (np. 15-30 minut) bezczynności  
- [ ] Tak — sesja wygaśnie, ale czas trwania limitu jest **nadmiernie długi** (np. > 24 godziny)  
- [ ] Nie — sesja **pozostaje aktywna** w nieskończoność podczas bezczynności  

### Czy limit czasu sesji jest wymuszany po stronie serwera (server-side)?
- [ ] Tak — serwer odrzuca żądania z wygasłymi tokenami niezależnie od stanu po stronie klienta  
- [ ] Nie — limit czasu jest wymuszany **wyłącznie** za pomocą JavaScript po stronie klienta lub meta-refresh  

### Czy aplikacja implementuje bezwzględny limit czasu sesji (absolute session timeout)?
- [ ] Tak — sesja jest kończona po ustalonym maksymalnym czasie trwania, niezależnie od aktywności  
- [ ] Nie — sesja **może** być przedłużana w nieskończoność, dopóki trwa ciągła aktywność  

### Czy identyfikator sesji jest unieważniany na serwerze po wygaśnięciu limitu czasu?
- [ ] Tak — token sesji **nie może** zostać ponownie użyty po osiągnięciu progu limitu czasu  
- [ ] Nie — token sesji **nadal jest ważny** na serwerze, nawet po przekierowaniu klienta do strony logowania  

### Czy sesja może być utrzymywana w nieskończoność przy użyciu zautomatyzowanych żądań heartbeat?
- [ ] Nie — Absolute Timeout lub dodatkowe mechanizmy kontrolne **zapobiegają** nieskończonemu przedłużaniu sesji  
- [ ] Tak — okresowe żądania w tle (np. AJAX) **mogą** utrzymywać sesję w nieskończoność

---

## WSTG-SESS-08 — Session Puzzling

Session Puzzling, znany również jako Session Variable Overloading (przeciążanie zmiennych sesyjnych), występuje, gdy aplikacja wykorzystuje tę samą zmienną sesyjną do wielu celów w różnych modułach lub nie czyści poprawnie stanów sesji podczas przejść między procesami (workflow). Atakujący wykorzystują to zachowanie, wypełniając zmienne sesyjne w jednym kontekście — takim jak podgląd profilu osoby nieuwierzytelnionej lub wieloetapowa rejestracja — a następnie przechodząc do obszaru chronionego, gdzie aplikacja błędnie ufa tym wcześniej istniejącym zmiennym. Może to prowadzić do krytycznych skutków, w tym ominięcia uwierzytelniania (Authentication Bypass), eskalacji uprawnień (Privilege Escalation) lub nieautoryzowanego dostępu do poufnych danych poprzez oszukanie aplikacji, aby uznała, że sesja znajduje się w stanie o wyższych uprawnieniach niż w rzeczywistości. Podatność ta jest najczęściej spotykana w złożonych, stanowych aplikacjach, w których logika zarządzania sesją jest stosowana niespójnie w różnych komponentach funkcjonalnych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### Czy aplikacja używa tych samych nazw zmiennych sesyjnych do różnych celów w oddzielnych modułach?
- [ ] Nie — nazwy zmiennych są unikalne lub zakresy (scopes) są odizolowane  
- [ ] Tak — istnieją wspólne nazwy zmiennych, ale stan jest czyszczony między modułami  
- [ ] Tak — istnieją wspólne nazwy zmiennych, a stan **nie jest** czyszczony  

### Czy działania nieuwierzytelnione mogą wpływać na zmienne sesyjne używane w kontekstach uwierzytelnionych?
- [ ] Nie — zmienne sesyjne są inicjowane dopiero **po** pomyślnym uwierzytelnieniu  
- [ ] Tak — zmienne sesyjne mogą być ustawiane przed logowaniem, ale **nie wpływają** na stan po zalogowaniu  
- [ ] Tak — zmienne sesyjne ustawione przed uwierzytelnieniem **są** obdarzone zaufaniem po uwierzytelnieniu *(Krytyczne)*  

### Czy aplikacja ponownie inicjuje identyfikator sesji i powiązane z nim zmienne po zmianie poziomu uprawnień?
- [ ] Tak — sesja jest w pełni resetowana i **wydawany jest** nowy identyfikator (ID)  
- [ ] Częściowo — wydawany jest nowy identyfikator, ale niektóre zmienne sesyjne **pozostają**  
- [ ] Nie — identyfikator sesji i wszystkie zmienne **pozostają** bez zmian  

### Czy uprawnienia administracyjne mogą zostać uzyskane poprzez manipulację zmiennymi sesyjnymi za pomocą konta bez uprawnień?
- [ ] Nie — zmienne dotyczące uprawnień **nie mogą** być modyfikowane przez użytkowników  
- [ ] Tak — zmienne mogą być modyfikowane, ale aplikacja **weryfikuje** je w bazie danych po stronie serwera (backend)  
- [ ] Tak — zmienne mogą być modyfikowane, a aplikacja bezkrytycznie **ufa** stanowi sesji *(Krytyczne)*  

### Czy stan sesji jest czyszczony natychmiast po zakończeniu konkretnego przepływu pracy (np. resetowanie hasła, realizacja zakupu)?
- [ ] Tak — zmienne sesyjne dla danego przepływu są niszczone po jego zakończeniu  
- [ ] Nie — zmienne przepływu **pozostają** i mogą być ponownie użyte w innych obszarach aplikacji

---

## WSTG-SESS-09 — Testowanie pod kątem przejmowania sesji (Session Hijacking)

Przejmowanie sesji (Session Hijacking) występuje, gdy atakujący przechwytuje, przewiduje lub wymusza (Session Fixation) prawidłowy identyfikator sesji (SID), aby uzyskać nieautoryzowany dostęp do aktywnej sesji użytkownika. Podatność ta zazwyczaj wynika z niewystarczającego bezpieczeństwa warstwy transportowej, braku flag bezpieczeństwa w plikach cookie lub przewidywalnych algorytmów generowania SID, co pozwala atakującemu na całkowite obejście uwierzytelniania. Z perspektywy atakującego, skuteczne wykorzystanie podatności umożliwia pełne podszycie się pod ofiarę bez konieczności posiadania jej danych uwierzytelniających, co często osiąga się poprzez sniffing sieciowy, Cross-Site Scripting (XSS) lub ataki typu session fixation.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Narzędzia:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Czy identyfikatory sesji są chronione podczas przesyłania?
- [ ] Tak — flaga `Secure` jest **włączona** i HSTS jest rygorystycznie wymuszany  
- [ ] Tak — flaga `Secure` jest **włączona**, ale HSTS jest **wyłączony**  
- [ ] Nie — flaga `Secure` **nie jest stosowana**, co pozwala na przechwycenie SID przez niezaszyfrowane kanały *(Wysoki)*  

### Czy identyfikator sesji jest chroniony przed dostępem skryptów po stronie klienta?
- [ ] Tak — flaga `HttpOnly` jest **stosowana** do wszystkich plików cookie powiązanych z sesją  
- [ ] Nie — flaga `HttpOnly` **nie jest stosowana**, co pozwala na eksfiltrację SID poprzez XSS *(Krytyczny)*  

### Czy aplikacja implementuje powiązanie sesji z atrybutami klienta?
- [ ] Tak — sesja jest powiązana z atrybutami klienta (IP/User-Agent) i ponowne użycie **nie jest możliwe**  
- [ ] Tak — powiązanie sesji (Session Binding) istnieje, ale możliwe jest **obejście** poprzez spoofing nagłówków  
- [ ] Nie — brak powiązania sesji; SID **jest ważny** przy użyciu z dowolnego źródła  

### Czy identyfikator sesji jest wystarczająco losowy, aby zapobiec przewidywaniu?
- [ ] Tak — SID wykorzystuje kryptograficznie bezpieczny generator liczb pseudolosowych (CSPRNG)  
- [ ] Nie — długość SID jest niewystarczająca lub następuje po **przewidywalnej** sekwencji/wzorcu  

### Czy sesje współbieżne są zarządzane w celu ograniczenia okna ataku?
- [ ] Nie — aplikacja rygorystycznie wymusza pojedynczą aktywną sesję na użytkownika  
- [ ] Tak — wiele współbieżnych sesji jest **włączonych**, ale są one monitorowane  
- [ ] Tak — możliwe jest posiadanie **nieograniczonej liczby współbieżnych sesji** na różnych urządzeniach/lokalizacjach

---

## WSTG-SESS-10 — Testowanie JSON Web Tokens

Tokeny JSON Web Tokens (JWT) są powszechnie stosowane do bezstanowego zarządzania sesjami i propagacji tożsamości, jednak ich bezpieczeństwo zależy całkowicie od poprawnej implementacji algorytmów podpisu oraz rygorystycznej weryfikacji roszczeń (claims). Atakujący często próbują obejść uwierzytelnianie, wykorzystując błędy algorytmu „none”, łamiąc metodą Brute Force słabe klucze tajne HS256 lub przeprowadzając ataki typu algorithm-switching, w których asymetryczny klucz publiczny jest używany jako symetryczny sekret HMAC. Podatności te zazwyczaj występują w ciasteczkach sesyjnych lub nagłówkach Authorization, potencjalnie umożliwiając atakującemu eskalację uprawnień lub podszycie się pod dowolnego użytkownika poprzez modyfikację roszczeń, takich jak `role` lub `user_id`, bez naruszania kryptograficznej spójności tokena.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Materiały źródłowe:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Narzędzia:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### Czy serwer akceptuje algorytm „none”?
- [ ] Nie — serwer **odrzuca** tokeny używające `alg: none` w nagłówku  
- [ ] Tak — serwer **akceptuje** tokeny z `alg: none` i pustym podpisem *(Krytyczny)*  

### W jaki sposób weryfikowany jest podpis JWT i jak jest chroniony przed atakami Brute Force?
- [ ] Tak — używane są silne klucze asymetryczne (RS256/ES256) lub sekrety HMAC o wysokiej entropii  
- [ ] Tak — używany jest algorytm HS256, ale sekret **może** zostać złamany metodą Brute Force ze względu na niską entropię lub wartości domyślne  
- [ ] Nie — weryfikacja podpisu **nie jest stosowana** lub **może** zostać pominięta poprzez atak typu algorithm switching (z RS256 na HS256)  

### Czy standardowe roszczenia (exp, nbf, iat) są rygorystycznie egzekwowane przez backend?
- [ ] Tak — roszczenie `exp` (wygaśnięcie) jest obecne i **nie może** zostać pominięte  
- [ ] Tak — roszczenie `exp` jest obecne, ale serwer **nie egzekwuje** go podczas walidacji  
- [ ] Nie — roszczenia dotyczące wygaśnięcia są **nieobecne** lub ignorowane, co pozwala na bezterminowe ataki typu session replay  

### Czy implementacja zapobiega wstrzykiwaniu kluczy poprzez nagłówki (kid, jku, x5u)?
- [ ] Tak — nagłówki są sanityzowane i **dozwolone** są tylko zaufane wewnętrzne klucze/źródła  
- [ ] Nie — nagłówek `kid` (Key ID) jest podatny na directory traversal lub SQL Injection w celu wskazania na znany plik  
- [ ] Nie — nagłówki `jku` lub `x5u` **mogą** wskazywać na adresy URL kontrolowane przez atakującego w celu załadowania złośliwych kluczy

---

## WSTG-SESS-11 — Testing for Concurrent Sessions

Testowanie współbieżnych sesji (Concurrent Sessions) ocenia, czy aplikacja zezwala na utrzymywanie przez jedno konto użytkownika wielu jednoczesnych aktywnych sesji w różnych przeglądarkach, urządzeniach lub adresach IP. Brak ograniczeń dotyczących współbieżnych sesji zwiększa pole manewru dla atakujących, umożliwiając wykorzystanie przejętych tokenów sesyjnych (session tokens) lub skompromitowanych danych uwierzytelniających bez powiadamiania legalnego użytkownika lub przerywania istniejących sesji. Zachowanie to jest zazwyczaj oceniane poprzez uwierzytelnianie z wielu źródeł i monitorowanie stabilności sesji, co często ujawnia słabości w logice zarządzania sesjami lub brak reguł biznesowych skoncentrowanych na bezpieczeństwie. Z perspektywy atakującego, brak kontroli współbieżności pozwala na skryte utrzymywanie dostępu (persistence) po początkowym naruszeniu bezpieczeństwa i ułatwia równoległe ataki automatyczne.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Niski / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Czy sesje współbieżne są ograniczone przez politykę aplikacji?
- [ ] Tak — dozwolona jest tylko jedna sesja na użytkownika; nowe logowania unieważniają poprzednie *(Najbardziej bezpieczne)*  
- [ ] Tak — wymuszana jest stała liczba współbieżnych sesji, której nie można przekroczyć  
- [ ] Nie — możliwa jest nieograniczona liczba współbieżnych sesji  

### Jak aplikacja reaguje po osiągnięciu limitu sesji?
- [ ] Najstarsza aktywna sesja **jest automatycznie przerywana** (mitygacja session fixation/hijacking)  
- [ ] Użytkownik **jest proszony** o zakończenie istniejących sesji przed autoryzacją nowej sesji  
- [ ] Nowa próba logowania **jest blokowana** do czasu ręcznego wylogowania z innego urządzenia  
- [ ] Nie są podejmowane żadne działania — wiele sesji pozostaje **aktywnych** jednocześnie  

### Czy aplikacja udostępnia użytkownikowi interfejs do zarządzania sesjami?
- [ ] Tak — użytkownicy **mogą** przeglądać aktywne sesje (IP, urządzenie, czas) i zdalnie je kończyć  
- [ ] Tak — użytkownicy **mogą** przeglądać aktywne sesje, ale **nie mogą** ich zdalnie kończyć  
- [ ] Nie — użytkownicy **nie mogą** widzieć ani zarządzać innymi aktywnymi sesjami powiązanymi z ich kontem  

### Czy użytkownik jest powiadamiany o wykryciu aktywności współbieżnego logowania?
- [ ] Tak — alert (e-mail, SMS lub wewnątrz aplikacji) **jest generowany** przy logowaniu z nowych urządzeń lub lokalizacji  
- [ ] Nie — żadne powiadomienie **nie jest dostarczane**, gdy ustanawiane są współbieżne sesje

---

## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Reflected Cross Site Scripting (XSS) występuje, gdy aplikacja dołącza niezaufane dane do odpowiedzi HTTP bez wystarczającej walidacji lub kodowania, co powoduje wykonanie ładunku (Payload) w kontekście przeglądarki ofiary. Atakujący dostarczają złośliwe ładunki (Payloads) za pomocą socjotechniki, zazwyczaj poprzez spreparowane adresy URL lub formularze, w celu przejęcia sesji użytkownika, eksfiltracji wrażliwych plików cookie lub wykonania nieautoryzowanych działań w imieniu użytkownika. Podatność ta jest powszechnie spotykana w parametrach wyszukiwania, komunikatach o błędach i wszelkich punktach końcowych (endpoints), które odzwierciedlają dane wejściowe bezpośrednio w interfejsie użytkownika. Eksploatacja opiera się na interakcji ofiary ze złośliwym linkiem, co czyni go nietrwałym, ale wysoce skutecznym wektorem ataku (attack vector) wymierzonym w konkretnych administratorów lub uwierzytelnionych użytkowników.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki (High) |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Czy dane wejściowe dostarczone przez użytkownika są odzwierciedlane w treści odpowiedzi (response body)?
- [ ] Nie — dane wejściowe **nigdy** nie są odzwierciedlane użytkownikowi  
- [ ] Tak — dane wejściowe są odzwierciedlane, ale poprawnie zakodowane i obejście (Bypass) **nie jest możliwe**  
- [ ] Tak — dane wejściowe są odzwierciedlane **bez** żadnego kodowania ani sanityzacji  

### Czy zaimplementowano kodowanie wyjściowe uwzględniające kontekst (context-aware output encoding)?
- [ ] Tak — odpowiednie kodowanie (HTML, Attribute, JavaScript) jest **stosowane** w oparciu o specyficzny kontekst odzwierciedlenia  
- [ ] Tak — kodowanie jest **stosowane**, ale jest niewystarczające dla danego kontekstu (np. kodowanie HTML wewnątrz tagu `<script>`)  
- [ ] Nie — kodowanie **nie jest stosowane**  

### Czy można obejść walidację danych wejściowych lub filtry Web Application Firewall (WAF)?
- [ ] Nie — filtry skutecznie blokują wszystkie powszechne i zaciemnione (obfuscated) ładunki XSS  
- [ ] Tak — filtry są obecne, ale obejście (**Bypass**) jest możliwe przy użyciu kodowania znaków, niestandardowych tagów lub poliglottów (polyglots)  
- [ ] Nie — brak mechanizmów filtracji lub walidacji  

### Jaki jest kontekst wykonywania odzwierciedlonych danych wejściowych?
- [ ] Bezpieczny — dane wejściowe są odzwierciedlane w miejscu niewykonywalnym (np. wewnątrz standardowych tagów `<div>` lub `<span>`)  
- [ ] Ryzykowny — dane wejściowe są odzwierciedlane wewnątrz atrybutów HTML lub w parametrach `src`/`href` tagów  
- [ ] Krytyczny — dane wejściowe są odzwierciedlane bezpośrednio w blokach `<script>`, programach obsługi zdarzeń (event handlers) lub literałach szablonowych (template literals)  

### Czy obecne są nowoczesne nagłówki bezpieczeństwa przeglądarki w celu złagodzenia skutków XSS?
- [ ] Tak — `Content-Security-Policy` (CSP) jest **włączony** z restrykcyjną polityką script-src  
- [ ] Tak — `Content-Security-Policy` (CSP) jest **włączony**, ale zawiera `unsafe-inline` lub słabe białe listy (whitelists)  
- [ ] Nie — brak nagłówków CSP lub przestarzałych nagłówków `X-XSS-Protection`

---

## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Stored Cross Site Scripting (XSS), znany również jako Persistent XSS (trwały XSS), występuje, gdy aplikacja odbiera dane od użytkownika i przechowuje je w trwałej bazie danych, systemie plików lub innym nośniku danych bez odpowiedniej walidacji lub kodowania. Gdy ofiara przechodzi następnie na stronę, która pobiera i wyświetla te niezsanitowane dane, złośliwy skrypt wykonuje się w kontekście jej przeglądarki w ramach źródła (origin) aplikacji. Podatność ta jest szczególnie niebezpieczna, ponieważ nie wymaga od ofiary kliknięcia w specjalnie przygotowany link; każdy użytkownik przeglądający zainfekowaną stronę staje się celem, co może prowadzić do przejęcia sesji (Session Hijacking), nieautoryzowanych działań lub wyłudzania poświadczeń (Credential Harvesting). Atakujący zazwyczaj celują w sekcje komentarzy, profile użytkowników, fora dyskusyjne oraz logi administracyjne, gdzie wprowadzone dane są stale renderowane dla innych użytkowników lub administratorów.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Wysoki (High) |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Narzędzia:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Czy istnieją punkty wejścia, które przechowują dane wejściowe użytkownika w celu późniejszego wyświetlenia ich innym użytkownikom?
- [ ] Nie — aplikacja nie przechowuje danych kontrolowanych przez użytkownika do późniejszego renderowania  
- [ ] Tak — dane wejściowe są przechowywane (np. profile, komentarze), ale **nie** są renderowane w kontekście HTML  
- [ ] Tak — dane wejściowe są przechowywane i renderowane dla innych użytkowników lub administratorów  

### Czy podczas renderowania zapisanych danych stosowane jest kodowanie danych wyjściowych lub sanityzacja?
- [ ] Tak — kodowanie wyjściowe zależne od kontekstu (context-aware output encoding) **jest stosowane** i obejście **nie jest możliwe** *(Najbezpieczniej)*  
- [ ] Tak — sanityzacja (np. `DOMPurify`) **jest stosowana**, ale obejście **jest możliwe** z powodu błędów w konfiguracji  
- [ ] Nie — dane są renderowane bezpośrednio w drzewie DOM **bez** kodowania lub sanityzacji *(Poziom wysoki)*  

### Czy walidacja lub sanityzacja danych wejściowych może zostać pominięta przy użyciu alternatywnych payloadów?
- [ ] Nie — filtry w bezpieczny sposób obsługują różne rodzaje kodowania, niestandardowe znaczniki oraz programy obsługi zdarzeń (event handlers)  
- [ ] Tak — obejście **jest możliwe** przy użyciu kodowania encji HTML lub specyficznych znaczników (np. `<svg>`, `<img>`)  
- [ ] Tak — obejście **jest możliwe** poprzez payloady typu polyglot lub Mutation-based XSS (mXSS)  

### Czy Content Security Policy (CSP) zapewnia dodatkową warstwę ochrony?
- [ ] Tak — restrykcyjna polityka CSP jest **włączona** i zapobiega wykonywaniu skryptów inline oraz ładowaniu skryptów z nieautoryzowanych źródeł  
- [ ] Tak — polityka CSP jest **włączona**, ale jest błędnie skonfigurowana (np. `unsafe-inline` lub zbyt szeroki `script-src`)  
- [ ] Nie — polityka CSP **nie jest włączona** dla aplikacji  

### Czy możliwe jest osiągnięcie "Stored XSS to RCE" lub innych łańcuchów o wysokim wpływie (Blind XSS)?
- [ ] Nie — wpływ jest ograniczony do kontekstu przeglądarki po stronie klienta  
- [ ] Tak — Stored XSS **może** zostać użyty do atakowania administratorów (Blind XSS) w panelach wewnętrznych  
- [ ] Tak — Stored XSS **może** zostać połączony z innymi podatnościami (np. CSRF, eksfiltracja wrażliwych danych)

---

## WSTG-INPV-03 — Testing for HTTP Verb Tampering

HTTP Verb Tampering polega na wykorzystywaniu słabości w sposobie, w jaki serwery WWW i frameworki aplikacji autoryzują dostęp do konkretnych zasobów w oparciu o użytą metodę HTTP. Atakujący próbują obejść ograniczenia bezpieczeństwa, zastępując standardowe metody, takie jak `GET` lub `POST`, alternatywami takimi jak `HEAD`, `PUT`, `OPTIONS`, a nawet dowolnymi, niestandardowymi ciągami znaków, które backend może przetwarzać w sposób niespójny. Podatność ta występuje zazwyczaj, gdy konfiguracje bezpieczeństwa — takie jak filtry web.xml w Java EE lub reguły autoryzacji .NET — jawnie wymieniają dozwolone metody, ale nie uwzględniają innych, lub gdy stosowane jest podejście „deny-by-method” zamiast „deny-all”. Skuteczna eksploatacja może skutkować nieautoryzowanym dostępem do funkcji administracyjnych, modyfikacją danych lub ujawnieniem informacji dotyczących wewnętrznej konfiguracji serwera.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom ważności** | Średni / Wysoki* |

> *Poziom ważności staje się Wysoki, jeśli funkcje administracyjne lub modyfikacja danych (PUT/DELETE) mogą być wykonywane bez uwierzytelnienia.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### Czy aplikacja odpowiada na niestandardowe lub dowolne metody HTTP?
- [ ] Nie — aplikacja odrzuca wszystkie metody z wyjątkiem tych jawnie wymaganych  
- [ ] Tak — aplikacja odpowiada na standardowe metody (OPTIONS, TRACE), ale **nie można** obejść zabezpieczeń  
- [ ] Tak — aplikacja akceptuje dowolne ciągi znaków (np. FOO, TEST) i przetwarza je jako żądania `GET` lub `POST`  

### Czy można obejść uwierzytelnianie/autoryzację poprzez zmianę metody HTTP?
- [ ] Nie — mechanizmy kontroli bezpieczeństwa są stosowane globalnie, niezależnie od metody HTTP (Verb)  
- [ ] Tak — mechanizmy kontroli istnieją, ale obejście **jest możliwe** przy użyciu metody `HEAD`  
- [ ] Tak — mechanizmy kontroli bezpieczeństwa **nie są stosowane** do metod alternatywnych, co pozwala na nieautoryzowany dostęp do chronionych punktów końcowych (API)  

### Czy na serwerze włączone są niebezpieczne metody, takie jak `PUT` lub `DELETE`?
- [ ] Nie — niebezpieczne metody są **wyłączone** lub zwracają kod 405 Method Not Allowed  
- [ ] Tak — metody są włączone, ale wymagają poprawnego uwierzytelnienia z wysokimi uprawnieniami  
- [ ] Tak — metody są **włączone** i pozwalają na nieautoryzowane przesyłanie plików (Exploit) lub usuwanie zasobów *(Krytyczne)*  

### Czy metoda `OPTIONS` ujawnia wrażliwe informacje o dozwolonych metodach?
- [ ] Nie — metoda `OPTIONS` jest **wyłączona** lub zwraca ogólną odpowiedź  
- [ ] Tak — `OPTIONS` zwraca nagłówek `Allow`, ale nie ujawnia chronionych metod wewnętrznych  
- [ ] Tak — `OPTIONS` ujawnia metody wyłącznie wewnętrzne lub administracyjne, co ułatwia dalszą eksploatację  

### Czy metoda `TRACE` jest włączona, potencjalnie umożliwiając Cross-Site Tracing (XST)?
- [ ] Nie — metody `TRACE` i `TRACK` są **wyłączone**  
- [ ] Tak — metoda `TRACE` jest **włączona** i zwraca nagłówki HTTP, w tym wrażliwe pliki cookie/tokeny

---

## WSTG-INPV-04 — Testowanie pod kątem HTTP Parameter Pollution

HTTP Parameter Pollution (HPP) występuje, gdy aplikacja otrzymuje wiele parametrów HTTP o tej samej nazwie i przetwarza je w sposób niespójny lub niebezpieczny. Dostarczając zduplikowane parametry, atakujący może manipulować wewnętrzną logiką aplikacji, potencjalnie omijając Web Application Firewalls (WAF), filtry walidacji danych wejściowych lub mechanizmy kontroli dostępu. Podatność ta jest silnie uzależniona od używanego serwera WWW lub frameworka aplikacji, który może wybrać pierwsze, ostatnie lub połączone (skonkatenowane) wystąpienie powtarzających się parametrów. Eksploatacja zazwyczaj celuje w endpointy przekazujące parametry do wewnętrznych interfejsów API, zapytań do baz danych lub mechanizmów przekierowań URL, co prowadzi do skutków od Cross-Site Scripting (XSS) po nieautoryzowaną eksfiltrację danych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Status testu** | Nie przeprowadzono |
| **Krytyczność** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Jak aplikacja obsługuje wiele parametrów HTTP o tej samej nazwie?
- [ ] Nie — zduplikowane parametry są **odrzucane** lub ignorowane przez serwer  
- [ ] Tak — serwer konsekwentnie wybiera tylko **pierwsze** lub **ostatnie** wystąpienie  
- [ ] Tak — serwer **łączy (konkatenuje)** wartości, potencjalnie umożliwiając wstrzyknięcie (injection)  

### Czy HPP można wykorzystać do obejścia zabezpieczeń, takich jak WAF lub filtry danych wejściowych?
- [ ] Nie — zabezpieczenia prawidłowo sprawdzają **wszystkie** wystąpienia parametru  
- [ ] Tak — WAF lub filtr sprawdza tylko **pierwsze** wystąpienie, pozwalając złośliwemu **drugiemu** wystąpieniu na przejście  
- [ ] Tak — konkatenacja pozwala atakującemu na **podzielenie** Payloadu pomiędzy wiele parametrów w celu uniknięcia wykrycia  

### Czy aplikacja odzwierciedla zanieczyszczone parametry w odpowiedzi bez odpowiedniej sanitacji?
- [ ] Nie — parametry są sanitowane lub kodowane przed odzwierciedleniem w interfejsie użytkownika (UI)  
- [ ] Tak — zanieczyszczenie skutkuje odzwierciedloną treścią, ale sanitacja **jest stosowana**  
- [ ] Tak — zanieczyszczenie skutkuje odzwierciedloną treścią, a sanitacja **nie jest stosowana**, co prowadzi do XSS lub błędów logicznych  

### Czy parametry przekazywane do wewnętrznych wywołań API lub systemów back-end są podatne na zanieczyszczenie?
- [ ] Nie — żądania back-endowe wykorzystują bezpieczne, niedynamiczne metody konstruowania  
- [ ] Tak — HPP pozwala atakującemu na **wstrzyknięcie** lub **nadpisanie** parametrów w strukturze żądania back-endowego  

### Czy aplikacja wykazuje inne zachowanie, gdy parametry są zanieczyszczone w żądaniach GET w porównaniu do POST?
- [ ] Nie — zachowanie jest spójne dla wszystkich metod HTTP  
- [ ] Tak — tylko parametry GET są podatne na zanieczyszczenie  
- [ ] Tak — tylko parametry POST (body) są podatne na zanieczyszczenie

---

## WSTG-INPV-05 — SQL Injection

SQL Injection występuje, gdy dane wejściowe dostarczone przez użytkownika są włączane do zapytań SQL bez odpowiedniej parametryzacji (parameterization) lub oczyszczania (sanitization), co pozwala atakującym na manipulowanie logiką zapytania. Udana eksploatacja (exploitation) może prowadzić do nieautoryzowanej eksfiltracji danych (data exfiltration), obejścia uwierzytelniania (authentication bypass), modyfikacji danych, a w niektórych przypadkach do zdalnego wykonywania kodu (remote code execution) poprzez funkcje bazy danych, takie jak `xp_cmdshell` lub `LOAD_FILE()`. Podatność ta powszechnie występuje w formularzach logowania, funkcjach wyszukiwania, parametrach REST API, nagłówkach HTTP oraz we wszelkich punktach końcowych (endpoints), które tworzą dynamiczne zapytania SQL na podstawie danych kontrolowanych przez użytkownika. Z perspektywy atakującego, zidentyfikowanie punktu iniekcji (injection point) stanowi główną bramę do pełnego przejęcia bazy danych i potencjalnego poruszania się bocznego (lateral movement) wewnątrz infrastruktury.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Status testu** | Nie wykonano |
| **Krytyczność** | Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Narzędzia:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Czy parametry kontrolowane przez użytkownika są przetwarzane przy użyciu bezpiecznych metod dostępu do bazy danych?
- [ ] Tak — aplikacja używa wyłącznie ORM lub zapytań parametryzowanych (parameterized queries) i ich obejście **nie jest możliwe**  
- [ ] Tak — używane są zapytania parametryzowane, ale obejście **jest możliwe** w przypadkach brzegowych (np. klauzule `ORDER BY`)  
- [ ] Nie — do budowania zapytań SQL używana jest bezpośrednia konkatenacja ciągów znaków (raw string concatenation) **bez** parametryzacji  

### Czy mechanizm uwierzytelniania może zostać pominięty poprzez SQL Injection?
- [ ] Nie — parametry logowania **nie są** podatne na iniekcję  
- [ ] Tak — obejście uwierzytelniania (authentication bypass) **jest możliwe** przy użyciu ładunków (payloads) opartych na tautologii (np. `' OR 1=1 --`)  

### Czy możliwa jest eksfiltracja danych za pomocą technik in-band lub out-of-band?
- [ ] Nie — nie zidentyfikowano eksfiltracji danych  
- [ ] Tak — eksfiltracja UNION-based lub error-based **jest możliwa**  
- [ ] Tak — eksfiltracja typu blind (Boolean lub Time-based) **jest możliwa**  
- [ ] Tak — eksfiltracja out-of-band (DNS/HTTP) **jest możliwa**  

### Czy funkcje aplikacji wykazują podatności na SQL Injection drugiego stopnia (second-order)?
- [ ] Nie — przechowywane dane są bezpiecznie obsługiwane podczas ich pobierania do późniejszych zapytań  
- [ ] Tak — dane wejściowe użytkownika są przechowywane, a następnie używane w niebezpiecznym zapytaniu SQL **bez** walidacji  

### Czy uprawnienia konta serwisowego bazy danych są ograniczone do niezbędnego minimum?
- [ ] Tak — konto serwisowe posiada **najniższe uprawnienia** (least privilege) (np. ograniczone do konkretnych tabel/działań)  
- [ ] Nie — konto serwisowe posiada podwyższone uprawnienia (np. uprawnienia `DROP TABLE`, `FILE` lub **włączoną** funkcję `xp_cmdshell`)

---

## WSTG-INPV-06 — Testowanie pod kątem LDAP Injection

LDAP Injection występuje, gdy aplikacja włącza dane dostarczone przez użytkownika do filtrów LDAP (Lightweight Directory Access Protocol) bez odpowiedniej sanityzacji lub escapingu, co pozwala atakującemu na manipulowanie logiką zapytania. Poprzez wstrzykiwanie znaków specjalnych, takich jak gwiazdki, nawiasy i operatory logiczne, atakujący mogą modyfikować filtr wyszukiwania, aby ominąć mechanizmy uwierzytelniania lub wyeksfiltrować wrażliwe informacje z katalogu, w tym nazwy użytkowników, przynależność do grup i atrybuty organizacyjne. Podatność ta zazwyczaj objawia się w funkcjach takich jak wyszukiwarki katalogów firmowych, portale pracownicze lub systemy Single Sign-On (SSO) współpracujące z Active Directory lub OpenLDAP. Z perspektywy atakującego, udana eksploatacja często obejmuje techniki typu blind w celu enumeracji struktury katalogu lub wartości atrybutów bit po bicie, gdy bezpośredni wynik zapytania jest tłumiony przez aplikację.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Narzędzia:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### Czy aplikacja wykorzystuje LDAP do uwierzytelniania lub przeszukiwania katalogów?
- [ ] Nie — LDAP **nie jest używany** w architekturze aplikacji  
- [ ] Tak — LDAP jest używany, a wszystkie dane wejściowe są rygorystycznie sanityzowane lub parametryzowane  
- [ ] Tak — LDAP jest używany, a dane użytkownika są łączone z filtrami **bez** odpowiedniego escapingu  

### Czy metaznaki LDAP są odpowiednio escapowane lub filtrowane w danych wejściowych użytkownika?
- [ ] Tak — znaki takie jak `(`, `)`, `&`, `|`, `*` oraz `\` są **niedozwolone** lub są poprawnie escapowane  
- [ ] Nie — metaznaki **mogą** zostać wstrzyknięte w celu zmiany struktury filtra LDAP  

### Czy mechanizm uwierzytelniania może zostać pominięty poprzez wstrzyknięcie (injection)?
- [ ] Nie — logika logowania **nie jest podatna** na manipulację zapytaniami  
- [ ] Tak — uwierzytelnianie **może** zostać pominięte przy użyciu logicznego OR (`|`) lub wstrzyknięcia symbolu wieloznacznego (`*`) w polach nazwy użytkownika lub hasła  

### Czy możliwa jest ślepa eksfiltracja danych (blind data exfiltration) z usługi katalogowej?
- [ ] Nie — wyniki wyszukiwania są ograniczone i nie zaobserwowano różnic czasowych ani logicznych (boolean)  
- [ ] Tak — atrybuty katalogu **mogą** być enumerowane bit po bicie za pomocą technik boolean-based blind injection  
- [ ] Tak — pełne rekordy katalogu **są** odzwierciedlone w odpowiedzi aplikacji z powodu manipulacji zapytaniem  

### Czy drugorzędne komponenty aplikacji (np. moduły pocztowe, SSO) są podatne na wstrzykiwanie atrybutów LDAP?
- [ ] Nie — atrybuty LDAP są bezpiecznie obsługiwane we wszystkich komponentach  
- [ ] Tak — atakujący **może** zmodyfikować własne atrybuty (np. e-mail, przynależność do grupy) poprzez wstrzyknięcie kodu, aby podnieść uprawnienia lub przekierować komunikację

---

## WSTG-INPV-07 — XML Injection

XML Injection (Wstrzykiwanie XML) występuje, gdy aplikacja włącza dane dostarczone przez użytkownika do dokumentu lub strumienia XML bez odpowiedniej walidacji, sanitizacji lub escapowania znaków specjalnych XML. Poprzez wstrzykiwanie określonych tagów XML lub elementów strukturalnych, atakujący może manipulować logiką dokumentu, omijać mechanizmy uwierzytelniania lub naruszać integralność danych przetwarzanych przez backend. Ta podatność jest powszechnie spotykana w usługach sieciowych opartych na protokole SOAP, API REST akceptujących XML, asercjach SAML oraz aplikacjach, które dynamicznie generują pliki konfiguracyjne XML lub raporty. Z perspektywy atakującego, celem jest wyjście poza zamierzony kontekst danych w celu zmiany struktury dokumentu, co potencjalnie prowadzi do nieautoryzowanej eskalacji uprawnień (privilege escalation) lub wykonania niezamierzonej logiki biznesowej.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Status testu** | Nieprzeprowadzony |
| **Krytyczność** | Wysoka / Średnia |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Narzędzia:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### Czy aplikacja akceptuje i przetwarza dane wejściowe w formacie XML od użytkownika?
- [ ] Nie — aplikacja **nie** przetwarza danych wejściowych XML  
- [ ] Tak — dane XML są przetwarzane przez API (SOAP/REST)  
- [ ] Tak — dane XML są przetwarzane poprzez przesyłanie plików (SVG, DOCX itp.)  

### Czy znaki specjalne XML (np. `<`, `>`, `&`, `'`, `"`) są odpowiednio escapowane lub sanitizowane?
- [ ] Tak — wszystkie znaki specjalne są **odpowiednio** escapowane lub sanitizowane *(Najbezpieczniejsze)*  
- [ ] Tak — stosowane jest filtrowanie, ale obejście **jest możliwe** przy użyciu kodowania  
- [ ] Nie — surowe dane wejściowe są osadzane bezpośrednio w strukturach XML **bez** sanitizacji  

### Czy walidacja schematu (XSD/DTD) po stronie serwera jest wymuszana dla wszystkich danych wejściowych XML?
- [ ] Tak — rygorystyczna walidacja schematu jest **włączona** i zapobiega zmianom strukturalnym  
- [ ] Tak — walidacja jest **włączona**, ale **nie** zapobiega wstrzykiwaniu tagów w polach danych  
- [ ] Nie — walidacja schematu jest **wyłączona** lub nie została zaimplementowana  

### Czy atakujący może wstrzyknąć nowe tagi XML w celu zmiany struktury dokumentu lub logiki?
- [ ] Nie — struktura pozostaje nienaruszona niezależnie od danych wejściowych  
- [ ] Tak — tagi mogą być wstrzykiwane, ale **nie mogą** zmieniać logiki biznesowej  
- [ ] Tak — wstrzykiwanie tagów **jest możliwe** i pozwala na obejście logiki lub modyfikację danych *(Krytyczne)*  

### Czy można manipulować parserem XML przy użyciu sekcji CDATA lub komentarzy XML?
- [ ] Nie — parser poprawnie obsługuje lub odrzuca znaczniki CDATA/komentarzy  
- [ ] Tak — wstrzyknięcie `<![CDATA[...]]>` lub `<!-- ... -->` **jest możliwe** w celu ukrycia danych lub obejścia filtrów

---

## WSTG-INPV-08 — Testowanie pod kątem SSI Injection

Server-Side Includes (SSI) Injection występuje, gdy aplikacja nie oczyszcza danych wejściowych użytkownika przed ich włączeniem do plików HTML przetwarzanych przez silnik SSI serwera. Atakujący wykorzystują tę podatność, wstrzykując dyrektywy SSI, takie jak `<!--#exec cmd="id" -->`, w celu wykonania dowolnych poleceń powłoki (shell), odczytu lokalnych plików lub eksfiltracji zmiennych środowiskowych. Podatność ta jest zazwyczaj spotykana w aplikacjach obsługujących starsze rozszerzenia plików, takie jak `.shtml`, `.shtm` lub `.stm`, lub w sytuacjach, gdy serwer WWW jest jawnie skonfigurowany do parsowania SSI w standardowych plikach HTML. Udana eksploatacja przyznaje atakującemu te same uprawnienia, które posiada proces serwera WWW, co często prowadzi do pełnego przejęcia systemu lub ujawnienia poufnych danych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Status testu** | Nie wykonano |
| **Dotkliwość** | Wysoka |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Czy serwer WWW obsługuje i przetwarza dyrektywy SSI?
- [ ] Nie — serwer **nie obsługuje** SSI lub rozszerzenia plików takie jak `.shtml` są **wyłączone**  
- [ ] Tak — SSI **jest włączone**, ale dane wejściowe użytkownika **nie są** odzwierciedlane w przetwarzanych stronach  
- [ ] Tak — SSI **jest włączone**, a dane wejściowe użytkownika **są** odzwierciedlane w przetwarzanych stronach  

### Czy można wykonywać dowolne polecenia systemowe za pomocą dyrektywy `#exec`?
- [ ] Nie — dyrektywa `#exec` jest **wyłączona** lub ograniczona przez konfigurację serwera  
- [ ] Tak — wykonywanie poleceń **jest możliwe** za pomocą Payloadów `#exec cmd` lub `#exec cgi`  

### Czy możliwa jest eksfiltracja poufnych plików lub zmiennych środowiskowych?
- [ ] Nie — dyrektywy takie jak `#include` lub `#config` są **wyłączone**  
- [ ] Tak — odczyt plików lokalnych (np. `/etc/passwd`) **jest możliwy** poprzez `#include virtual`  
- [ ] Tak — zmienne środowiskowe serwera **mogą** zostać wyeksfiltrowane za pomocą `#printenv` lub `#echo`  

### Czy walidacja danych wejściowych lub zabezpieczenia WAF są skuteczne przeciwko Payloadom SSI?
- [ ] Tak — stosowana **jest** rygorystyczna walidacja danych wejściowych i bypass **nie jest możliwy**  
- [ ] Tak — walidacja/WAF **jest stosowana**, ale bypass **jest możliwy** przy użyciu kodowania znaków lub innej składni SSI  
- [ ] Nie — brak walidacji lub ochrony WAF dla znaków SSI (`<`, `!`, `#`, `-`, `"`)

---

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

## WSTG-INPV-10 — IMAP SMTP Injection

Podatności typu IMAP and SMTP injection powstają, gdy aplikacja internetowa niewłaściwie filtruje dane dostarczane przez użytkownika przed ich włączeniem do poleceń przesyłanych do serwera pocztowego. Poprzez wstrzyknięcie znaków Carriage Return i Line Feed (CRLF), atakujący może zakończyć zamierzone polecenie i dołączyć dowolne instrukcje pocztowe, takie jak dodanie dodatkowych odbiorców (CC/BCC) lub modyfikacja treści wiadomości. Błędy te najczęściej występują w bramkach web-to-mail, formularzach kontaktowych oraz klientach pocztowych komunikujących się bezpośrednio z serwerami poczty za pomocą protokołów takich jak IMAP4 lub SMTP. Skuteczna eksploatacja pozwala na nieautoryzowany relay spamu, eksfiltrację poufnej zawartości e-maili, a w niektórych przypadkach na całkowite naruszenie integralności serwera pocztowego.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Status testu** | Niewykonany |
| **Severity** | Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Czy aplikacja przetwarza dane wejściowe dostarczone przez użytkownika w celu konstruowania poleceń IMAP lub SMTP?
- [ ] Nie — aplikacja **nie** wchodzi w interakcję z serwerami pocztowymi poprzez dane wejściowe użytkownika  
- [ ] Tak — aplikacja wchodzi w interakcję z serwerami pocztowymi, ale korzysta z bezpiecznego API lub biblioteki  
- [ ] Tak — aplikacja konstruuje surowe polecenia pocztowe przy użyciu parametrów kontrolowanych przez użytkownika  

### Czy dane wejściowe są walidowane w celu zapobiegania wstrzykiwaniu sekwencji Carriage Return (`\r`) i Line Feed (`\n`)?
- [ ] Tak — znaki CRLF są **ściśle** filtrowane, kodowane lub odrzucane  
- [ ] Tak — dane wejściowe są walidowane względem ścisłej białej listy (allowlist) znaków  
- [ ] Nie — znaki CRLF **nie** są filtrowane, co pozwala na zakończenie polecenia i wstrzyknięcie (injection)  

### Czy atakujący może wstrzyknąć dodatkowe nagłówki wiadomości, takie jak `Bcc:` lub `Cc:`?
- [ ] Nie — modyfikacja nagłówków **nie jest możliwa** ze względu na ścisłe ograniczenia danych wejściowych  
- [ ] Tak — wstrzyknięcie dodatkowych nagłówków **jest możliwe** poprzez sekwencje CRLF  
- [ ] Tak — relay poczty do domen zewnętrznych **jest włączony** i możliwy do wykorzystania *(Krytyczne)*  

### Czy aplikacja pozwala na manipulację poleceniami IMAP w celu uzyskania dostępu do nieautoryzowanych skrzynek pocztowych?
- [ ] Nie — aplikacja **nie** korzysta z IMAP lub ściśle ogranicza dostęp do skrzynki dla uwierzytelnionego użytkownika  
- [ ] Tak — IMAP command injection **jest możliwe**, ale ograniczone do enumeracji metadanych  
- [ ] Tak — IMAP command injection pozwala na odczyt lub usunięcie wiadomości e-mail innych użytkowników  

### Czy serwer pocztowy backendu jest podatny na command pipelining?
- [ ] Nie — serwer pocztowy odrzuca polecenia wsadowe lub wiele poleceń w jednej sesji  
- [ ] Tak — serwer pocztowy przetwarza wiele poleceń wstrzykniętych za pośrednictwem pojedynczego pola wejściowego

---

## WSTG-INPV-11 — Code Injection

Code injection (wstrzykiwanie kodu) występuje, gdy aplikacja ewaluuje fragmenty kodu dostarczone przez użytkownika w swoim środowisku wykonawczym (runtime environment), zazwyczaj poprzez niebezpieczne funkcje specyficzne dla języka, takie jak `eval()`, `exec()` lub `system()`. Podatność ta różni się od command injection, ponieważ celuje w kontekst wykonawczy języka programowania (np. PHP, Python, Node.js), a nie w powłokę bazowego systemu operacyjnego. Atakujący wykorzystują te punkty do wykonywania dowolnej logiki, eksfiltracji zmiennych środowiskowych lub uzyskania pełnego zdalnego wykonania kodu (Remote Code Execution (RCE)) na serwerze. Pentesterzy powinni zbadać funkcjonalności przetwarzające wyrażenia matematyczne, szablony po stronie serwera (server-side templates) lub dynamiczne parametry konfiguracyjne, w których dane wejściowe mogą być interpretowane jako kod wykonywalny.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom dotkliwości** | Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Czy jakiekolwiek funkcje aplikacji ewaluują dynamiczne dane wejściowe za pomocą funkcji ewaluacji specyficznych dla języka?
- [ ] Nie — funkcje dynamicznej ewaluacji **nie występują** w kodzie źródłowym  
- [ ] Tak — funkcje takie jak `eval()`, `exec()` lub `include()` są używane, ale **nie przyjmują** danych wejściowych od użytkownika  
- [ ] Tak — funkcje są używane i przetwarzają dane dostarczone przez użytkownika  

### Czy przed ewaluacją stosowane są mechanizmy walidacji lub sanityzacji danych wejściowych?
- [ ] Tak — dane wejściowe są ściśle walidowane względem **sztywnej białej listy (whitelist)**  
- [ ] Tak — dane wejściowe są sanityzowane poprzez czarną listę (blacklisting), ale obejście (bypass) **jest możliwe**  
- [ ] Nie — dane wejściowe są przekazywane bezpośrednio do funkcji ewaluacji i **nie są stosowane żadne mechanizmy kontrolne**  

### Czy środowisko wykonawcze jest odizolowane (sandboxed) lub ograniczone w celu uniemożliwienia dostępu na poziomie systemu?
- [ ] Tak — wykonanie odbywa się w wysoce ograniczonej piaskownicy (sandbox), w której wywołania systemowe **nie mogą** być wykonywane  
- [ ] Tak — istnieją pewne ograniczenia, ale ucieczka z piaskownicy (sandbox escape) **jest możliwa**  
- [ ] Nie — środowisko pozwala na pełny dostęp do API języka i funkcji systemu operacyjnego *(Krytyczny)*  

### Czy Code Injection może zostać wykorzystane do eksfiltracji wrażliwych informacji?
- [ ] Nie — wykonanie jest typu "blind" i żadna komunikacja poza pasmem (out-of-band) **nie jest możliwa**  
- [ ] Tak — zmienne środowiskowe lub pliki lokalne **mogą** zostać wyeksfiltrowane za pomocą żądań HTTP/DNS  
- [ ] Tak — wyniki wykonanego kodu są **bezpośrednio odzwierciedlone** w odpowiedzi aplikacji  

### Czy aplikacja pozwala na zdalne dołączanie plików (Remote File Inclusion) lub dynamiczne ładowanie bibliotek?
- [ ] Nie — mogą być wykonywane tylko lokalne, predefiniowane ścieżki kodu  
- [ ] Tak — aplikacja **może** zostać zmuszona do załadowania i wykonania kodu ze zdalnego lub kontrolowanego przez atakującego źródła

---

## WSTG-INPV-12 — Command Injection

Command Injection (wstrzykiwanie poleceń) występuje, gdy aplikacja przekazuje niebezpieczne dane dostarczone przez użytkownika, takie jak dane z formularzy, nagłówki HTTP lub pliki cookie, do powłoki systemowej (shell). Ta podatność pozwala atakującemu na wykonywanie dowolnych poleceń systemu operacyjnego na serwerze, zazwyczaj z uprawnieniami procesu aplikacji webowej. Z perspektywy atakującego, podatność ta jest wykorzystywana poprzez użycie metaznaków powłoki, takich jak średniki, potoki (pipes) lub backticki, w celu dołączenia złośliwych poleceń do zamierzonego przepływu wykonywania. Skutki są często katastrofalne i prowadzą do pełnego przejęcia systemu, nieautoryzowanej eksfiltracji danych lub ruchu bocznego (lateral movement) wewnątrz sieci wewnętrznej.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Status testu** | Nie wykonano |
| **Krytyczność** | Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Narzędzia:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### Czy aplikacja wywołuje polecenia na poziomie systemu lub pliki wykonywalne powłoki?
- [ ] Nie — aplikacja **nie** wchodzi w interakcję z powłoką systemu operacyjnego  
- [ ] Tak — aplikacja używa wewnętrznych API lub bibliotek wysokiego poziomu do zadań systemowych  
- [ ] Tak — aplikacja bezpośrednio wywołuje polecenia powłoki poprzez wywołania systemowe, takie jak `system()`, `exec()` lub `popen()`  

### Czy dane wejściowe użytkownika są walidowane lub sanityzowane przed przekazaniem do wywołań systemowych?
- [ ] Tak — stosowana jest ścisła biała lista (allow-listing) i walidacja danych wejściowych  
- [ ] Tak — stosowana jest sanityzacja lub filtrowanie znaków (escaping), ale możliwe jest obejście zabezpieczeń (bypass)  
- [ ] Nie — dane wejściowe użytkownika są przekazywane bezpośrednio do powłoki **bez** walidacji  

### Czy metaznaki powłoki mogą zostać użyte do manipulacji przepływem wykonywania poleceń?
- [ ] Nie — metaznaki są poprawnie filtrowane lub eskapowane  
- [ ] Tak — możliwe jest wykonanie typu in-band, gdzie wynik polecenia jest zwracany w odpowiedzi  
- [ ] Tak — możliwe jest wykonanie typu blind, gdzie wynik nie jest zwracany w odpowiedzi  

### Czy możliwa jest eksfiltracja danych poza pasmem (OOB) z hosta?
- [ ] Nie — serwer nie posiada połączeń wychodzących (egress) lub sygnały OOB (DNS/HTTP) zawodzą  
- [ ] Tak — eksfiltracja danych poprzez sygnały OOB oparte na DNS lub HTTP **jest możliwa**  

### Czy proces aplikacji działa z podwyższonymi uprawnieniami systemowymi?
- [ ] Nie — aplikacja działa na dedykowanym koncie usługowym o niskich uprawnieniach  
- [ ] Tak — aplikacja działa jako `root`, `Administrator` lub inny użytkownik o wysokich uprawnieniach *(Krytyczne)*

---

## WSTG-INPV-13 — Format String Injection

Wstrzyknięcie łańcucha formatującego (Format String Injection) występuje, gdy aplikacja webowa przekazuje dane wejściowe kontrolowane przez użytkownika bezpośrednio do argumentu będącego łańcuchem formatującym funkcji o zmiennej liczbie argumentów, takiej jak rodzina funkcji `printf` w języku C lub niskopoziomowe wrappery logowania. Chociaż podatność ta jest rzadziej spotykana w nowoczesnych frameworkach webowych opartych na kodzie zarządzanym (managed code), pozostaje ona krytyczna w starszych aplikacjach CGI, natywnych rozszerzeniach lub specyficznych systemach logowania wchodzących w interakcję z niskopoziomowymi bibliotekami systemowymi. Atakujący wykorzystują tę lukę, dostarczając specyfikatory formatu, takie jak `%x` lub `%p`, aby ujawnić wrażliwe adresy pamięci i dane ze stosu, lub specyfikator `%n` w celu wykonania dowolnego zapisu w pamięci. Skuteczna eksploatacja zazwyczaj skutkuje całkowitym przejęciem systemu poprzez zdalne wykonanie kodu (Remote Code Execution - RCE) lub, co najmniej, znaczącym stanem odmowy usługi (Denial-of-Service - DoS) poprzez wymuszenie awarii procesów.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Status testu** | Nie przeprowadzono |
| **Poziom ważności** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Czy aplikacja przetwarza dane wejściowe użytkownika poprzez kod natywny lub starsze interfejsy CGI?
- [ ] Nie — aplikacja została zbudowana całkowicie w oparciu o frameworki kodu zarządzanego (np. JVM, CLR)  
- [ ] Tak — aplikacja korzysta z JNI, natywnych rozszerzeń C/C++ lub starszych binarnych skryptów CGI, gdzie podatności typu format string **mogą** istnieć  

### Czy dane wejściowe użytkownika są przekazywane jako główny argument do funkcji obsługujących formatowanie?
- [ ] Nie — dane wejściowe są przekazywane jako argumenty danych, a łańcuchy formatujące są **statyczne** i **zakodowane na sztywno (hardcoded)**  
- [ ] Tak — dane wejściowe użytkownika są bezpośrednio łączone z argumentem łańcucha formatującego, ale **zastosowano** sanityzację  
- [ ] Tak — dane wejściowe użytkownika są bezpośrednio używane jako łańcuch formatujący i **nie zastosowano** żadnej sanityzacji  

### Czy możliwe jest wycieknięcie zawartości pamięci przy użyciu specyfikatorów formatu?
- [ ] Nie — dane wejściowe są walidowane, a specyfikatory takie jak `%p`, `%x` lub `%s` **nie są przetwarzane**  
- [ ] Tak — podanie specyfikatorów `%p` lub `%x` skutkuje odzwierciedleniem adresów pamięci stosu lub sterty  
- [ ] Tak — wrażliwe dane (np. wskaźniki, stack cookies) **mogą** zostać wytransferowane za pomocą specyfikatorów formatu  

### Czy można spowodować awarię aplikacji lub manipulować nią za pomocą specyfikatora `%n`?
- [ ] Nie — specyfikatory umożliwiające zapis w pamięci są **wyłączone** lub blokowane przez kompilator/środowisko  
- [ ] Tak — aplikacja ulega awarii (DoS), gdy podane zostaną ciągi `%s%s%s%s` lub podobne  
- [ ] Tak — dowolny zapis w pamięci poprzez `%n` jest **możliwy**, co potencjalnie prowadzi do zdalnego wykonania kodu (Remote Code Execution - RCE)  

### Czy nowoczesne zabezpieczenia binarne (ASLR, DEP, Stack Canaries) są możliwe do obejścia poprzez tę iniekcję?
- [ ] Nie — środowisko jest utwardzone (hardened), a wyciek pamięci **nie może** zostać wykorzystany do obejścia zabezpieczeń  
- [ ] Tak — Format String Injection **może** zostać użyte do wycieku adresów bazowych, co sprawia, że obejście ASLR/DEP jest **możliwe**

---

## WSTG-INPV-14 — Testowanie pod kątem podatności inkubowanych (Incubated Vulnerabilities)

Podatności inkubowane (Incubated Vulnerabilities), określane również jako podatności trwałe (persistent) lub drugiego rzędu (second-order), występują, gdy złośliwy ładunek (Payload) jest przechowywany przez aplikację w stanie „uśpionym”, a następnie wykonywany lub przetwarzany w innym kontekście. Ten wzorzec ataku zazwyczaj celuje w systemy back-endowe, konsole administracyjne lub zautomatyzowane narzędzia raportujące, które pobierają dane ze współdzielonej bazy danych lub systemu plików bez odpowiedniej ponownej walidacji lub kodowania wyjściowego (output encoding). Pentesterzy muszą śledzić cykl życia danych wejściowych użytkownika od punktu wejścia („inkubatora”) do każdego miejsca, w którym te dane są ostatecznie renderowane lub wykorzystywane przez inne komponenty lub aplikacje pomocnicze. Skuteczna eksploatacja (Exploit) często prowadzi do skutków o wysokim priorytecie, takich jak przejęcie sesji administracyjnej (administrative session hijacking) poprzez Stored XSS lub eksfiltracja danych za pomocą SQL Injection drugiego rzędu w wewnętrznych modułach raportowych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Status testu** | Nieprzeprowadzono |
| **Severity** | Wysoka |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Narzędzia:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Czy istnieją punkty końcowe (endpoints), w których dane wejściowe kontrolowane przez użytkownika są przechowywane w celu późniejszego pobrania przez innych użytkowników lub procesy?
- [ ] Nie — aplikacja **nie** przechowuje danych wejściowych użytkownika do późniejszego wykorzystania  
- [ ] Tak — dane wejściowe **są** przechowywane w polach bazy danych, plikach dziennika (log files) lub ustawieniach konfiguracji  

### Czy walidacja danych wejściowych (input validation) lub sanityzacja (sanitization) jest stosowana przed zapisaniem danych do warstwy trwałego przechowywania?
- [ ] Tak — ścisła walidacja oparta na białej liście (allow-list) **jest stosowana** w punkcie wejścia  
- [ ] Tak — ogólna sanityzacja **jest stosowana**, ale obejście (bypass) **jest możliwe** poprzez kodowanie lub niestandardowe znaki  
- [ ] Nie — dane wejściowe są przechowywane w ich surowej, niewalidowanej formie  

### Czy przechowywane dane są odpowiednio kodowane lub sanityzowane, gdy są renderowane w interfejsach administracyjnych lub pomocniczych?
- [ ] Tak — kodowanie wyjściowe (output encoding) uwzględniające kontekst **jest stosowane** wszędzie tam, gdzie dane są renderowane  
- [ ] Tak — kodowanie **jest stosowane** w niektórych miejscach, ale **brakuje** go w innych (np. wewnętrzny panel administracyjny)  
- [ ] Nie — dane są renderowane bez żadnego kodowania ani sanityzacji, co pozwala na wykonanie ładunku (Payload)  

### Czy ładunki (Payloads) przechowywane w bazie danych mogą wpływać na procesy wsadowe (batch processes) back-endu lub systemy monitorowania typu out-of-band (OOB)?
- [ ] Nie — procesy back-endowe używają bezpiecznych metod parsowania lub zapytań parametryzowanych  
- [ ] Tak — przechowywane ładunki **mogą** wywoływać interakcje w logice back-endu (np. CSV injection, command injection w logach)  
- [ ] Tak — przechowywane ładunki **mogą** wywoływać żądania out-of-band (OOB) do serwerów kontrolowanych przez atakującego

---

## WSTG-INPV-15 — HTTP Splitting Smuggling

Podatności typu HTTP Splitting i Smuggling wynikają z rozbieżności w sposobie, w jaki serwery proxy (frontend) i serwery zaplecza (backend) interpretują i przetwarzają granice żądań HTTP, w szczególności w odniesieniu do nagłówków `Content-Length` oraz `Transfer-Encoding`. Poprzez konstruowanie niejednoznacznych żądań, atakujący może „przemycić” (smuggle) ukryte żądanie do backendu lub wstrzyknąć sekwencje CRLF w celu rozszczepienia odpowiedzi, co prowadzi do zatrucia pamięci podręcznej (cache poisoning), przejęcia żądania (request hijacking) lub obejścia mechanizmów kontroli bezpieczeństwa. Wady te zazwyczaj objawiają się w złożonych środowiskach wykorzystujących reverse proxy, load balancery lub sieci CDN, które posiadają niespójną logikę parsowania. Z perspektywy atakującego pozwala to na przekierowanie ruchu użytkowników, kradzież poufnych tokenów sesyjnych oraz wykonywanie nieautoryzowanych działań w kontekście sesji innych użytkowników.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Narzędzia:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Czy środowisko jest podatne na przemycanie żądań (request smuggling) poprzez rozbieżności CL.TE lub TE.CL?
- [ ] Nie — serwery frontendowe i backendowe spójnie obsługują granice żądań  
- [ ] Tak — istnieją rozbieżności, ale eksploatacja **nie jest możliwa** ze względu na zabezpieczenia infrastrukturalne  
- [ ] Tak — przemycanie żądań CL.TE lub TE.CL **jest możliwe**, co pozwala ukrytym żądaniom dotrzeć do backendu *(Wysoki)*  
- [ ] Tak — TE.TE (podwójne kodowanie/obfuskacja) **jest możliwe** w celu obejścia filtrów frontendu *(Krytyczny)*  

### Czy możliwe jest uzyskanie rozszczepienia odpowiedzi HTTP (HTTP Response Splitting) poprzez wstrzyknięcie CRLF w nagłówkach?
- [ ] Nie — aplikacja poprawnie oczyszcza sekwencje CRLF we wszystkich danych wejściowych nagłówków  
- [ ] Tak — sekwencje CRLF są odzwierciedlane w nagłówkach, ale rozszczepienie odpowiedzi **nie jest możliwe**  
- [ ] Tak — wstrzyknięcie CRLF (**CRLF injection**) **jest możliwe**, co pozwala na wstrzyknięcie nagłówka lub zatrucie pamięci podręcznej (cache poisoning)  

### Czy mechanizmy kontroli bezpieczeństwa (WAF/ACL) są obchodzone przy użyciu przemyconych żądań?
- [ ] Nie — mechanizmy kontroli bezpieczeństwa mają zastosowanie zarówno do żądań zewnętrznych, jak i przemyconych  
- [ ] Tak — przemycone żądania **mogą** obejść reguły WAF frontendu lub listy kontroli dostępu (ACL) oparte na adresach IP  

### Czy możliwe jest przejęcie sesji innych użytkowników lub przekierowanie ich ruchu?
- [ ] Nie — strumienie żądań/odpowiedzi są odizolowane i nie mogą zostać skrzyżowane  
- [ ] Tak — przemycanie żądań (**request smuggling**) **jest możliwe** i pozwala na przechwytywanie żądań innych użytkowników *(Krytyczny)*  
- [ ] Tak — rozszczepienie odpowiedzi (**response splitting**) **jest możliwe** i pozwala na zatrucie pamięci podręcznej po stronie przeglądarki lub XSS

---

## WSTG-INPV-16 — HTTP Request Smuggling

Ataki typu HTTP Request Smuggling (HRS) występują, gdy łańcuch serwerów HTTP (np. load balancer i serwer webowy back-end) w różny sposób interpretuje granice żądań, zazwyczaj z powodu konfliktów między nagłówkami `Content-Length` i `Transfer-Encoding`. Atakujący wykorzystuje tę desynchronizację do „przemycenia” (smuggle) wpisu do bufora żądań serwera back-end, skutecznie poprzedzając żądanie kolejnego legalnego użytkownika fragmentem kontrolowanym przez atakującego. Technika ta pozwala na przeprowadzenie poważnych ataków, w tym przejmowanie sesji (Session Hijacking), omijanie mechanizmów kontroli bezpieczeństwa oraz zatruwanie pamięci podręcznej (Cache Poisoning). Eksploatacja jest zazwyczaj wykonywana poprzez analizę opartą na czasie (timing-based analysis) lub poprzez obserwację nieoczekiwanych odpowiedzi z back-endu po wysłaniu kolejnych żądań do serwera.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom krytyczności** | Krytyczny / Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Narzędzia:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Czy serwery front-end i back-end obsługują nagłówek `Transfer-Encoding: chunked` w sposób spójny?
- [ ] Tak — oba serwery obsługują kodowanie chunked identycznie i desynchronizacja **nie jest możliwa**  
- [ ] Nie — serwery interpretują nagłówki w różny sposób, ale stosowana jest **sanityzacja/normalizacja**  
- [ ] Nie — desynchronizacja CL.TE (Content-Length/Transfer-Encoding) **jest możliwa** *(Krytyczny)*  
- [ ] Nie — desynchronizacja TE.CL (Transfer-Encoding/Content-Length) **jest możliwa** *(Krytyczny)*  

### Czy atakujący może zatruć pamięć podręczną serwera lub klienta za pomocą przemyconego żądania?
- [ ] Nie — buforowanie (caching) jest **wyłączone** lub niepodatne na zatruwanie  
- [ ] Tak — przemycone żądania **mogą** przekierowywać użytkowników do złośliwych domen lub wstrzykiwać skrypty poprzez zatruwanie pamięci podręcznej (Cache Poisoning)  

### Czy możliwe jest przechwycenie żądań innych użytkowników lub tokenów sesyjnych poprzez konkatenację żądań?
- [ ] Nie — smuggling **nie pozwala** na ujawnienie danych innych użytkowników  
- [ ] Tak — smuggling pozwala na dołączenie żądania kolejnego użytkownika do kontrolowanego przez atakującego body żądania `POST`, co czyni eksfiltrację danych **możliwą**  

### Czy infrastruktura korzysta z HTTP/2 i czy jest podatna na desynchronizację H2.CL lub H2.TE?
- [ ] Nie — aplikacja korzysta wyłącznie z HTTP/1.1 lub protokół HTTP/2 jest zaimplementowany bezpiecznie bez obniżania wersji (downgrading)  
- [ ] Tak — występuje obniżenie wersji z HTTP/2 do HTTP/1.1 (cleartext downgrade) i desynchronizacja **jest możliwa**  

### Czy nieprawidłowo sformułowane nagłówki (np. `Transfer-Encoding:  chunked` ze spacją) są obsługiwane w bezpieczny sposób?
- [ ] Tak — nieprawidłowe nagłówki są odrzucane lub normalizowane na wszystkich poziomach infrastruktury  
- [ ] Nie — desynchronizacja TE.TE **jest możliwa** z powodu niespójnej obsługi zniekształconych lub ukrytych nagłówków

---

## WSTG-INPV-17 — Testowanie pod kątem Host Header Injection

Host Header Injection występuje, gdy aplikacja ufa dostarczonemu przez użytkownika nagłówkowi `Host` i włącza go do logiki po stronie serwera, takiej jak generowanie linków lub przekierowania, bez wystarczającej walidacji. Atakujący manipulują tym nagłówkiem, aby ułatwić przeprowadzenie Web Cache Poisoning, zatrucie mechanizmu resetowania hasła (Password Reset Poisoning) poprzez przekierowanie wrażliwych tokenów do zewnętrznej domeny lub obejście mechanizmów kontroli bezpieczeństwa opartych na routingu nagłówkowym. Skuteczna eksploatacja pozwala atakującemu na eksfiltrację danych sesyjnych, przejęcie konta (Account Takeover) lub przekierowanie niczego niepodejrzewających użytkowników do złośliwej infrastruktury. Podatność ta jest najczęściej spotykana w środowiskach z systemami równoważenia obciążenia (Load Balancing) lub w aplikacjach, które dynamicznie generują bezwzględne adresy URL na podstawie kontekstu przychodzącego żądania.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się Wysoki, jeśli nagłówek Host jest używany do generowania wrażliwych linków w wiadomościach e-mail, takich jak linki do resetowania hasła.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Narzędzia:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### Czy aplikacja odzwierciedla wartość nagłówka `Host` w linkach, przekierowaniach lub skryptach?
- [ ] Nie — aplikacja używa sztywno zakodowanych (hardcoded) bazowych adresów URL lub rygorystycznie walidowanej konfiguracji  
- [ ] Tak — nagłówek `Host` jest odzwierciedlany, ale rygorystycznie walidowany względem białej listy (whitelist)  
- [ ] Tak — nagłówek `Host` jest odzwierciedlany, a obejście (bypass) jest możliwe poprzez nieprawidłowo sformatowane wartości (np. `expected.com:attacker.com`)  
- [ ] Tak — nagłówek `Host` jest odzwierciedlany **bez** żadnej walidacji  

### Czy alternatywne nagłówki związane z hostem są przetwarzane przez aplikację lub infrastrukturę?
- [ ] Nie — nagłówki takie jak `X-Forwarded-Host`, `X-Host` lub `Forwarded` są ignorowane  
- [ ] Tak — alternatywne nagłówki są przetwarzane, ale **nie** nadpisują logiki wewnętrznej  
- [ ] Tak — alternatywne nagłówki **mogą** być użyte do nadpisania podstawowej wartości nagłówka `Host`  

### Czy nagłówek `Host` może zostać zmanipulowany w celu zatrucia wiadomości e-mail generowanych przez system (np. resetowania hasła)?
- [ ] Nie — linki w wiadomościach e-mail używają statycznej, zaufanej domeny z konfiguracji serwera  
- [ ] Tak — nagłówek `Host` wpływa na linki w e-mailach, ale walidacji **nie da się** obejść  
- [ ] Tak — nagłówek `Host` **może** zostać zmanipulowany w celu przekierowania tokenów resetowania hasła do domeny kontrolowanej przez atakującego *(Krytyczne)*  

### Czy aplikacja jest podatna na Web Cache Poisoning poprzez nagłówek `Host`?
- [ ] Nie — brak mechanizmu buforowania (caching) lub nagłówek `Host` jest częścią klucza pamięci podręcznej (cache key)  
- [ ] Tak — pamięć podręczna istnieje, ale rygorystycznie waliduje lub ignoruje nagłówek `Host` dla wejść niebędących kluczem (unkeyed inputs)  
- [ ] Tak — pamięć podręczna istnieje i **może** zostać zatruta przez wstrzyknięty nagłówek `Host`, aby serwować złośliwą treść innym użytkownikom  

### Czy serwer akceptuje dowolne nagłówki `Host` dla routingu wirtualnych hostów (virtual host routing)?
- [ ] Nie — serwer zwraca błąd lub stronę domyślną dla nierozpoznanych nagłówków `Host`  
- [ ] Tak — serwer akceptuje dowolny nagłówek `Host`, co **jest** użyteczne dla rekonesansu lub enumeracji usług wewnętrznych

---

## WSTG-INPV-18 — Testowanie pod kątem Server-Side Template Injection

Server-Side Template Injection (SSTI) występuje, gdy aplikacja osadza dane wejściowe kontrolowane przez użytkownika w silniku szablonów po stronie serwera bez odpowiedniej sanityzacji lub maskowania (escaping). Pozwala to atakującemu na wstrzyknięcie złośliwych dyrektyw szablonów, które są następnie wykonywane na serwerze, co potencjalnie prowadzi do pełnego przejęcia systemu poprzez zdalne wykonywanie kodu (RCE). SSTI zazwyczaj objawia się w funkcjach takich jak dynamiczne generowanie wiadomości e-mail, strony profilowe oraz systemy powiadomień, które korzystają z silników takich jak Jinja2, Twig lub FreeMarker. Z perspektywy atakującego, podatność ta jest często wykorzystywana do wycieku wrażliwych zmiennych środowiskowych, odczytu plików wewnętrznych lub wykonywania poleceń systemowych poprzez wykorzystanie natywnych obiektów i metod silnika szablonów.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Status testu** | Nieprzeprowadzony |
| **Krytyczność** | Krytyczny |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Narzędzia:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### Czy aplikacja wykorzystuje silnik szablonów po stronie serwera do przetwarzania danych wejściowych użytkownika?
- [ ] Nie — aplikacja **nie** używa szablonów po stronie serwera dla treści dynamicznych  
- [ ] Tak — szablony są używane, ale dane użytkownika **nie** są osadzane w dyrektywach szablonów  
- [ ] Tak — dane użytkownika są osadzane w szablonach **bez** odpowiedniej sanityzacji  

### Czy podstawowe wyrażenia arytmetyczne są obliczane po wstrzyknięciu do pól wejściowych?
- [ ] Nie — ładunki (payloads) takie jak `{{7*7}}` lub `${7*7}` są odzwierciedlane jako dosłowne ciągi znaków  
- [ ] Tak — wyrażenia są obliczane (np. zwracany jest wynik `49`), co potwierdza, że silnik szablonów **jest podatny**  

### Czy istnieje możliwość uzyskania dostępu do wbudowanych obiektów lub metod silnika szablonów?
- [ ] Nie — dostęp do niebezpiecznych obiektów, metod lub konfiguracji jest **ograniczony** lub odizolowany (sandboxed)  
- [ ] Tak — wbudowane obiekty (np. `self`, `config`, `__class__`) **mogą** być dostępne w celu wycieku wrażliwych danych  
- [ ] Tak — zdalne wykonywanie kodu (RCE) poprzez wywołania systemowe lub biblioteki OS **jest możliwe**  

### Czy wdrożono mechanizm piaskownicy (sandbox) lub białej listy (allowlist) zapobiegający wykonywaniu niebezpiecznych dyrektyw?
- [ ] Tak — ścisła biała lista lub bezpieczna piaskownica jest wdrożona i jej obejście **nie jest możliwe**  
- [ ] Tak — piaskownica istnieje, ale jej obejście **jest możliwe** za pomocą specyficznych ładunków zależnych od silnika  
- [ ] Nie — **nie zastosowano** żadnej piaskownicy ani białej listy w silniku szablonów

---

## WSTG-INPV-19 — Testowanie podatności na Server-Side Request Forgery (SSRF)

Podatność Server-Side Request Forgery (SSRF) występuje, gdy atakujący może wpłynąć na aplikację webową, aby ta wykonywała nieautoryzowane żądania z serwera do zasobów wewnętrznych lub zewnętrznych. Poprzez manipulację parametrami oczekującymi adresów URL, nazw hostów lub adresów IP, atakujący może obejść mechanizmy kontroli na poziomie sieci, takie jak zapory sieciowe (firewalle) i listy ACL, aby skanować segmenty sieci wewnętrznej, uzyskiwać dostęp do usług metadanych w chmurze (IMDS) lub wchodzić w interakcję z podatnymi wewnętrznymi interfejsami API i bazami danych. Ta podatność zazwyczaj objawia się w funkcjonalnościach obejmujących pobieranie obrazów, generowanie plików PDF, webhooks lub przesyłanie plików za pomocą adresu URL. Z perspektywy atakującego, SSRF jest potężnym prymitywem wykorzystywanym do przemieszczania się w głąb izolowanych środowisk (pivot), eksfiltracji poufnych danych konfiguracyjnych lub potencjalnego uzyskania zdalnego wykonania kodu (Remote Code Execution, RCE) poprzez interakcję z usługami wewnętrznymi, takimi jak Redis lub Memcached.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Status testu** | Nie wykonano |
| **Krytyczność** | Wysoka / Krytyczna |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Narzędzia:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### Czy aplikacja przetwarza adresy URL lub nazwy hostów dostarczone przez użytkownika?
- [ ] Nie — żadna funkcjonalność nie przyjmuje zewnętrznych adresów URL ani nazw hostów  
- [ ] Tak — funkcjonalność istnieje, ale dane wejściowe **są rygorystycznie walidowane** względem listy dozwolonych (allow-list)  
- [ ] Tak — funkcjonalność istnieje i przyjmuje adresy URL dostarczone przez użytkownika  

### Czy stosowane są mechanizmy walidacji lub czarne listy (deny-lists) dla żądanego celu?
- [ ] Tak — wymuszana jest rygorystyczna **lista dozwolonych** (allow-list) zaufanych domen/adresów IP  
- [ ] Tak — stosowana jest **lista blokowanych** (deny-list) (np. blokowanie `127.0.0.1`, `169.254.169.254`)  
- [ ] Nie — do danych wejściowych **nie jest stosowana żadna walidacja** ani filtrowanie  

### Czy możliwe jest obejście filtrów poprzez kodowanie, przekierowania lub DNS rebinding?
- [ ] Nie — filtry są solidne i bezpiecznie obsługują przekierowania oraz rozwiązywanie nazw DNS  
- [ ] Tak — obejście **jest możliwe** przy użyciu kodowania URL lub alternatywnych formatów adresów IP (hex, octal)  
- [ ] Tak — obejście **jest możliwe** poprzez przekierowania HTTP 3xx do celów wewnętrznych  
- [ ] Tak — obejście **jest możliwe** poprzez ataki DNS rebinding w celu rozwiązania nazw na wewnętrzne adresy IP  

### Czy aplikacja może połączyć się z wewnętrznymi usługami metadanych lub interfejsami lokalnymi?
- [ ] Nie — sieć lokalna i punkty końcowe metadanych w chmurze są **nieosiągalne**  
- [ ] Tak — dostęp do localhost (`127.0.0.1`) lub usług pętli zwrotnej (loopback) **jest możliwy**  
- [ ] Tak — dostęp do usług metadanych w chmurze (np. AWS/GCP/Azure IMDS) **jest możliwy** *(Krytyczne)*  

### Jaki jest charakter odpowiedzi SSRF?
- [ ] Blind — użytkownik nie otrzymuje żadnej odpowiedzi ani metadanych  
- [ ] Partial — metadane odpowiedzi (nagłówki, rozmiar, czas trwania) są zwracane użytkownikowi  
- [ ] Full — pełna treść odpowiedzi z zasobu wewnętrznego **jest odzwierciedlona** w odpowiedzi dla użytkownika

---

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

## WSTG-CRYP-01 — Testowanie pod kątem słabych zabezpieczeń warstwy transportowej

Testowanie pod kątem słabych zabezpieczeń warstwy transportowej obejmuje analizę implementacji i konfiguracji SSL/TLS w celu zapewnienia poufności i integralności danych podczas transmisji. Atakujący celują w błędy konfiguracji, takie jak przestarzałe protokoły (SSLv2, TLS 1.0), słabe zestawy szyfrów (NULL, RC4 lub anonimowe) oraz nieważne lub słabo podpisane certyfikaty, aby przeprowadzić ataki typu Man-in-the-Middle (MitM). Test ten zazwyczaj dotyczy punktów końcowych serwera WWW aplikacji lub systemów równoważenia obciążenia (load balancer), gdzie nawiązywana jest szyfrowana komunikacja. Skuteczna eksploatacja pozwala napastnikowi na przechwycenie wrażliwych danych, przejęcie sesji użytkownika lub obniżenie poziomu bezpieczeństwa połączenia do stanów niezabezpieczonych poprzez wymuszenie słabszego protokołu (downgrade) lub deszyfrację ruchu.


| Pole | Wartość |
|---|---|
| **ID WSTG** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Wysoki / Średni* |

> *Poziom istotności jest Wysoki, jeśli przesyłane są dane wrażliwe (PII, dane uwierzytelniające, tokeny sesyjne); Średni dla ogólnego ruchu informacyjnego.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Czy obsługiwane są przestarzałe lub niebezpieczne protokoły TLS/SSL?
- [ ] Nie — włączone są wyłącznie TLS 1.2 i TLS 1.3  
- [ ] Tak — włączone są TLS 1.0 lub TLS 1.1  
- [ ] Tak — włączone są SSLv2 lub SSLv3 *(Krytyczne)*  

### Czy serwer akceptuje słabe lub niebezpieczne zestawy szyfrów?
- [ ] Nie — obsługiwane są tylko silne, nowoczesne szyfry (np. AES-GCM, ChaCha20)  
- [ ] Tak — obsługiwane są szyfry ze słabym szyfrowaniem (np. 3DES, RC4) lub o niskiej długości klucza  
- [ ] Tak — obsługiwane są szyfry NULL, anonimowe (ADH) lub eksportowe *(Krytyczne)*  

### Czy certyfikat serwera jest ważny i zaufany?
- [ ] Tak — certyfikat jest ważny, pasuje do domeny i jest podpisany przez zaufany urząd certyfikacji (CA)  
- [ ] Nie — certyfikat wygasł lub używa słabego algorytmu podpisu (np. SHA-1)  
- [ ] Nie — certyfikat jest podpisany samodzielnie (self-signed) lub występuje niezgodność nazwy domeny (mismatch)  

### Czy serwer obsługuje Secure Renegotiation i zapobiega atakom typu Downgrade?
- [ ] Tak — Secure Renegotiation jest obsługiwana i mechanizm TLS Fallback SCSV jest włączony  
- [ ] Nie — Secure Renegotiation nie jest obsługiwana, co potencjalnie umożliwia wstrzyknięcie danych typu MitM  
- [ ] Nie — serwer jest podatny na ataki POODLE lub ROBOT  

### Czy nagłówek HTTP Strict Transport Security (HSTS) jest prawidłowo zaimplementowany?

| Nagłówek | Obecny | Brak |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Tak — nagłówek HSTS jest obecny z długim czasem `max-age` i włączoną opcją `includeSubDomains`  
- [ ] Tak — nagłówek HSTS jest obecny, ale brakuje opcji `includeSubDomains` lub ma on krótki `max-age`  
- [ ] Nie — nagłówek HSTS nie jest obecny

---

## WSTG-CRYP-02 — Testowanie podatności na Padding Oracle

Podatności typu Padding Oracle występują, gdy proces deszyfrowania aplikacji ujawnia informacje o tym, czy dopełnienie (padding) odszyfrowanego tekstu zaszyfrowanego (ciphertext) jest prawidłowe czy nie. Poprzez obserwację tych odpowiedzi kanałem bocznym (side-channel) — takich jak odrębne kody statusu HTTP, specyficzne komunikaty o błędach lub różnice w czasie odpowiedzi — atakujący może odszyfrować dane bez znajomości klucza szyfrującego i potencjalnie ponownie zaszyfrować dowolny tekst jawny (plaintext). Podatność ta zazwyczaj objawia się w zaszyfrowanych plikach cookie sesji, tokenach uwierzytelniających lub parametrach URL wykorzystujących szyfry blokowe w trybie Cipher Block Chaining (CBC) z dopełnieniem PKCS#7. Eksploatacja pozwala na całkowitą utratę poufności i może prowadzić do naruszenia integralności poprzez konstruowanie sfałszowanych bloków zaszyfrowanych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Wysoki / Krytyczny* |

> *Poziom krytyczności jest krytyczny, jeśli podatność występuje w zarządzaniu sesją, tokenach tożsamości lub szyfrowaniu danych osobowych (PII).

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Czy szyfr blokowy w trybie CBC jest używany dla wrażliwych danych?
- [ ] Nie — aplikacja używa uwierzytelnionego szyfrowania (AES-GCM/ChaCha20-Poly1305)  
- [ ] Tak — aplikacja używa szyfrów blokowych, ale dopełnienie (padding) **nie jest** wymagane (np. szyfry strumieniowe lub tryb CTR)  
- [ ] Tak — aplikacja używa trybu CBC i stosowane jest dopełnienie (**padding**)  

### Czy serwer ujawnia poprawność dopełnienia poprzez kanały boczne?
- [ ] Nie — serwer zwraca jednolite komunikaty o błędach i spójne czasy odpowiedzi  
- [ ] Tak — serwer zwraca różne kody statusu HTTP (np. 200 vs 500) dla błędów dopełnienia  
- [ ] Tak — serwer zwraca specyficzne komunikaty o błędach (np. "Invalid Padding", "Decryption failed")  
- [ ] Tak — serwer wykazuje mierzalne różnice czasowe podczas niepowodzenia deszyfrowania  

### Czy możliwa jest automatyczna deszyfracja tekstu zaszyfrowanego?
- [ ] Nie — kanały boczne **nie są** wystarczająco stabilne do eksploatacji  
- [ ] Tak — tekst zaszyfrowany (ciphertext) **może** zostać częściowo odszyfrowany (ostatnie kilka bloków)  
- [ ] Tak — pełne odzyskanie tekstu jawnego (plaintext) **jest możliwe** przy użyciu narzędzi takich jak `PadBuster`  

### Czy atakujący może wykonać atak bit-flipping lub sfałszować poprawne teksty zaszyfrowane?
- [ ] Nie — mechanizmy kontroli integralności (HMAC) są weryfikowane **przed** deszyfrowaniem  
- [ ] Tak — brak kontroli integralności, ale konstrukcja ładunku (Payload) **nie jest** wykonalna  
- [ ] Tak — konstrukcja dowolnego tekstu zaszyfrowanego **jest możliwa**, co pozwala na eskalację uprawnień (Privilege Escalation) lub wstrzykiwanie danych (Data Injection)

---

## WSTG-CRYP-03 — Testowanie pod kątem przesyłania wrażliwych informacji kanałami nieszyfrowanymi

Przesyłanie wrażliwych danych kanałami nieszyfrowanymi, takimi jak zwykły protokół HTTP, naraża informacje na przechwycenie i modyfikację poprzez ataki typu Man-in-the-Middle (MITM). Ta podatność zazwyczaj występuje, gdy aplikacje webowe nie wymuszają protokołu Transport Layer Security (TLS) na wszystkich punktach końcowych (endpoints), szczególnie w starszych komponentach, formularzach logowania lub podczas początkowej fazy przejścia z HTTP na HTTPS. Z perspektywy atakującego pozwala to na pasywny sniffing poświadczeń uwierzytelniających, tokenów sesyjnych oraz danych osobowych (Personally Identifiable Information - PII) w przejętych lub niezaufanych segmentach sieci, takich jak publiczne sieci Wi-Fi. Zapewnienie, że wszystkie wrażliwe dane są zamknięte wewnątrz solidnego tunelu TLS, jest kluczowe dla zachowania poufności i integralności interakcji użytkownika oraz zapobiegania przejęciom sesji (session hijacking).


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Wysoki / Średni |

> *Poziom zagrożenia staje się Wysoki, jeśli poświadczenia uwierzytelniające, identyfikatory sesji lub PII są przesyłane tekstem jawnym (cleartext).

**Odnośniki:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### Czy aplikacja zezwala na komunikację przez nieszyfrowany protokół HTTP dla wrażliwych punktów końcowych?
- [ ] Nie — cały ruch aplikacji jest rygorystycznie wymuszany przez HTTPS *(Najbardziej bezpieczne)*  
- [ ] Tak — strony niewrażliwe są dostępne przez HTTP, ale wrażliwe punkty końcowe **nie są**  
- [ ] Tak — wrażliwe punkty końcowe (logowanie, koszyk, profil) **są** dostępne przez nieszyfrowany protokół HTTP *(Wysokie ryzyko)*  

### Czy zaimplementowano globalne przekierowanie z HTTP na HTTPS?
- [ ] Tak — aplikacja wykonuje przekierowanie 301/302 do HTTPS dla wszystkich żądań HTTP  
- [ ] Tak — przekierowanie jest wykonywane tylko dla konkretnych wrażliwych punktów końcowych  
- [ ] Nie — żadne przekierowanie z HTTP na HTTPS **nie jest stosowane**  

### Czy zaimplementowano HTTP Strict Transport Security (HSTS)?
- [ ] Tak — HSTS jest **włączony** z długim parametrem `max-age` oraz zawiera dyrektywy `includeSubDomains` i `preload`  
- [ ] Tak — HSTS jest **włączony**, ale brakuje dyrektyw `includeSubDomains` lub `preload`  
- [ ] Nie — HSTS **nie jest włączony** na serwerze aplikacji  

### Czy wrażliwe pliki cookie są chronione przed transmisją tekstem jawnym?

| Nazwa pliku cookie | Obecna flaga Secure | Brak flagi Secure |
|--------------------|:-------------------:|:-----------------:|
| `SessionID`        |                     |                   |
| `AuthToken`        |                     |                   |
| `CSRF-Token`       |                     |                   |

### Czy aplikacja przesyła wrażliwe dane do zewnętrznych API kanałami nieszyfrowanymi?
- [ ] Nie — wszystkie zewnętrzne wywołania API są wykonywane przez szyfrowane tunele (HTTPS)  
- [ ] Tak — niektóre niewrażliwe dane telemetryczne są wysyłane przez HTTP  
- [ ] Tak — klucze API, PII lub poświadczenia **są** wysyłane do podmiotów zewnętrznych przez nieszyfrowany protokół HTTP

---

## WSTG-CRYP-04 — Testowanie pod kątem słabych prymitywów kryptograficznych

Słabe prymitywy kryptograficzne obejmują stosowanie przestarzałych lub niebezpiecznych algorytmów, takich jak MD5, SHA-1, DES lub RC4, które są podatne na ataki kolizyjne (collision attacks), analizę częstotliwościową lub deszyfrowanie metodą Brute Force. Słabości te występują zazwyczaj podczas haszowania haseł, szyfrowania danych w spoczynku (data encryption at rest), generowania tokenów sesyjnych lub weryfikacji podpisów cyfrowych w backendzie aplikacji. Z perspektywy atakującego, zidentyfikowanie tych prymitywów pozwala na naruszenie integralności i poufności danych, np. poprzez łamanie przechwyconych haszy w celu odzyskania haseł w formie tekstu jawnego lub fałszowanie tokenów uwierzytelniających. Nowoczesne aplikacje powinny zamiast tego wykorzystywać standardowe w branży, odporne na kolizje i charakteryzujące się wysoką entropią prymitywy, takie jak Argon2, Bcrypt lub AES-GCM, aby zapewnić solidną ochronę przed kryptoanalizą.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Status testu** | Nie wykonano |
| **Dotkliwość** | Wysoka |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Czy do przechowywania wrażliwych danych używane są przestarzałe algorytmy haszujące, takie jak MD5 lub SHA-1?
- [ ] Nie — aplikacja korzysta z nowoczesnych algorytmów odpornych na kolizje, takich jak Argon2 lub Bcrypt  
- [ ] Tak — przestarzałe algorytmy są używane, ale **wyłącznie** do kontroli integralności danych niewrażliwych z punktu widzenia bezpieczeństwa  
- [ ] Tak — przestarzałe algorytmy **są używane** do haseł lub wrażliwych tokenów *(Wysoka)*  

### Czy obecnie stosowane są słabe symetryczne szyfry blokowe, takie jak DES, 3DES lub RC4?
- [ ] Nie — używany jest algorytm AES (128-bitowy lub wyższy) w bezpiecznym trybie, takim jak GCM lub CBC  
- [ ] Tak — starsze szyfry są **włączone** w celu zapewnienia kompatybilności wstecznej, ale **nie są** domyślne  
- [ ] Tak — starsze szyfry są główną metodą ochrony danych w spoczynku lub w transmisji  

### Czy aplikacja implementuje własną („roll-your-own”) lub niestandardową logikę kryptograficzną?
- [ ] Nie — aplikacja ściśle przestrzega standardowych, recenzowanych bibliotek kryptograficznych  
- [ ] Tak — istnieją niestandardowe wrappery, ale bazowe prymitywy **są** standardem branżowym  
- [ ] Tak — do wrażliwych danych **stosowane są** autorskie algorytmy kryptograficzne lub niestandardowa logika *(Krytyczna)*  

### Czy sole kryptograficzne i wektory inicjujące (IV) są zarządzane zgodnie z najlepszymi praktykami?
- [ ] Tak — sole są unikalne dla każdego użytkownika, a IV są nieprzewidywalne dla każdej operacji  
- [ ] Tak — sole są używane, ale **nie są** unikalne dla wszystkich rekordów  
- [ ] Nie — sole są statyczne, zahardkodowane lub ich brakuje, a IV **są używane wielokrotnie** w różnych sesjach  

### Czy długość kluczy asymetrycznych (RSA, Diffie-Hellman) jest wystarczająca według nowoczesnych standardów?
- [ ] Tak — klucze RSA/DH mają 2048 bitów lub więcej, albo używana jest kryptografia krzywych eliptycznych (ECC)  
- [ ] Nie — klucze mają mniej niż 2048 bitów, ale obejście zabezpieczeń **nie jest** trywialne  
- [ ] Nie — klucze mają 1024 bity lub mniej, co czyni je **podatnymi** na faktoryzację lub ataki obliczeniowe

---

## WSTG-BUSL-01 — Testowanie walidacji danych logiki biznesowej

Testowanie walidacji danych logiki biznesowej koncentruje się na zdolności aplikacji do identyfikowania i odrzucania danych, które są poprawne pod względem składniowym, ale nieprawidłowe pod względem logicznym w kontekście domeny biznesowej. Atakujący wykorzystują te błędy, przesyłając nieoczekiwane wartości — takie jak ujemne ilości w koszyku zakupowym, daty z przeszłości dla wydarzeń przyszłych lub ekstremalnie duże liczby całkowite — w celu obejścia ograniczeń i manipulacji stanem aplikacji. Podatności te często występują w wieloetapowych przepływach pracy (workflows), procesach zamówień oraz modułach zarządzania kontem, gdzie logika po stronie serwera nie weryfikuje integralności kontekstowej danych dostarczonych przez użytkownika. Skuteczna eksploatacja może prowadzić do strat finansowych, naruszenia integralności oraz nieautoryzowanego dostępu do zastrzeżonych funkcji lub danych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Wysoki / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Czy aplikacja wymusza logiczne limity zakresu dla wejściowych danych numerycznych?
- [ ] Tak — aplikacja poprawnie odrzuca wartości ujemne, zerowe lub nadmiernie duże, gdy są one niewłaściwe *(Najbardziej bezpieczne)*  
- [ ] Tak — aplikacja odrzuca niektóre nieprawidłowe zakresy, ale **jest** podatna na przepełnienie liczby całkowitej (integer overflow) lub obejście granic (boundary bypass)  
- [ ] Nie — aplikacja akceptuje każdą wartość numeryczną niezależnie od kontekstu biznesowego *(Krytyczne)*  

### Czy weryfikacja walidacji po stronie serwera jest spójna we wszystkich warstwach aplikacji?
- [ ] Tak — walidacja jest stosowana spójnie zarówno w warstwie API/usługi, jak i bazy danych  
- [ ] Tak — walidacja **jest stosowana** w interfejsie użytkownika (UI/Frontend), ale obejście poprzez bezpośrednie żądanie API **jest możliwe**  
- [ ] Nie — walidacja **nie jest stosowana** lub jest niespójna, co pozwala na logiczne uszkodzenie danych  

### Czy logika biznesowa może zostać naruszona poprzez manipulację danymi w wieloetapowych procesach (workflows)?
- [ ] Nie — stan jest utrzymywany w bezpieczny sposób na serwerze, a dane z poprzednich kroków **nie mogą** zostać zmodyfikowane  
- [ ] Tak — walidacja odbywa się w początkowych krokach, ale końcowe przesłanie danych **nie weryfikuje** ponownie integralności danych  
- [ ] Tak — obejście procesu (workflow bypass) **jest możliwe** poprzez pomijanie kroków lub przesyłanie danych poza kolejnością  

### Czy aplikacja prawidłowo obsługuje dane typu „edge case”, które omijają ograniczenia biznesowe?
- [ ] Tak — ograniczenia są solidne i obsługują nietypowe, ale poprawne składniowo dane wejściowe (np. lata przestępne, zmiany stref czasowych)  
- [ ] Tak — niektóre przypadki graniczne (edge cases) są obsługiwane, ale określone kombinacje **mogą** prowadzić do nieoczekiwanego zachowania  
- [ ] Nie — przypadki graniczne prowadzą bezpośrednio do błędów logiki, takich jak darmowe zakupy lub nieautoryzowane zmiany statusu

---

## WSTG-BUSL-02 — Testowanie możliwości fałszowania żądań

Fałszowanie żądań w kontekście logiki biznesowej polega na manipulowaniu parametrami zdefiniowanymi przez aplikację w celu wykonania działań wykraczających poza zamierzony przepływ pracy (workflow) lub poziom uprawnień. Atakujący biorą na cel procesy wieloetapowe, takie jak systemy realizacji zamówień (checkout) lub rejestracja kont, gdzie stan jest utrzymywany za pomocą parametrów po stronie klienta, których serwer nie weryfikuje ponownie względem zaufanego źródła po stronie backendu. Poprzez zmianę ukrytych pól, wartości JSON lub parametrów REST, takich jak ceny, ilości lub wewnętrzne flagi statusu, atakujący może ominąć wymagania dotyczące płatności, przeprowadzić eskalację uprawnień (privilege escalation) lub wywołać niezamierzone zmiany stanu. Ta podatność zazwyczaj objawia się w stanowych formularzach internetowych lub interfejsach API, gdzie serwer bezkrytycznie ufa reprezentacji stanu biznesowego przesłanej przez klienta podczas transakcji.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom krytyczności** | Wysoki / Krytyczny* |

> *Poziom krytyczności staje się Krytyczny, jeśli sfałszowane żądanie pozwala na straty finansowe, nieautoryzowane działania administracyjne lub pełne przejęcie konta.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Czy aplikacja weryfikuje parametry transakcyjne względem serwerowego „źródła prawdy” (source of truth)?
- [ ] Tak — wszystkie wrażliwe parametry (cena, rola, ID) są weryfikowane w bazie danych, a ich obejście jest **niemożliwe**  
- [ ] Tak — walidacja jest stosowana, ale można ją ominąć poprzez Parameter Pollution lub alternatywne kodowanie  
- [ ] Nie — aplikacja ufa wartościom po stronie klienta w celu określenia kosztu lub wyniku transakcji *(Krytyczny)*  

### Czy wartości krytyczne dla biznesu (np. ilości, kwoty) mogą być modyfikowane na wartości nieautoryzowane?
- [ ] Nie — wartości ujemne, zerowe lub nadmiernie wysokie są odrzucane przez serwer  
- [ ] Tak — serwer akceptuje zmodyfikowane wartości, ale tylko w ograniczonym zakresie  
- [ ] Tak — wartości ujemne lub zerowe **mogą** zostać przesłane, co prowadzi do strat finansowych lub obejścia logiki  

### Czy ukryte pola lub parametry API są używane do utrzymywania stanu w procesach wieloetapowych?
- [ ] Nie — stan jest utrzymywany w bezpieczny sposób za pomocą sesji po stronie serwera lub podpisanych tokenów  
- [ ] Tak — stan jest przekazywany przez klienta, ale kontrole integralności (HMAC/Signatures) są **włączone** i skuteczne  
- [ ] Tak — stan jest przekazywany przez klienta, a kontrole integralności są **wyłączone** lub mogą zostać usunięte  

### Czy możliwe jest sfałszowanie żądań pomijających obowiązkowe kroki pośrednie w przepływie pracy?
- [ ] Nie — serwer wymusza ścisłą sekwencję operacji dla każdej transakcji  
- [ ] Tak — kroki **mogą** zostać pominięte, ale ostateczne działanie nadal wymaga prawidłowych danych z poprzednich kroków  
- [ ] Tak — ostateczne żądanie „wyślij” (submit) lub „zakończ” (complete) **może** zostać sfałszowane bezpośrednio w celu ominięcia autoryzacji lub kroków płatności

---

## WSTG-BUSL-03 — Testy kontroli integralności

Testowanie kontroli integralności koncentruje się na weryfikacji, czy aplikacja zapewnia, że dane pozostają niezmienione i wiarygodne podczas ich przesyłania przez różne procesy biznesowe lub między klientem a serwerem. Atakujący biorą na cel tę logikę, manipulując krytycznymi parametrami — takimi jak ceny produktów, ilości, uprawnienia użytkowników lub stany transakcji — wewnątrz żądań HTTP, ukrytych pól lub plików cookie (cookies). Jeśli w aplikacji brakuje weryfikacji po stronie serwera lub solidnych mechanizmów kontroli integralności, takich jak Hashing czy HMACs, napastnik może manipulować tymi wartościami w celu dokonania oszustwa, eskalacji własnych uprawnień lub obejścia zamierzonych ograniczeń biznesowych. Test ten jest kluczowy dla każdej aplikacji obsługującej transakcje finansowe, wrażliwe zmiany stanów lub wieloetapowe przepływy pracy (workflows), gdzie dane wyjściowe jednego etapu służą jako zaufane dane wejściowe dla następnego.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się Wysoki, jeśli naruszenie integralności umożliwia kradzież środków finansowych, nieautoryzowaną eskalację uprawnień lub modyfikację trwałych rekordów.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Czy wrażliwe parametry biznesowe są narażone na manipulację po stronie klienta?
- [ ] Nie — wrażliwe wartości są zarządzane wyłącznie po stronie serwera  
- [ ] Tak — wartości takie jak cena, ilość lub flagi statusu **są** ujawnione, ale zaszyfrowane  
- [ ] Tak — wartości **są** ujawnione w formie otwartego tekstu (plain text) w ukrytych polach, parametrach URL lub plikach cookie  

### Czy mechanizm integralności (np. HMAC, Checksum) jest stosowany do parametrów kontrolowanych przez klienta?
- [ ] Tak — solidna kontrola integralności **jest stosowana** i weryfikowana przy każdym żądaniu  
- [ ] Tak — kontrola integralności **jest stosowana**, ale używa słabego/przewidywalnego algorytmu lub zakodowanego na stałe (hardcoded) sekretu  
- [ ] Nie — żadne kontrole integralności **nie są stosowane** do wrażliwych parametrów  

### Czy kontrola integralności może zostać pominięta lub obliczona ponownie przez atakującego?
- [ ] Nie — weryfikacja sygnatury jest rygorystycznie wymuszana, a obejście **nie jest możliwe**  
- [ ] Tak — weryfikacja sygnatury może zostać pominięta poprzez usunięcie parametru sygnatury  
- [ ] Tak — sygnatura **może** zostać obliczona ponownie, ponieważ klucz tajny lub logika są ujawnione/słabe  

### Czy aplikacja wykonuje ponowną walidację danych po stronie backendu względem zaufanego źródła?
- [ ] Tak — serwer ponownie waliduje wszystkie wartości względem bazy danych i obejście **nie jest możliwe**  
- [ ] Tak — serwer wykonuje częściową walidację, ale obejście **jest możliwe** w określonych przypadkach brzegowych (edge cases)  
- [ ] Nie — serwer ufa danym dostarczonym przez klienta bez weryfikacji po stronie backendu *(Krytyczne)*  

### Czy możliwa jest manipulacja stanem procesu wieloetapowego?
- [ ] Nie — stan sesji jest zarządzany po stronie serwera i nie może być manipulowany  
- [ ] Tak — informacje o stanie są przekazywane przez klienta i manipulacja **jest możliwa** w celu pominięcia kroków lub powtórzenia działań

---

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

## WSTG-BUSL-05 — Testowanie limitów liczby użyć danej funkcji

Ten test ocenia, czy aplikacja wymusza ograniczenia logiki biznesowej dotyczące częstotliwości oraz całkowitej liczby wykonań określonej akcji lub funkcji przez pojedynczego użytkownika, sesję lub adres IP. Brak wdrożenia tych limitów pozwala atakującym na nadużywanie istotnych funkcji — takich jak wysyłanie kodów SMS OTP, generowanie e-maili do resetowania hasła, stosowanie kodów rabatowych czy wykonywanie obciążających eksportów danych — co prowadzi do strat finansowych, wyczerpania zasobów lub utraty reputacji. Podatności te zazwyczaj występują w końcówkach API (endpoints) lub funkcjach komunikacyjnych i są wykorzystywane za pomocą zautomatyzowanych skryptów powtarzających akcję znacznie powyżej zamierzonych progów biznesowych. Z perspektywy atakującego jest to główny cel dla ataków typu SMS pumping, mail bombing lub workflowów typu Brute Force, które nie posiadają jawnych ograniczeń Rate Limiting lub liczników użyć.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się Wysoki, jeśli funkcja obejmuje transakcje finansowe, koszty SMS lub wpływa na dostępność całego systemu.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### Czy aplikacja definiuje i wymusza limity dla wrażliwych funkcji biznesowych?
- [ ] Tak — limity są rygorystycznie wymuszane, a ich obejście jest **niemożliwe**  
- [ ] Tak — limity są wymuszane, ale są zbyt wysokie, aby zapobiec nadużyciom  
- [ ] Nie — nie stosuje się żadnych limitów dla wykonania funkcji  

### Czy limity Rate Limiting oraz liczniki użycia są wymuszane po stronie serwera?
- [ ] Tak — wymuszanie odbywa się wyłącznie po stronie serwera i **nie może** być manipulowane  
- [ ] Nie — wymuszanie opiera się na logice po stronie klienta (np. JavaScript lub ukryte pola)  
- [ ] Nie — nie istnieje żadne wymuszanie na żadnym poziomie  

### Czy limity użycia mogą zostać pominięte poprzez manipulację nagłówkami lub sesją?
- [ ] Nie — obejście poprzez nagłówki `X-Forwarded-For`, `Origin` lub rotację sesji jest **niemożliwe**  
- [ ] Tak — limity **mogą** zostać pominięte poprzez podszywanie się pod nagłówki IP lub usuwanie plików cookie  
- [ ] Tak — limity **mogą** zostać pominięte poprzez tworzenie wielu kont lub sesji  

### Czy przekroczenie limitu wyzwala odpowiednią reakcję obronną?
- [ ] Tak — aplikacja zwraca kod `429 Too Many Requests` i loguje zdarzenie *(Najbezpieczniejsze)*  
- [ ] Tak — aplikacja zwraca błąd, ale **nie** loguje potencjalnego nadużycia  
- [ ] Nie — aplikacja kontynuuje przetwarzanie żądań, ale ignoruje wynik  
- [ ] Nie — aplikacja nie udziela odpowiedzi lub ulega awarii pod obciążeniem  

### Czy wpływ nadużycia funkcji jest istotny dla biznesu?
- [ ] Nie — nadużycie funkcji ma znikomy wpływ na koszty lub zasoby  
- [ ] Tak — nadużycie **może** prowadzić do niewielkiego zużycia zasobów lub uciążliwości dla użytkownika  
- [ ] Tak — nadużycie **może** prowadzić do znacznych kosztów finansowych (np. opłaty za SMS) lub odmowy usługi *(Krytyczne)*

---

## WSTG-BUSL-06 — Testowanie pod kątem omijania przepływów pracy

Omijanie przepływu pracy (Workflow) polega na manipulowaniu logiką aplikacji w celu pominięcia zamierzonych sekwencji lub wymagań wstępnych w procesach wieloetapowych. Atakujący identyfikują wrażliwe punkty końcowe (endpoints) — takie jak potwierdzenia płatności, zatwierdzenia administracyjne lub ekrany zakończenia MFA — i próbują uzyskać do nich bezpośredni dostęp, pomijając obowiązkowe kroki pośrednie. Ta podatność występuje, gdy logika po stronie serwera nie utrzymuje solidnej maszyny stanów, polegając zamiast tego na założeniu, że użytkownik podąża ścieżką wyznaczoną przez interfejs użytkownika (UI). Skuteczna eksploatacja może prowadzić do krytycznych błędów logiki biznesowej (Business Logic), w tym nieautoryzowanych transakcji, eskalacji uprawnień (Privilege Escalation) oraz omijania mechanizmów weryfikacji tożsamości.


| Pole | Wartość |
|---|---|
| **ID WSTG** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się wysoki, jeśli obejście pozwala na nieautoryzowane transakcje finansowe, eskalację uprawnień lub dostęp administracyjny.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Czy aplikacja zawiera wieloetapowe procesy biznesowe?
- [ ] Nie — aplikacja składa się wyłącznie z akcji opartych na pojedynczych żądaniach  
- [ ] Tak — obecne są procesy wieloetapowe (np. koszyki zakupowe, formularze typu wizard, rejestracja)  

### Czy sekwencja kroków jest ściśle egzekwowana przez serwer?
- [ ] Tak — śledzenie stanu po stronie serwera zapewnia, że kroki **nie mogą** zostać pominięte  
- [ ] Tak — mechanizmy kontrolne po stronie klienta sugerują sekwencję, ale serwer **nie waliduje** postępu  
- [ ] Nie — aplikacja **nie wymusza** żadnej konkretnej kolejności operacji  

### Czy atakujący może dotrzeć do końcowego etapu procesu poprzez bezpośrednie wywołanie docelowego adresu URL lub punktu końcowego (endpoint)?
- [ ] Nie — bezpośredni dostęp do kroków końcowych **nie jest możliwy** bez wcześniejszego ukończenia etapów poprzedzających  
- [ ] Tak — kroki końcowe (np. `/api/v1/checkout/complete`) **mogą** zostać wywołane poprzez bezpośrednie żądanie  

### Czy aplikacja weryfikuje, czy dane wymagane z poprzednich kroków są obecne i prawidłowe?
- [ ] Tak — serwer waliduje wszystkie dane z poprzednich kroków przed kontynuowaniem  
- [ ] Nie — serwer **jest** podatny na „pomijanie” kroków, w których dane są oczekiwane, ale nie są weryfikowane  

### Czy mechanizmy kontroli bezpieczeństwa, takie jak MFA lub weryfikacja tożsamości, mogą zostać pominięte poprzez manipulację przepływem pracy?
- [ ] Nie — krytyczne mechanizmy kontrolne **nie mogą** zostać obejściowe poprzez manipulację przepływem  
- [ ] Tak — obejście MFA lub weryfikacji tożsamości **jest możliwe** poprzez przejście bezpośrednio do kroku następującego po weryfikacji

---

## WSTG-BUSL-07 — Testowanie mechanizmów obronnych przed nadużyciami aplikacji

Mechanizmy obronne przed nadużyciami aplikacji oceniają zdolność aplikacji do identyfikowania, rejestrowania i reagowania na anomalne zachowania użytkowników, które odbiegają od zamierzonej logiki biznesowej (Business Logic). W przeciwieństwie do tradycyjnych zabezpieczeń opartych na sygnaturach, mechanizmy te koncentrują się na wykrywaniu wzorców takich jak szybkie serie żądań (rapid-fire requests), Credential Stuffing czy sekwencyjna enumeracja zasobów, które sygnalizują zautomatyzowane nadużycia. Z perspektywy atakującego, test ten określa odporność aplikacji na automatyzację oraz ataki typu "low and slow", zaprojektowane w celu eksfiltracji danych lub wyczerpania zasobów bez wyzwalania standardowych alertów bezpieczeństwa. Brak wdrożenia tych zabezpieczeń często skutkuje masowym scrapingiem danych (Data Scraping), przejęciami kont (Account Takeover) lub atakami odmowy usługi (DoS) poprzez legalne, ale zasobożerne funkcje aplikacji.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Średni / Wysoki |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Czy mechanizmy Rate Limiting lub Throttling są wymuszane na wrażliwych punktach końcowych (endpoints)?
- [ ] Tak — rygorystyczny Rate Limiting **jest stosowany** i wymuszany na wszystkich wrażliwych punktach końcowych *(Najbardziej bezpieczne)*  
- [ ] Tak — Rate Limiting **jest stosowany**, ale można go pominąć poprzez rotację adresów IP lub manipulację nagłówkami  
- [ ] Tak — Rate Limiting **jest stosowany** tylko w punktach końcowych uwierzytelniania, pozostawiając inne niezabezpieczone  
- [ ] Nie — Rate Limiting ani Throttling **nie są stosowane** w żadnych funkcjach aplikacji  

### Czy aplikacja wykrywa i blokuje wzorce interakcji zautomatyzowanych?
- [ ] Tak — analiza behawioralna lub mechanizmy CAPTCHA **są włączone** i skutecznie blokują boty  
- [ ] Tak — zabezpieczenia przed automatyzacją (Anti-automation) **są włączone**, ale ich obejście **jest możliwe** przy użyciu przeglądarek headless lub cyklicznej zmiany sesji (Session Cycling)  
- [ ] Nie — aplikacja **nie potrafi** odróżnić użytkownika (człowieka) od zautomatyzowanego skryptu  

### Czy anomalne zachowania są rejestrowane i czy następuje po nich reakcja obronna?
- [ ] Tak — nadużycie wyzwala automatyczne blokowanie i powiadamia zespół ds. bezpieczeństwa  
- [ ] Tak — nadużycie jest rejestrowane w logach na potrzeby audytu, ale **nie są** podejmowane żadne automatyczne działania obronne  
- [ ] Nie — aplikacja **nie rejestruje** ani nie reaguje na nietypowe wzorce aktywności  

### Czy mechanizmy obronne mogą zostać pominięte poprzez manipulację identyfikatorami po stronie klienta lub nagłówkami?
- [ ] Nie — mechanizmy obronne opierają się na stanie po stronie serwera i zweryfikowanej telemetrii  
- [ ] Tak — mechanizmy kontrolne **mogą** zostać pominięte poprzez czyszczenie plików cookie lub rotację ciągów `User-Agent`  
- [ ] Tak — mechanizmy kontrolne **mogą** zostać pominięte poprzez wstrzykiwanie nagłówków takich jak `X-Forwarded-For` lub `X-Real-IP`

---

## WSTG-BUSL-08 — Testowanie przesyłania nieoczekiwanych typów plików

Przesyłanie nieoczekiwanych typów plików pozwala atakującym na obejście ograniczeń logiki biznesowej poprzez wysyłanie plików, których aplikacja nie zamierza przetwarzać, co potencjalnie prowadzi do zdalnego wykonania kodu (Remote Code Execution - RCE), Cross-Site Scripting (XSS) lub odmowy usługi (Denial of Service - DoS). Atakujący badają te podatności, modyfikując rozszerzenia plików, typy MIME i Magic Bytes, aby oszukać logikę walidacji po stronie serwera i skłonić ją do zaakceptowania złośliwej zawartości. Zjawisko to występuje zazwyczaj przy wgrywaniu zdjęć profilowych, w systemach zarządzania dokumentami lub załącznikach do zgłoszeń serwisowych (support tickets), gdzie walidacja po stronie backendu jest niewystarczająca. Udana eksploatacja może zagrozić serwerowi hostującemu, jeśli przesłany plik zostanie wykonany, lub prowadzić do ataków po stronie klienta, jeśli plik zostanie zaserwowany innym użytkownikom z nieprawidłowym nagłówkiem `Content-Type`.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Status testu** | Nie przeprowadzono |
| **Krytyczność** | Wysoka / Krytyczna |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Czy aplikacja ogranicza przesyłanie plików do określonego zestawu rozszerzeń?
- [ ] Tak — wymuszana jest ścisła biała lista (whitelist) rozszerzeń i obejście **nie jest możliwe**  
- [ ] Tak — biała lista rozszerzeń istnieje, ale obejście **jest możliwe** za pomocą podwójnych rozszerzeń lub bajtów null (null bytes)  
- [ ] Nie — każde rozszerzenie pliku **może** zostać przesłane  

### Czy zawartość pliku jest walidowana poza samym rozszerzeniem?
- [ ] Tak — logika po stronie serwera weryfikuje Magic Bytes i przeprowadza głęboką inspekcję treści  
- [ ] Tak — logika po stronie serwera weryfikuje typ MIME wyłącznie za pomocą nagłówka `Content-Type`  
- [ ] Nie — po sprawdzeniu rozszerzenia **nie jest stosowana** żadna walidacja zawartości  

### Czy atakujący może wykonać kod poprzez przesłanie web shella lub skryptu?
- [ ] Nie — przesłane pliki są przechowywane w katalogu bez uprawnień do wykonywania lub w kontenerze danych (S3/Azure Blob)  
- [ ] Tak — pliki są przechowywane w katalogu dostępnym przez WWW, ale wykonywanie **jest wyłączone** poprzez konfigurację serwera  
- [ ] Tak — przesłane złośliwe skrypty **mogą** być bezpośrednio wykonane za pomocą przewidywalnej ścieżki URL *(Krytyczna)*  

### Czy ataki po stronie klienta, takie jak XSS, są możliwe poprzez przesłane typy plików?
- [ ] Nie — pliki są serwowane z nagłówkami `Content-Disposition: attachment` i `X-Content-Type-Options: nosniff`  
- [ ] Tak — pliki SVG lub HTML **mogą** zostać przesłane i wyrenderowane w przeglądarce, co prowadzi do XSS  
- [ ] Tak — metadane obrazu (EXIF) **są przetwarzane** i odzwierciedlane w interfejsie użytkownika bez sanityzacji

---

## WSTG-BUSL-09 — Testowanie przesyłania złośliwych plików

Funkcjonalność przesyłania plików pozwala użytkownikom na przesyłanie danych do serwera, co stanowi bezpośredni wektor wprowadzania złośliwej zawartości do środowiska aplikacji. Atakujący wykorzystują słabą logikę walidacji, aby przesyłać web shells, malware lub specjalnie spreparowane pliki — takie jak SVG lub HTML — w celu uzyskania Remote Code Execution (RCE) lub Cross-Site Scripting (XSS). Podatność ta występuje zazwyczaj w funkcjach takich jak ustawienia profilu, systemy zarządzania dokumentami oraz moduły obsługi załączników, gdzie serwer nie weryfikuje poprawnie typów plików, ich zawartości oraz uprawnień do wykonywania. Skuteczna eksploatacja często prowadzi do pełnego przejęcia systemu, eksfiltracji danych lub wykorzystania serwera jako punktu przeskoku (pivot) do ataków na sieć wewnętrzną.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Status testu** | Nie wykonano |
| **Poziom ważności** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### Czy aplikacja udostępnia funkcjonalność przesyłania plików?
- [ ] Nie — funkcjonalność przesyłania plików nie istnieje  
- [ ] Tak — funkcjonalność istnieje dla uwierzytelnionych użytkowników  
- [ ] Tak — funkcjonalność jest dostępna dla **nieuwierzytelnionych** użytkowników *(Krytyczne)*  

### Czy walidacja rozszerzeń plików jest wymuszana po stronie serwera?
- [ ] Tak — rygorystyczna biała lista (allowlist) rozszerzeń jest **stosowana** i jej obejście **nie jest możliwe**  
- [ ] Tak — biała lista jest stosowana, ale jej obejście **jest możliwe** poprzez manipulację wielkością liter lub podwójne rozszerzenia  
- [ ] Tak — stosowana jest czarna lista (blocklist), którą **można** obejść za pomocą alternatywnych rozszerzeń (np. `.phtml`, `.asa`)  
- [ ] Nie — żadna walidacja rozszerzeń nie jest **stosowana** po stronie serwera  

### Czy zawartość pliku jest walidowana poza samym rozszerzeniem lub typem MIME?
- [ ] Tak — aplikacja weryfikuje magic bytes i przeprowadza głęboką inspekcję zawartości  
- [ ] Tak — aplikacja sprawdza jedynie nagłówek `Content-Type`, który **można** łatwo sfałszować  
- [ ] Nie — aplikacja polega całkowicie na rozszerzeniu pliku w celu walidacji  

### Czy przesłane pliki są przechowywane w katalogu z uprawnieniami do wykonywania?
- [ ] Nie — pliki są przechowywane w dedykowanej usłudze (np. S3) lub w katalogu bez możliwości wykonywania  
- [ ] Tak — pliki są przechowywane na serwerze WWW, ale ich wykonywanie jest **wyłączone** poprzez konfigurację  
- [ ] Tak — pliki są przechowywane w katalogu dostępnym z poziomu WWW i ich wykonywanie **jest dozwolone** *(Krytyczne)*  

### Czy filtry bezpieczeństwa mogą zostać pominięte poprzez manipulację nazwą pliku?
- [ ] Nie — nazwy plików są oczyszczane (sanitized) lub losowo generowane przez serwer  
- [ ] Tak — filtry **mogą** zostać pominięte przy użyciu bajtów zerowych (np. `shell.php%00.jpg`)  
- [ ] Tak — znaki path traversal (np. `../../`) **mogą** być użyte do przesyłania plików do dowolnych katalogów

---

## WSTG-BUSL-10 — Testowanie funkcjonalności płatności

Testowanie funkcjonalności płatności polega na identyfikowaniu błędów logiki biznesowej, które pozwalają atakującemu na ominięcie Payment Gateways, manipulowanie sumami zamówień lub podszywanie się pod sygnały pomyślnej transakcji. Podatności te zazwyczaj występują w komunikacji między aplikacją, przeglądarką klienta a zewnętrznymi procesorami płatności, takimi jak Stripe, PayPal lub wewnętrznymi systemami księgowymi. Atakujący może próbować modyfikować parametry ceny podczas przesyłania, przesyłać ujemne ilości produktów w celu obniżenia całkowitego kosztu lub powtarzać (Replay) wywołania zwrotne (Callbacks) pomyślnych transakcji, aby zrealizować zamówienia bez faktycznej płatności. Skuteczna eksploatacja prowadzi do bezpośrednich strat finansowych, rozbieżności w stanach magazynowych oraz potencjalnego naruszenia integralności e-commerce aplikacji.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Status testu** | Nie przeprowadzono |
| **Dotkliwość** | Wysoka / Krytyczna |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Czy kwota transakcji jest walidowana po stronie serwera przed przetworzeniem?
- [ ] Tak — kwota jest rygorystycznie walidowana względem bazy danych produktów po stronie serwera *(Najbezpieczniejsze)*  
- [ ] Tak — kwota jest walidowana, ale obejście logiki **jest możliwe** poprzez Parameter Pollution lub zmianę waluty  
- [ ] Nie — kwota transakcji **może** zostać zmodyfikowana w żądaniu po stronie klienta i jest akceptowana przez backend  

### Czy ilość pozycji może być manipulowana w celu uzyskania zerowej lub ujemnej kwoty całkowitej?
- [ ] Nie — ujemne lub zerowe ilości są odrzucane przez logikę walidacji aplikacji  
- [ ] Tak — ujemne ilości są akceptowane w koszyku, ale suma jest korygowana podczas płatności  
- [ ] Tak — ujemne ilości **mogą** być użyte do obniżenia całkowitej sumy zamówienia lub uzyskania kredytu *(Krytyczne)*  

### Czy aplikacja bezpiecznie obsługuje wywołania zwrotne (Callbacks) lub Webhooks bramki płatniczej?
- [ ] Tak — wywołania zwrotne wymagają ważnego podpisu kryptograficznego, którego **nie można** sfałszować  
- [ ] Tak — podpisy są sprawdzane, ale klucz (Secret) jest słaby lub weryfikacja **może** zostać pominięta  
- [ ] Nie — aplikacja polega na nieuwierzytelnionych lub niezweryfikowanych wywołaniach zwrotnych w celu potwierdzenia statusu płatności  

### Czy aplikacja jest podatna na Race Conditions podczas etapu finalizacji zamówienia lub płatności?
- [ ] Nie — integralność transakcyjna i mechanizmy blokowania bazy danych są **włączone**  
- [ ] Tak — Race Conditions **są możliwe**, ale nie wpływają na ostateczną cenę ani realizację  
- [ ] Tak — współbieżne żądania **mogą** prowadzić do wielokrotnej realizacji dla pojedynczej płatności lub "podwójnego wydatkowania" (Double-spending) kart podarunkowych  

### Czy kody rabatowe lub punkty lojalnościowe podlegają manipulacji lub ponownemu użyciu?
- [ ] Nie — kody są walidowane pod kątem jednorazowego użytku, a ograniczenia logiczne są **stosowane**  
- [ ] Tak — kody mogą być używane wielokrotnie lub stosowane do niekwalifikujących się pozycji, ale wpływ jest ograniczony  
- [ ] Tak — atakujący **mogą** stosować wiele sprzecznych kodów lub omijać daty wygaśnięcia i limity użycia

---

## WSTG-CLNT-01 — Testing for DOM Based Cross Site Scripting

DOM-based Cross-Site Scripting (DOM XSS) występuje, gdy aplikacja zawiera kod JavaScript po stronie klienta, który przetwarza dane z niezaufanego źródła w sposób niebezpieczny, zazwyczaj poprzez zapisanie tych danych z powrotem do Document Object Model (DOM) za pomocą niebezpiecznego ujścia (sink). W przeciwieństwie do wariantów reflected lub stored XSS, podatność ta istnieje całkowicie w kodzie po stronie klienta; payload często nie jest w ogóle wysyłany do serwera, ponieważ jest przetwarzany przez ujścia takie jak `innerHTML`, `eval()` lub `document.write()`. Atakujący wykorzystują tę podatność, tworząc adresy URL ze złośliwymi fragmentami lub parametrami, które po załadowaniu przez przeglądarkę ofiary wykonują dowolny kod JavaScript w kontekście sesji użytkownika. Może to prowadzić do eksfiltracji tokenów sesyjnych, kradzieży wrażliwych danych oraz wykonywania nieautoryzowanych działań w imieniu uwierzytelnionego użytkownika.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Narzędzia:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Czy niezaufane źródła (sources) są używane do przechwytywania danych kontrolowanych przez użytkownika w kodzie JavaScript?
- [ ] Nie — JavaScript **nie** używa `location.hash`, `location.search` ani `document.referrer`  
- [ ] Tak — niezaufane źródła są używane, ale dane **nie** są przekazywane do żadnych ujść wykonawczych (execution sinks)  
- [ ] Tak — niezaufane źródła są używane, a dane przepływają bezpośrednio do logiki po stronie klienta  

### Czy dane kontrolowane przez użytkownika są przekazywane do niebezpiecznych ujść (sinks) DOM?
- [ ] Nie — ujścia takie jak `innerHTML`, `outerHTML`, `document.write()` lub `eval()` **nie** są używane  
- [ ] Tak — niebezpieczne ujścia są obecne, ale dane są rygorystycznie **sanityzowane** przy użyciu bezpiecznej biblioteki, takiej jak DOMPurify  
- [ ] Tak — niebezpieczne ujścia są obecne, a dane są przetwarzane z użyciem **niewystarczającego** lub niestandardowego filtrowania opartego na wyrażeniach regularnych (regex)  
- [ ] Tak — niebezpieczne ujścia są obecne, a dane są przekazywane **bez** żadnej sanityzacji lub kodowania *(Krytyczne)*  

### Czy ujście wykonawcze (execution sink) może zostać osiągnięte poprzez payload w adresie URL?
- [ ] Nie — przepływ od źródła do ujścia (source-to-sink) **nie może** zostać wywołany poprzez zewnętrzne dane wejściowe  
- [ ] Tak — przepływ jest **możliwy**, ale wymaga złożonej interakcji ze strony użytkownika  
- [ ] Tak — przepływ jest **możliwy** i może zostać wywołany poprzez prosty fragment adresu URL lub parametr zapytania (query parameter)  

### Czy nowoczesne nagłówki bezpieczeństwa lub mechanizmy ochrony w przeglądarce neutralizują ryzyko?
- [ ] Tak — restrykcyjna polityka `Content-Security-Policy` (CSP) z dyrektywami `script-src` oraz `trusted-types` jest **włączona** i zapobiega wykonaniu kodu  
- [ ] Tak — nagłówek CSP jest obecny, ale `unsafe-inline` lub słabe skróty (hashes) sprawiają, że obejście (bypass) jest **możliwe**  
- [ ] No — brak nagłówka CSP mitygującego wykonanie kodu DOM  

### Czy aplikacja korzysta z frameworków frontendowych z wbudowanymi mechanizmami ochrony?
- [ ] Tak — framework (np. Angular, React) automatycznie koduje dane i obejście **nie jest możliwe**  
- [ ] Tak — framework jest używany, ale mechanizmy pomijania zabezpieczeń (escape hatches), takie jak `dangerouslySetInnerHTML`, są **włączone**  
- [ ] Nie — aplikacja używa czystego JavaScriptu (vanilla JavaScript) lub przestarzałych bibliotek (np. stare wersje jQuery) bez automatycznego kodowania (escaping)

---

## WSTG-CLNT-02 — Testowanie wykonywania skryptów JavaScript

Testowanie wykonywania skryptów JavaScript koncentruje się na identyfikacji przypadków, w których aplikacja niewłaściwie obsługuje dane dostarczone przez użytkownika, co prowadzi do wykonywania nieautoryzowanych skryptów w kontekście przeglądarki ofiary. Ta podatność zazwyczaj skutkuje wystąpieniem Cross-Site Scripting (XSS), co pozwala atakującym na eksfiltrację ciasteczek sesyjnych, manipulację Document Object Model (DOM) lub wykonywanie działań w imieniu użytkownika. Testerzy penetracyjni badają punkty wyjścia (sinks), takie jak `innerHTML`, `document.write()` i `eval()`, w których mogą być przetwarzane niezutylizowane dane pochodzące ze źródeł (sources), takich jak fragmenty URL, `localStorage` lub `window.name`. Skuteczna eksploatacja narusza integralność i poufność sesji użytkownika i może służyć jako punkt wyjścia (pivot) dla bardziej złożonych ataków po stronie klienta (client-side).


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Narzędzia:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### Czy aplikacja przetwarza dane dostarczone przez użytkownika w niebezpiecznych punktach wyjścia (JavaScript sinks)?
- [ ] Nie — wszystkie dane są poddawane sanityzacji lub używane w bezpiecznych punktach wyjścia, takich jak `textContent`  
- [ ] Tak — dane są przetwarzane w niebezpiecznych punktach wyjścia, ale zastosowano sanityzację/kodowanie  
- [ ] Tak — dane są przetwarzane w niebezpiecznych punktach wyjścia, a sanityzacja nie została zastosowana  

### Czy obecna jest polityka Content Security Policy (CSP) w celu ograniczenia wykonywania skryptów?
- [ ] Tak — restrykcyjna polityka CSP jest włączona i jej obejście (bypass) nie jest możliwe  
- [ ] Tak — polityka CSP jest włączona, ale obejście jest możliwe poprzez `unsafe-inline` lub niezabezpieczone sieci CDN  
- [ ] Nie — polityka CSP nie jest włączona  

### Czy skrypty JavaScript mogą być wykonywane poprzez parametry URL lub fragmenty (DOM-based XSS)?
- [ ] Nie — dane wejściowe są prawidłowo zakodowane i wykonanie skryptu nie jest możliwe  
- [ ] Tak — dane wejściowe są odzwierciedlone w DOM, ale wykonanie jest uniemożliwione przez mechanizmy ochronne nowoczesnych frameworków  
- [ ] Tak — dane wejściowe są bezpośrednio wykonywane w przeglądarce, a eksploatacja (Exploit) jest możliwa  

### Czy programy obsługi zdarzeń (event handlers) lub schematy URI są podatne na wstrzykiwanie skryptów (script injection)?
- [ ] Nie — dane wejściowe są poddawane sanityzacji w celu usunięcia atrybutów zdarzeń oraz schematów `javascript:`  
- [ ] Tak — programy obsługi zdarzeń mogą zostać wstrzyknięte, ale filtry ograniczają złożoność ładunku (Payload)  
- [ ] Tak — możliwe jest wykonanie dowolnego kodu JavaScript poprzez atrybuty `onerror`, `onload` lub `href`

---

## WSTG-CLNT-03 — HTML Injection

HTML Injection występuje, gdy aplikacja nie przeprowadza sanityzacji danych wejściowych dostarczonych przez użytkownika przed ich wyrenderowaniem w przeglądarce, co pozwala atakującemu na wstrzyknięcie dowolnych tagów HTML do modelu obiektowego dokumentu (DOM) strony internetowej. Choć podatność ta jest podobna do Cross-Site Scripting (XSS), głównym celem HTML Injection jest manipulacja wizualną prezentacją strony lub ułatwienie ataków typu phishing poprzez wstrzykiwanie złośliwych formularzy i zwodniczych treści. Atakujący celują w parametry, które są odzwierciedlane w UI, takie jak frazy wyszukiwania, pola profilu użytkownika lub komunikaty o błędach, aby skłonić ofiary do ujawnienia poufnych danych uwierzytelniających (credentials) lub interakcji z nieautoryzowanymi linkami zewnętrznymi. Podatność ta stanowi istotne zagrożenie dla integralności interfejsu użytkownika aplikacji i może być wstępem do bardziej złożonych ataków socjotechnicznych lub związanych z sesją.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Niski / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Czy dane wejściowe kontrolowane przez użytkownika są odzwierciedlane w DOM bez odpowiedniego kodowania HTML?
- [ ] Nie — wszystkie odzwierciedlone dane są ściśle kodowane (HTML-encoded) przed wyrenderowaniem  
- [ ] Tak — niektóre znaki **są** kodowane, ale możliwe są **obejścia (bypasses)** dla określonych tagów  
- [ ] Tak — dane wejściowe są odzwierciedlane w formie surowej i HTML injection **jest możliwe**  

### Czy można wstrzyknąć strukturalne elementy HTML w celu zmiany układu strony?
- [ ] Nie — aplikacja filtruje lub usuwa tagi strukturalne, takie jak `<div>`, `<table>` lub `<iframe>`  
- [ ] Tak — elementy strukturalne **mogą** zostać wstrzyknięte, ale **nie mają** znaczącego wpływu na UI  
- [ ] Tak — znaczące zniekształcenie interfejsu (UI defacement) **jest możliwe** poprzez wstrzyknięte tagi  

### Czy możliwe jest wstrzyknięcie funkcjonalnych elementów phishingowych, takich jak formularze lub złośliwe linki?
- [ ] Nie — tagi `<form>`, `<input>` oraz `<a>` są skutecznie blokowane lub poddawane sanityzacji  
- [ ] Tak — złośliwe linki **mogą** zostać wstrzyknięte, aby przekierowywać użytkowników do zewnętrznych witryn  
- [ ] Tak — funkcjonalne elementy `<form>` **mogą** zostać wstrzyknięte w celu wyłudzania poświadczeń *(Średni)*  

### Czy nagłówki bezpieczeństwa lub Content Security Policy (CSP) ograniczają skutki wstrzyknięcia?
- [ ] Tak — wdrożono restrykcyjne CSP, a reguły `form-action` lub `frame-src` **są stosowane**  
- [ ] Nie — nagłówki bezpieczeństwa są obecne, ale CSP **jest wyłączone** lub zawiera wpisy unsafe-inline/wildcards  
- [ ] Nie — żadne CSP ani odpowiednie nagłówki bezpieczeństwa **nie są włączone**

---

## WSTG-CLNT-04 — Testowanie pod kątem przekierowań URL po stronie klienta (Client-Side URL Redirect)

Przekierowanie URL po stronie klienta (Client-Side URL Redirect) występuje, gdy aplikacja webowa przyjmuje dane wejściowe kontrolowane przez użytkownika — zazwyczaj poprzez parametry URL lub fragmenty hash — i wykorzystuje je wewnątrz sinka przekierowań po stronie klienta, takiego jak `window.location` lub `document.location.href`. Podatność ta jest wykorzystywana przez atakujących głównie do przeprowadzania zaawansowanych kampanii phishingowych, ponieważ ofiara początkowo ufa legalnej domenie, zanim zostanie po cichu przekierowana do złośliwej witryny. Poza phishingiem, przekierowania po stronie klienta mogą być często łączone w łańcuchy (chained) w celu eksfiltracji wrażliwych danych, takich jak tokeny dostępu OAuth lub identyfikatory sesji (session identifiers), jeśli przekierowanie następuje, gdy wartości te są obecne w adresie URL lub nagłówku `Referer`. W nowoczesnych aplikacjach typu Single Page Application (SPA), podatność ta często pojawia się w logice routingu, parametrach "return to" po uwierzytelnieniu lub funkcjonalnościach zmiany języka.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Średni* |

> *Poziom istotności staje się Wysoki, jeśli przekierowanie może zostać wykorzystane do eksfiltracji tokenów OAuth, identyfikatorów sesji lub obejścia mechanizmów kontroli bezpieczeństwa w procesie uwierzytelniania.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Narzędzia:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Czy aplikacja używa skryptów po stronie klienta do obsługi przekierowań na podstawie danych wejściowych użytkownika?
- [ ] Nie — przekierowanie jest obsługiwane wyłącznie po stronie serwera za pomocą kodów stanu HTTP `3xx`  
- [ ] Tak — aplikacja używa JavaScript sinks, takich jak `window.location`, do nawigacji na podstawie parametrów URL  
- [ ] Tak — aplikacja używa routera frameworka po stronie klienta (np. React Router, Vue Router), który przetwarza ścieżki dostarczone przez użytkownika  

### Czy zaimplementowano walidację danych wejściowych lub mechanizmy białej listy (whitelisting) dla docelowego adresu URL?
- [ ] Tak — wymuszana jest rygorystyczna **biała lista** (whitelist) dozwolonych domen/ścieżek i obejście **nie jest możliwe**  
- [ ] Tak — walidacja istnieje, ale opiera się na słabych wyrażeniach regularnych (regex) lub czarnych listach (blacklists), a obejście **jest możliwe**  
- [ ] Nie — aplikacja akceptuje dowolny ciąg znaków jako cel przekierowania  

### Czy logikę przekierowania można obejść za pomocą typowych technik zaciemniania (obfuscation)?
- [ ] Nie — mechanizmy kontrolne poprawnie obsługują zakodowane znaki, adresy URL relatywne względem protokołu (`//`) oraz ukośniki wsteczne (backslashes)  
- [ ] Tak — obejście **jest możliwe** przy użyciu kodowania URL lub podwójnego kodowania (double-encoding)  
- [ ] Tak — obejście **jest możliwe** przy użyciu adresów URL relatywnych względem protokołu lub znaku `@` (np. `https://expected.com@attacker.com`)  
- [ ] Tak — obejście **jest możliwe** przy użyciu specyficznych cech przeglądarek (browser quirks) lub wstrzyknięcia bajtu zerowego (null byte injection)  

### Czy proces przekierowania ujawnia wrażliwe dane witrynie docelowej?
- [ ] Nie — żadne wrażliwe dane nie są obecne w adresie URL ani przesyłane w nagłówkach podczas przekierowania  
- [ ] Tak — wrażliwe dane (np. tokeny sesyjne, klucze API) **wyciekają** poprzez nagłówek `Referer` do witryny zewnętrznej  
- [ ] Tak — wrażliwe dane obecne we fragmencie adresu URL (`#`) **są dostępne** dla skryptów witryny docelowej  

### Czy możliwe jest wykonanie kodu JavaScript poprzez sink przekierowania (DOM-based XSS)?
- [ ] Nie — sink pozwala jedynie na nawigację do protokołów `http` lub `https`  
- [ ] Tak — sink przekierowania pozwala na użycie URI `javascript:` lub `data:`, co sprawia, że DOM-based XSS **jest możliwy**

---

## WSTG-CLNT-05 — Testowanie pod kątem CSS Injection

CSS Injection występuje, gdy aplikacja zezwala na wpływ danych wejściowych użytkownika na kaskadowe arkusze stylów (CSS) bez odpowiedniej sanityzacji lub escapowania. Choć często postrzegane jako problem kosmetyczny, atakujący mogą wykorzystać CSS Injection do eksfiltracji wrażliwych danych, takich jak tokeny CSRF lub identyfikatory sesji, używając selektorów atrybutów i właściwości background-image do wywoływania żądań zewnętrznych. Luka ta zazwyczaj występuje w punktach końcowych (API), gdzie preferencje użytkownika (np. niestandardowe motywy, kolory czcionek) są odzwierciedlane wewnątrz bloków `<style>` lub atrybutów `style` inline. Z perspektywy atakującego, udany Exploit może prowadzić do UI redressing, phishingu poprzez nieautoryzowaną modyfikację układu strony lub dyskretnej eksfiltracji danych w środowiskach, w których rygorystyczne polityki Content Security Policy (CSP) mogłyby w innym przypadku blokować ataki oparte na JavaScript.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się wysoki (High), jeśli punkt iniekcji pozwala na eksfiltrację wrażliwych atrybutów, takich jak tokeny CSRF, lub jeśli może zostać wykorzystany do UI redressing w krytycznych procesach.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Czy aplikacja odzwierciedla dane wejściowe kontrolowane przez użytkownika w kontekstach CSS?
- [ ] Nie — dane wejściowe **nie są** odzwierciedlane w tagach style, atrybutach ani plikach CSS  
- [ ] Tak — dane są odzwierciedlane, ale ściśle ograniczone do bezpiecznych wartości alfanumerycznych  
- [ ] Tak — dane są odzwierciedlane wewnątrz atrybutu `style` lub bloku `<style>`  

### Czy metaznaki i funkcje specyficzne dla CSS są odpowiednio poddawane sanityzacji?
- [ ] Tak — znaki takie jak `{`, `}`, `:` oraz funkcje takie jak `url()` są **poprawnie escapowane**  
- [ ] Tak — sanityzacja jest wdrożona, ale możliwe jest jej obejście (bypass) poprzez kodowanie lub komentarze CSS  
- [ ] Nie — żadna sanityzacja nie jest stosowana wobec metaznaków CSS  

### Czy możliwa jest eksfiltracja danych przy użyciu selektorów CSS?
- [ ] Nie — selektory **nie mogą** wyzwalać żądań zewnętrznych ani powodować wycieku danych  
- [ ] Tak — częściowa eksfiltracja danych **jest możliwa** poprzez selektory atrybutów i `background-image`  
- [ ] Tak — pełna eksfiltracja wrażliwych tokenów **jest możliwa** przy użyciu zautomatyzowanych technik Brute Force  

### Czy polityka Content Security Policy (CSP) mityguje ryzyko CSS Injection?
- [ ] Tak — dyrektywa `style-src` jest ograniczona do 'self', a `img-src` lub `connect-src` **są ograniczone**  
- [ ] Tak — CSP istnieje, ale używa 'unsafe-inline' lub zezwala na domeny zewnętrzne poprzez symbole wieloznaczne  
- [ ] Nie — nie wdrożono CSP w celu zapobiegania ładowaniu zewnętrznych zasobów przez CSS  

### Czy podatność może zostać wykorzystana do przeprowadzenia UI redressing lub phishingu?
- [ ] Nie — zakres iniekcji jest zbyt ograniczony, aby znacząco zmodyfikować układ strony  
- [ ] Tak — modyfikacja interfejsu użytkownika (UI) **jest możliwa**, co pozwala na nakładanie złośliwych elementów  
- [ ] Tak — możliwa jest pełna kontrola nad układem strony, co ułatwia przeprowadzenie wysoce przekonujących ataków phishingowych

---

## WSTG-CLNT-06 — Testowanie pod kątem manipulacji zasobami po stronie klienta

Manipulacja zasobami po stronie klienta (Client-side resource manipulation) występuje, gdy aplikacja pozwala na kontrolowane przez użytkownika dane wejściowe w celu wpływania na źródło lub ścieżkę zasobów ładowanych przez przeglądarkę, takich jak skrypty, arkusze stylów lub ramki iframe. Poprzez manipulację fragmentami URI, parametrami zapytań (query parameters) lub innymi źródłami DOM, atakujący może wymusić na aplikacji załadowanie złośliwej zawartości zewnętrznej lub wykonanie nieautoryzowanego kodu. Podatność ta jest zazwyczaj spotykana w logice JavaScript, która dynamicznie uzupełnia atrybuty `src` lub `href` elementów HTML bez wystarczającej walidacji względem zaufanej białej listy (allow-list). Z perspektywy atakującego, podatność ta jest wykorzystywana poprzez spreparowanie adresu URL, który przekierowuje ładowanie zasobów na serwer kontrolowany przez atakującego, co potencjalnie skutkuje atakiem DOM-based Cross-Site Scripting (XSS), eksfiltracją wrażliwych danych lub manipulacją interfejsem użytkownika (UI redressing).


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Średni / Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Narzędzia:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### Czy aplikacja używa odbiorników (sinks) po stronie klienta do dynamicznego ładowania lub osadzania zasobów?
- [ ] Nie — zasoby są ładowane wyłącznie poprzez statyczny kod HTML lub logikę po stronie serwera  
- [ ] Tak — JavaScript po stronie klienta dynamicznie uzupełnia atrybuty zasobów, takie jak `src`, `href` lub `data`  

### Czy zaimplementowano mechanizmy walidacji lub białe listy (allow-lists) dla źródeł URI zasobów?
- [ ] Tak — rygorystyczne białe listy (allow-lists) i walidacja pochodzenia (origin validation) **są stosowane** do wszystkich dynamicznych zasobów  
- [ ] Tak — walidacja istnieje, ale opiera się na słabych czarnych listach (blacklists) lub łatwych do obejścia wyrażeniach regularnych (regex)  
- [ ] Nie — do kontrolowanych przez użytkownika ścieżek zasobów nie są stosowane żadne mechanizmy walidacji  

### Czy możliwe jest obejście walidacji URI przy użyciu alternatywnych protokołów lub kodowania?
- [ ] Nie — obejście filtrów zasobów **nie jest możliwe**  
- [ ] Tak — obejście **jest możliwe** przy użyciu innych protokołów, takich jak `data:`, `vbscript:` lub `javascript:`  
- [ ] Tak — obejście **jest możliwe** przy użyciu zakodowanych znaków lub bajtów zerowych (null bytes) w celu ominięcia prostego dopasowania ciągów znaków  

### Czy atakujący może przekierować ładowanie zasobów do zewnętrznej, niezaufanej domeny?
- [ ] Nie — ścieżki zasobów są ograniczone do tego samego pochodzenia (same origin) lub stałego podkatalogu  
- [ ] Tak — aplikacja **może** zostać zmuszona do ładowania skryptów lub stylów z dowolnej domeny zewnętrznej  

### Jaki jest maksymalny wpływ manipulacji zasobami?
- [ ] Niski — manipulacja wpływa tylko na elementy niewykonawcze, takie jak obrazy lub niewrażliwy tekst  
- [ ] Średni — manipulacja pozwala na wstrzykiwanie CSS (CSS injection) lub znaczące przekształcenie interfejsu (UI redressing)  
- [ ] Wysoki — manipulacja prowadzi do ataku DOM-based XSS lub wykonania dowolnego kodu JavaScript *(Krytyczny)*

---

## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) jest mechanizmem przeglądarkowym, który pozwala serwerowi na jawne zezwolenie na dostęp do swoich zasobów z różnych źródeł (origins), skutecznie rozluźniając politykę Same-Origin Policy (SOP). Błędne konfiguracje występują, gdy aplikacja nadmiernie ufa dowolnym źródłom, często poprzez odzwierciedlanie nagłówka `Origin` w nagłówku odpowiedzi `Access-Control-Allow-Origin` lub poprzez stosowanie źle zaimplementowanych białych list opartych na wyrażeniach regularnych (regex). Z perspektywy atakującego, permisywna polityka CORS może zostać wykorzystana poprzez umieszczenie złośliwego skryptu na kontrolowanej domenie, który wykonuje uwierzytelnione żądania do podatnej aplikacji, co pozwala atakującemu na eksfiltrację wrażliwych danych, takich jak tokeny CSRF lub PII, bezpośrednio z treści odpowiedzi. Podatność ta jest najbardziej krytyczna w przypadku punktów końcowych API, które przetwarzają uwierzytelnione sesje i zwracają niepubliczne informacje.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Średni / Wysoki |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Narzędzia:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Czy aplikacja implementuje nagłówki CORS na uwierzytelnionych punktach końcowych?
- [ ] Nie — CORS **nie jest włączony**; przeglądarka domyślnie stosuje rygorystyczną politykę Same-Origin Policy  
- [ ] Tak — nagłówki CORS są obecne i ograniczone do rygorystycznej **białej listy** (whitelist)  
- [ ] Tak — nagłówki CORS są obecne, ale skonfigurowane w sposób **permisywny**  

### W jaki sposób serwer obsługuje nagłówek `Access-Control-Allow-Origin` (ACAO)?
- [ ] ACAO jest ustawiony na konkretną, **zaufaną** domenę statyczną  
- [ ] ACAO jest ustawiony na `*` (wildcard) i poświadczenia (credentials) **nie mogą** być przesyłane  
- [ ] ACAO jest ustawiony na `*` (wildcard) i poświadczenia **mogą** być przesyłane *(Uwaga: większość przeglądarek blokuje taką konfigurację)*  
- [ ] ACAO **dynamicznie odzwierciedla** wartość nagłówka `Origin` z żądania  

### Czy nagłówek `Access-Control-Allow-Credentials` (ACAC) jest ustawiony na `true`?
- [ ] Nie — ACAC **nie występuje** lub jest ustawiony na `false`  
- [ ] Tak — ACAC jest ustawiony na `true`, ale ACAO jest **ograniczone** do zaufanych źródeł  
- [ ] Tak — ACAC jest ustawiony na `true`, a ACAO **odzwierciedla** dowolne źródła *(Krytyczne)*  

### Czy biała lista źródeł może zostać pominięta poprzez manipulację subdomeną lub źródłem null?
- [ ] Nie — biała lista jest rygorystycznie egzekwowana i **nie może** zostać pominięta  
- [ ] Tak — biała lista akceptuje **dowolną** subdomenę celu (np. `attacker.target.com`)  
- [ ] Tak — biała lista akceptuje źródło `null` poprzez sandboksowane ramki iframe  
- [ ] Tak — biała lista jest podatna na obejścia wyrażeń regularnych (np. `target.com.attacker.com` lub `target.com-safe.com`)  

### Czy konfiguracja CORS pozwala na eksfiltrację wrażliwych danych?
- [ ] Nie — wystawione punkty końcowe **nie zawierają** wrażliwych danych ani danych specyficznych dla sesji  
- [ ] Tak — uwierzytelnione punkty końcowe zwracają PII, poświadczenia lub tokeny CSRF, które **mogą** zostać odczytane przez nieuprawnione źródło

---

## WSTG-CLNT-08 — Testowanie pod kątem Cross Site Flashing

Cross-Site Flashing (XSF) to podatność po stronie klienta, występująca, gdy plik Flash (SWF) niewłaściwie przetwarza dane wejściowe kontrolowane przez użytkownika, umożliwiając atakującemu wykonanie złośliwego kodu ActionScript lub przedostanie się do środowiska JavaScript przeglądarki. Poprzez manipulację parametrami takimi jak `FlashVars` lub ciągi zapytań URL przekazywane do ujść danych (sinks) takich jak `ExternalInterface.call`, `getURL` lub `loadMovie`, atakujący może wykonywać działania w kontekście podatnej domeny, w tym przejmowanie sesji (session hijacking) i eksfiltrację danych. Podatność ta występuje głównie w starszych aplikacjach korporacyjnych (legacy), które nadal hostują pliki SWF, gdzie niewystarczająca walidacja danych wejściowych pozwala na wykorzystanie filmu Flash jako wektora dla Cross-Site Scripting (XSS) lub na obejście polityki Same-Origin Policy (SOP) poprzez zbyt liberalne konfiguracje między-domenowe (cross-domain).


| Pole | Wartość |
|---|---|
| **ID WSTG** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Średni / Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Czy w aplikacji hostowane są starsze pliki Adobe Flash (.swf)?
- [ ] Nie — żadne pliki Flash nie są hostowane ani przywoływane w zakresie aplikacji  
- [ ] Tak — pliki SWF są obecne, ale serwują wyłącznie treści statyczne  
- [ ] Tak — pliki SWF są obecne i przyjmują parametry dynamiczne poprzez `FlashVars` lub ciągi URL  

### Czy dane wejściowe przekazywane do ujść danych (sinks) ActionScript są oczyszczane?
- [ ] Tak — wszystkie dane wejściowe są rygorystycznie walidowane, a manipulacja ujściami ActionScript **nie jest możliwa**  
- [ ] Tak — walidacja jest stosowana, ale jej obejście **jest możliwe** za pomocą specyficznych technik kodowania  
- [ ] Nie — dane wejściowe są przekazywane bezpośrednio do wrażliwych ujść, takich jak `ExternalInterface.call` lub `getURL` *(Krytyczne)*  

### Czy plik Flash może zostać użyty do wykonania dowolnego kodu JavaScript (XSS)?
- [ ] Nie — `ExternalInterface` jest **wyłączone** lub parametr `allowScriptAccess` jest ustawiony na `never`  
- [ ] Tak — parametr `allowScriptAccess` jest ustawiony na `sameDomain`, ale plik SWF jest hostowany w docelowej domenie  
- [ ] Tak — parametr `allowScriptAccess` jest ustawiony na `always`, co umożliwia XSS z dowolnej domeny  

### Czy polityka `crossdomain.xml` zapobiega nieautoryzowanym żądaniom cross-origin?
- [ ] Tak — plik `crossdomain.xml` jest restrykcyjny i zezwala tylko na zaufane, konkretne źródła (origins)  
- [ ] Nie — plik `crossdomain.xml` **nie istnieje**  
- [ ] Tak — polityka jest zbyt liberalna (np. `<allow-access-from domain="*" />`)  

### Czy można wymusić na pliku SWF załadowanie zewnętrznych filmów kontrolowanych przez atakującego?
- [ ] Nie — nie można wymusić na aplikacji ładowania zewnętrznych plików SWF  
- [ ] Tak — funkcje `loadMovie` lub `loadMovieNum` przyjmują niewalidowane zewnętrzne adresy URL

---

## WSTG-CLNT-09 — Testowanie pod kątem Clickjackingu

Clickjacking, znany również jako UI redressing, występuje, gdy atakujący wykorzystuje przezroczyste lub nieprzezroczyste warstwy, aby nakłonić użytkownika do kliknięcia w przycisk lub link na innej stronie, podczas gdy użytkownik zamierzał kliknąć w stronę najwyższego poziomu. Luka ta narusza integralność działań użytkownika, co potencjalnie prowadzi do nieautoryzowanej modyfikacji danych, zmian na koncie lub transferów finansowych wykonywanych w imieniu ofiary. Atakujący zazwyczaj wykorzystują tę podatność poprzez osadzenie docelowej strony internetowej wewnątrz elementu `<iframe>` na kontrolowanej, złośliwej stronie i nałożenie na nią zwodniczej treści. Występuje to najczęściej na stronach wykonujących wrażliwe akcje zmieniające stan (state-changing actions), takie jak przyciski „Usuń konto”, formularze resetowania hasła lub panele konfiguracji administracyjnej.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Status testu** | Not Performed |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się Wysoki, jeśli za pomocą Clickjackingu można wywołać wrażliwe akcje (np. transfery środków, usunięcie konta lub Privilege Escalation).

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Narzędzia:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Czy aplikacja implementuje nagłówki po stronie serwera w celu zapobiegania nieautoryzowanemu ramkowaniu (framing)?
- [ ] Tak — `X-Frame-Options` lub `Content-Security-Policy` **są stosowane** i zapobiegają ramkowaniu *(Najbezpieczniej)*  
- [ ] Tak — nagłówki są obecne, ale **błędnie skonfigurowane** (np. `X-Frame-Options: ALLOW-FROM`, który nie posiada szerokiego wsparcia w przeglądarkach)  
- [ ] Nie — żadne nagłówki chroniące przed ramkowaniem **nie są stosowane**  

### Czy dyrektywa `frame-ancestors` w `Content-Security-Policy` (CSP) jest zaimplementowana?
- [ ] Tak — `frame-ancestors 'none'` lub `'self'` **jest stosowana**  
- [ ] Tak — dyrektywa `frame-ancestors` jest obecna, ale **zezwala** na niezaufane lub zbyt szerokie źródła (origins)  
- [ ] Nie — dyrektywa jest **nieobecna** lub CSP nie zostało zaimplementowane  

### Czy nagłówek `X-Frame-Options` (XFO) jest poprawnie skonfigurowany pod kątem wsparcia dla starszych przeglądarek (legacy)?
- [ ] Tak — `DENY` lub `SAMEORIGIN` **jest stosowane**  
- [ ] Tak — XFO jest obecny, ale **ignorowany** przez nowoczesne przeglądarki, ponieważ istnieje dyrektywa CSP `frame-ancestors`  
- [ ] Nie — nagłówek XFO jest **nieobecny** lub ustawiony na niebezpieczną wartość  

### Czy zabezpieczenia przed ramkowaniem można obejść za pomocą znanych technik?
- [ ] Nie — obejście **nie jest możliwe** przy użyciu standardowych technik (double-framing itp.)  
- [ ] Tak — zabezpieczenie **może** zostać obejścięte z powodu słabego wyrażenia regularnego (regex) lub logiki w białej liście `frame-ancestors`  
- [ ] Tak — zabezpieczenie **może** zostać obejścięte, ponieważ aplikacja polega wyłącznie na technice frame-busting opartej na JavaScript  

### Czy wrażliwa akcja jest podatna na eksploatację poprzez Proof-of-Concept Clickjackingu?
- [ ] Nie — ramkowanie jest blokowane lub żadne wrażliwe akcje nie są dostępne  
- [ ] Tak — UI redressing **jest możliwy**, ale wymaga złożonej interakcji użytkownika  
- [ ] Tak — UI redressing **jest możliwy** i pozwala na natychmiastowe akcje zmieniające stan *(Krytyczne)*

---

## WSTG-CLNT-10 — Testowanie WebSockets

Testowanie WebSockets koncentruje się na pełnodupleksowym kanale komunikacyjnym ustanowionym między klientem a serwerem, który pomija tradycyjne wzorce żądanie-odpowiedź protokołu HTTP. Pentesterzy oceniają bezpieczeństwo procesu Handshake, obecność mechanizmów ochrony przed Cross-Site WebSocket Hijacking (CSWSH) oraz rygorystyczność walidacji danych wejściowych (Input Validation) stosowanej wobec Payloadów wiadomości. Eksploatacja (Exploitation) zazwyczaj polega na przejęciu aktywnej sesji w celu eksfiltracji danych w czasie rzeczywistym lub wstrzykiwaniu złośliwych Payloadów, które wyzwalają luki po stronie serwera, takie jak Command Injection lub SQLi poprzez trwałe gniazdo (socket). Ponieważ wiele tradycyjnych zapór aplikacji internetowych (WAF) nie inspekcjonuje skutecznie ruchu WebSocket, protokół ten często stanowi dyskretny wektor do omijania zabezpieczeń obwodowych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Wysoki / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Narzędzia:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### Czy połączenie WebSocket jest ustanawiane przez bezpieczny kanał?
- [ ] Tak — `wss://` (WebSocket Secure) jest wymuszane dla wszystkich połączeń  
- [ ] Nie — używane jest `ws://`, co pozwala na podsłuch typu person-in-the-middle (PITM)  

### Czy aplikacja jest chroniona przed Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] Nie — serwer rygorystycznie waliduje nagłówek `Origin` podczas procesu Handshake  
- [ ] Tak — walidacja nagłówka `Origin` jest słaba lub **może** zostać pominięta poprzez wartości null lub sfałszowane źródła  
- [ ] Tak — nie jest wykonywana żadna walidacja nagłówka `Origin`, co pozwala atakującym cross-site na zainicjowanie połączenia *(Krytyczne)*  

### Czy walidacja i sanityzacja danych wejściowych (Input Validation/Sanitization) są stosowane wobec Payloadów wiadomości WebSocket?
- [ ] Tak — wszystkie wiadomości przychodzące są poddawane sanityzacji i walidowane względem schematu  
- [ ] Tak — istnieje pewna walidacja, ale jej obejście (bypass) **jest możliwe** przy użyciu zakodowanych lub niestandardowych Payloadów  
- [ ] Nie — wiadomości są przetwarzane bezpośrednio, co pozwala na ataki typu Injection (XSS, SQLi, RCE) lub manipulację logiką  

### Czy kontrole uwierzytelniania (Authentication) i autoryzacji (Authorization) są wykonywane przy każdej wiadomości WebSocket?
- [ ] Tak — sesja jest weryfikowana dla każdej wiadomości lub protokół jest bezstanowy (stateless) z użyciem tokenów  
- [ ] Tak — uwierzytelnianie odbywa się podczas Handshake, ale autoryzacja dla konkretnych działań **nie jest** wymuszana dla każdej wiadomości  
- [ ] Nie — uwierzytelnianie jest sprawdzane tylko podczas Handshake, co pozwala na przejęcie sesji w celu uzyskania pełnego dostępu  

### Czy serwer implementuje Rate Limiting lub ograniczenia zasobów dla protokołu WebSocket?
- [ ] Tak — Rate Limiting i maksymalne rozmiary wiadomości są **włączone** i wymuszane  
- [ ] Nie — nie istnieją żadne limity, co pozwala na atak Denial of Service (DoS) poprzez zalewanie wiadomościami (message flooding) lub duże rozmiary Payloadów

---

## WSTG-CLNT-11 — Test Web Messaging

Web Messaging, lub `postMessage`, umożliwia komunikację między różnymi pochodzeniami (cross-origin) pomiędzy obiektami okien, takimi jak strona nadrzędna a wywołane okno popup lub osadzona ramka iframe. Mechanizm ten jest kluczowy dla funkcjonowania nowoczesnych stron internetowych, ale wprowadza istotne ryzyko, jeśli aplikacja odbierająca komunikat nie zweryfikuje tożsamości nadawcy lub niewłaściwie obsłuży ładunek (payload) wiadomości. Atakujący wykorzystują te podatności, hostując złośliwą stronę, która osadza docelową aplikację i wysyła spreparowane komunikaty w celu wywołania niepożądanych działań, eksfiltracji wrażliwych danych lub wykonania ataku DOM-based Cross-Site Scripting (XSS). Skuteczna eksploatacja występuje, gdy procedury obsługi komunikatów (message listeners) nie walidują rygorystycznie właściwości `origin` lub przekazują dane `message.data` bezpośrednio do niebezpiecznych ujść (execution sinks).

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności (Severity)** | Średni / Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Narzędzia:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### Czy aplikacja wykorzystuje API `postMessage` do komunikacji między dokumentami?
- [ ] Nie — procedury obsługi (listeners) `postMessage` **nie** są obecne w kodzie po stronie klienta  
- [ ] Tak — `postMessage` jest używany do wewnętrznej komunikacji między komponentami  
- [ ] Tak — `postMessage` jest używany do komunikacji z domenami zewnętrznymi (third-party) lub widgetami  

### Czy pochodzenie (origin) komunikatów przychodzących jest ściśle walidowane?
- [ ] Tak — origin jest sprawdzany względem ścisłej białej listy (whitelist) przy użyciu operatora `===` lub porównania identycznego *(Najbezpieczniejsze)*  
- [ ] Tak — origin jest walidowany za pomocą wyrażenia regularnego (regex), ale logika **jest** podatna na obejście (np. `^https://trusted.com`)  
- [ ] Nie — właściwość `origin` **nie** jest sprawdzana, co pozwala dowolnej domenie na wysyłanie komunikatów  
- [ ] Nie — użyto symbolu wieloznacznego (wildcard) `*` w `targetOrigin` nadawcy, co potencjalnie ujawnia dane złośliwym odbiorcom  

### Czy ładunek (payload) komunikatu jest sanityzowany przed przetworzeniem przez aplikację?
- [ ] Tak — dane są traktowane jako zwykły tekst i nie są używane żadne niebezpieczne ujścia (sinks)  
- [ ] Tak — dane są parsowane (np. `JSON.parse`) i walidowane przed użyciem, a obejście walidacji **nie jest możliwe**  
- [ ] Nie — dane komunikatu są przekazywane bezpośrednio do niebezpiecznych ujść (sinks), takich jak `innerHTML`, `eval()` lub `setTimeout()`  
- [ ] Nie — dane komunikatu są używane do nawigacji w oknie lub zmiany `location.href` bez uprzedniej walidacji  

### Czy atakujący może wywołać nieautoryzowane działania lub dokonać eksfiltracji danych poprzez złośliwy komunikat?
- [ ] Nie — nawet przy sfałszowanym komunikacie nie można uzyskać dostępu do wrażliwych działań ani danych  
- [ ] Tak — spreparowany komunikat **może** wywołać zmiany stanu wrażliwych danych (np. zmiana hasła, aktualizacja profilu)  
- [ ] Tak — spreparowany komunikat **może** skutkować atakiem DOM-based XSS lub eksfiltracją tokenów CSRF/danych sesyjnych

---

## WSTG-CLNT-12 — Testowanie Browser Storage

Testowanie pamięci masowej przeglądarki (Browser Storage) obejmuje analizę sposobu, w jaki aplikacja wykorzystuje mechanizmy po stronie klienta, takie jak LocalStorage, SessionStorage, IndexedDB oraz WebSQL do utrwalania danych. Niezabezpieczone, wrażliwe informacje — w tym PII, tokeny uwierzytelniające lub identyfikatory sesji — mogą zostać przejęte, jeśli atakujący zdoła przeprowadzić atak Cross-Site Scripting (XSS) lub uzyska fizyczny dostęp do urządzenia użytkownika. Pentesterzy muszą ustalić, czy dane są przechowywane otwartym tekstem (cleartext), czy pozostają dostępne po wylogowaniu i czy cykl życia pamięci masowej jest odpowiednio zarządzany, aby zapobiec wyciekowi danych. Z perspektywy atakującego, mechanizmy te są atrakcyjnym celem do eksfiltracji informacji utrzymujących stan, co pozwala na przejęcie sesji (Session Hijacking) lub długotrwałe utrzymanie dostępu do konta.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się Wysoki, jeśli tokeny sesyjne lub wrażliwe dane PII są przechowywane w LocalStorage i są dostępne poprzez XSS.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### Czy aplikacja przechowuje wrażliwe informacje w pamięci przeglądarki?
- [ ] Nie — aplikacja **nie używa** pamięci przeglądarki lub przechowuje jedynie niewrażliwe stany interfejsu użytkownika (UI)  
- [ ] Tak — wrażliwe dane są przechowywane, ale **są szyfrowane** przy użyciu silnej kryptografii po stronie klienta  
- [ ] Tak — wrażliwe dane (tokeny, PII, sekrety) są przechowywane **otwartym tekstem** (cleartext) *(Średni)*  

### Czy zapisane dane są dostępne dla nieautoryzowanych skryptów (ryzyko XSS)?
- [ ] Nie — dane są przechowywane w ciasteczkach HttpOnly (nie w Browser Storage) lub pamięć masowa **nie jest wykorzystywana**  
- [ ] Tak — używany jest LocalStorage/SessionStorage, co sprawia, że dane są **całkowicie dostępne** poprzez dowolny Payload XSS *(Wysoki)*  

### Czy pamięć przeglądarki jest odpowiednio czyszczona po wylogowaniu użytkownika?
- [ ] Tak — cała pamięć powiązana z aplikacją jest **jawnie czyszczona** podczas procesu wylogowywania  
- [ ] Nie — pamięć pozostaje po wylogowaniu, ale **nie zawiera** wrażliwych danych sesji  
- [ ] Nie — tokeny uwierzytelniające lub PII **pozostają** w pamięci po zakończeniu sesji *(Średni)*  

### Czy aplikacja używa IndexedDB lub WebSQL dla dużych/wrażliwych zbiorów danych?
- [ ] Nie — te mechanizmy pamięci **nie są używane**  
- [ ] Tak — dane są przechowywane i **odpowiednio oczyszczane** (sanitized) przed wyrenderowaniem w DOM  
- [ ] Tak — wrażliwe zbiory danych są przechowywane **otwartym tekstem** (cleartext) w strukturach IndexedDB/WebSQL  

### Czy integralność przechowywanych danych może zostać zmanipulowana w celu wpłynięcia na logikę aplikacji?
- [ ] Nie — aplikacja waliduje lub podpisuje dane przed ich przetworzeniem z pamięci  
- [ ] Tak — modyfikacja wartości w pamięci (np. role użytkownika, flagi) **jest możliwa** i skutkuje zmianą zachowania aplikacji

---

## WSTG-CLNT-13 — Testowanie pod kątem Cross-Site Script Inclusion (XSSI)

Cross-Site Script Inclusion (XSSI) występuje, gdy aplikacja internetowa eksportuje wrażliwe dane wewnątrz dynamicznie generowanego pliku JavaScript lub odpowiedzi JSONP, które strona kontrolowana przez atakującego może następnie dołączyć za pomocą tagu `<script>`. Ponieważ dołączanie skryptów (script inclusion) omija politykę Same-Origin Policy (SOP), atakujący może wykonać skrypt we własnym kontekście i nadpisać obiekty globalne lub zmienne w celu eksfiltracji wrażliwych informacji. Podatność ta zazwyczaj objawia się w uwierzytelnionych punktach końcowych (endpoints), które zwracają dane specyficzne dla użytkownika, takie jak adresy e-mail, klucze API lub metadane sesji w formacie skryptu. Z perspektywy atakującego, eksploatacja polega na przygotowaniu złośliwej strony, która odwołuje się do podatnego skryptu i przechwytuje wynikowe zmiany zmiennych globalnych lub wywołania funkcji.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Średni / Wysoki |

> *Poziom zagrożenia staje się wysoki, jeśli wyciekłe dane obejmują tokeny CSRF, identyfikatory sesji lub wysoce wrażliwe dane osobowe (PII).

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Czy jakiekolwiek uwierzytelnione punkty końcowe zwracają dynamiczny JavaScript lub JSONP zawierający wrażliwe informacje?
- [ ] Nie — wszystkie skrypty są statyczne i **nie mogą** zawierać danych specyficznych dla użytkownika  
- [ ] Tak — istnieją dynamiczne skrypty, ale **nie zawierają** one wrażliwych informacji  
- [ ] Tak — dynamiczne skrypty zawierają dane PII lub tokeny i **są** dostępne między domenami (across origins)  

### Czy nagłówek `X-Content-Type-Options: nosniff` jest zaimplementowany w odpowiedziach zawierających wrażliwe skrypty?
- [ ] Tak — nagłówek jest **stosowany** i zapobiega sniffingowi typów MIME (MIME-type sniffing)  
- [ ] Nie — brakuje nagłówka, co potencjalnie pozwala na wykonanie treści niebędącej skryptem jako skrypt  

### Czy wrażliwe skrypty są chronione przez mechanizmy anty-XSSI, takie jak prefiksy niewykonywalne lub niestandardowe nagłówki?
- [ ] Tak — skrypty wymagają niestandardowego nagłówka lub tokena, którego **nie można** przesłać za pomocą standardowego tagu `<script>`  
- [ ] Tak — skrypty używają prefiksów niewykonywalnych (np. `)]}'`), których **nie można** łatwo ominąć  
- [ ] Nie — skrypty są bezpośrednio dostępne za pomocą żądań GET przy użyciu poświadczeń otoczenia (ambient credentials) *(Krytyczne)*  

### Czy możliwa jest eksfiltracja wrażliwych danych poprzez nadpisywanie zmiennych globalnych lub prototypów?
- [ ] Nie — wrażliwe dane mają ograniczony zasięg (scope) lub są chronione przez nowoczesne funkcje przeglądarki  
- [ ] Tak — wrażliwe dane są przypisane do zmiennych globalnych i **mogą** zostać przechwycone  
- [ ] Tak — dane wyciekają poprzez wywołania zwrotne JSONP (callbacks) które **mogą** zostać przechwycone

---

## WSTG-CLNT-14 — Testowanie pod kątem Reverse Tabnabbing

Reverse Tabnabbing to podatność po stronie klienta, w której strona otwierana za pomocą `target="_blank"` zachowuje funkcjonalne odniesienie do strony nadrzędnej poprzez obiekt `window.opener`. Pozwala to kontrolowanej przez atakującego witrynie docelowej na programowe przekierowanie oryginalnej, zaufanej karty do złośliwego adresu URL, zazwyczaj strony phishingowej zaprojektowanej w celu wykradania danych uwierzytelniających. Atak wykorzystuje naturalne zaufanie użytkownika do oryginalnej karty, ponieważ prawdopodobieństwo zauważenia, że strona, którą właśnie opuścił, przeszła do innej domeny, jest niskie. Nowoczesne praktyki bezpieczeństwa wymagają stosowania atrybutów `rel="noopener"` lub `rel="noreferrer"` w znacznikach anchor (kotwicach) lub ustawienia właściwości `opener` na null w JavaScript, aby zapobiec tej komunikacji międzyokiennej.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Niski / Średni* |

> *Poziom zagrożenia jest Niski w większości nowoczesnych przeglądarek, ponieważ obecnie domyślnie ustawiają one `target="_blank"` na `rel="noopener"`. Poziom zagrożenia staje się Średni, jeśli aplikacja jest skierowana do starszych przeglądarek lub używa `window.open()` bez ustawienia właściwości `opener` na null.

**Odnośniki:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Czy występują linki, które otwierają się w nowym oknie lub karcie?
- [ ] Nie — żadne linki nie używają `target="_blank"` ani równoważnego przekierowania opartego na JavaScript  
- [ ] Tak — `target="_blank"` jest używane wyłącznie w wewnętrznych, zaufanych linkach  
- [ ] Tak — `target="_blank"` jest używane w linkach zewnętrznych lub adresach URL dostarczonych przez użytkownika  

### Czy relacja `window.opener` jest ograniczona w znacznikach anchor?
- [ ] Tak — `rel="noopener"` lub `rel="noreferrer"` jest **konsekwentnie stosowane** do wszystkich linków z `target="_blank"` *(Najbezpieczniej)*  
- [ ] Tak — atrybuty są stosowane, ale **brakuje ich** w linkach o wysokim ryzyku lub generowanych przez użytkownika  
- [ ] Nie — atrybuty **nie są stosowane**, co pozwala stronie podrzędnej na dostęp do `window.opener`  

### Czy aplikacja jest podatna na Reverse Tabnabbing poprzez JavaScriptowe `window.open()`?
- [ ] Nie — `window.open()` nie jest używane lub jawnie ustawia właściwość `opener` na null  
- [ ] Tak — `window.open()` jest używane, a odniesienie `opener` **pozostaje dostępne** dla okna podrzędnego  

### Czy atakujący może kontrolować cel linku, który otwiera się w nowej karcie?
- [ ] Nie — wszystkie cele linków są wpisane na sztywno i wskazują na zaufane domeny  
- [ ] Tak — cele linków są dostarczane przez użytkownika (np. profile w mediach społecznościowych, linki w biogramach) i **nie są** sanitizowane pod kątem zabezpieczeń przed tabnabbingiem

---

## WSTG-APIT-01 — Rekonesans API

Rekonesans API to systematyczny proces identyfikacji i mapowania powierzchni ataku API w celu odkrycia punktów końcowych (endpoints), obsługiwanych metod oraz podstawowych struktur danych. Atakujący przeprowadzają tę fazę, aby odkryć nieudokumentowane (shadow) API, przestarzałe wersje z podatnościami typu legacy oraz publicznie dostępne pliki dokumentacji, takie jak definicje Swagger lub OpenAPI, które ujawniają wewnętrzną logikę biznesową. Poprzez analizę przewidywalnych wzorców URL, plików JavaScript po stronie klienta oraz wyników brute-force katalogów, przeciwnik może zbudować kompleksową mapę API, aby zidentyfikować cele dla ataków typu Broken Object Level Authorization (BOLA) lub Mass Assignment. Rekonesans ten zazwyczaj celuje w powszechne ścieżki, takie jak `/api/v1/`, `/swagger.json` lub `/graphql`, aby uzyskać mapę drogową funkcjonalności backendu aplikacji.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Status testu** | Nie przeprowadzono |
| **Poziom zagrożenia** | Informacyjny / Niski |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Narzędzia:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Czy pliki dokumentacji API (Swagger, OpenAPI, WSDL) są publicznie dostępne?
- [ ] Nie — dokumentacja API nie jest dostępna lub dostęp do niej jest ograniczony przez uwierzytelnianie  
- [ ] Tak — dokumentacja jest dostępna, ale wymaga prawidłowych danych uwierzytelniających  
- [ ] Tak — dokumentacja API (np. `/swagger-ui.html`) jest **publicznie dostępna** bez uwierzytelniania  

### Czy za pomocą fuzzingu można odkryć nieudokumentowane lub „ukryte” (shadow) punkty końcowe API?
- [ ] Nie — fuzzing katalogów i punktów końcowych nie ujawnił żadnych nieudokumentowanych ścieżek  
- [ ] Tak — znaleziono nieudokumentowane punkty końcowe, ale **nie można** uzyskać do nich dostępu bez autoryzacji  
- [ ] Tak — znaleziono nieudokumentowane punkty końcowe i **można** uzyskać do nich dostęp bez prawidłowej autoryzacji  

### Czy aplikacja ujawnia wiele wersji API (np. /v1/, /v2/, /beta/)?
- [ ] Nie — dostępna jest tylko bieżąca, zabezpieczona wersja API  
- [ ] Tak — istnieją starsze wersje, ale stosują te **same** mechanizmy kontroli bezpieczeństwa co wersja bieżąca  
- [ ] Tak — starsze wersje (np. `/v1/`) są dostępne i **brakuje** w nich mechanizmów kontroli bezpieczeństwa obecnych w nowszych wersjach  

### Czy zasoby po stronie klienta (JavaScript/aplikacje mobilne) wyciekają struktury punktów końcowych API lub klucze?
- [ ] Nie — kod po stronie klienta **nie zawiera** zahardkodowanych ścieżek API ani poufnych kluczy  
- [ ] Tak — kod po stronie klienta zawiera mapy punktów końcowych API, ale **nie zawiera** poufnych kluczy  
- [ ] Tak — klucze API, sekrety lub adresy URL punktów końcowych przeznaczonych wyłącznie do użytku wewnętrznego **są** zahardkodowane w zasobach po stronie klienta  

### Czy na znanych punktach końcowych można wykryć ukryte parametry lub nagłówki?
- [ ] Nie — fuzzing parametrów (np. za pomocą `Arjun`) **nie ujawnił** ukrytych danych wejściowych  
- [ ] Tak — wykryto ukryte parametry (np. `debug=true`, `admin=1`), ale **nie są** one funkcjonalne  
- [ ] Tak — wykryto ukryte parametry i **mogą** one zostać użyte do zmiany zachowania aplikacji

---

## WSTG-APIT-02 — API Broken Object Level Authorization

Broken Object Level Authorization (BOLA), znane również jako Insecure Direct Object Reference (IDOR), występuje, gdy API nie przeprowadza walidacji, czy użytkownik posiada odpowiednie uprawnienia do uzyskania dostępu lub manipulacji konkretnym zasobem zidentyfikowanym przez identyfikator (ID). Atakujący wykorzystują tę podatność poprzez systematyczne enumerowanie lub zgadywanie identyfikatorów — takich jak numeryczne ID lub UUID — w ścieżkach żądań, parametrach zapytania lub strukturach JSON, aby uzyskać dostęp do danych należących do innych użytkowników. Luka ta jest najczęstszym i najbardziej dotkliwym problemem w nowoczesnym bezpieczeństwie API, mogącym prowadzić do masowej eksfiltracji danych (Exfiltration), nieautoryzowanej modyfikacji lub całkowitego przejęcia konta. Z perspektywy atakującego celem jest identyfikacja punktów końcowych (endpoints), które przyjmują identyfikatory obiektów, oraz przetestowanie, czy logika autoryzacji po stronie serwera jest nieobecna lub czy można ją pominąć poprzez manipulację tymi identyfikatorami.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Narzędzia:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Czy identyfikatory obiektów w żądaniach API są przewidywalne lub możliwe do wyliczenia (enumerable)?
- [ ] Nie — identyfikatory są długie, losowe i zabezpieczone kryptograficznie (np. UUIDv4)  
- [ ] Tak — identyfikatory są kolejnymi liczbami całkowitymi (np. `101`, `102`)  
- [ ] Tak — identyfikatory podążają za przewidywalnym wzorcem lub są oparte na publicznie dostępnych informacjach  

### Czy API przeprowadza walidację własności obiektu po stronie serwera dla każdego żądania?
- [ ] Tak — weryfikacja autoryzacji jest stosowana przy każdym żądaniu i jej obejście (bypass) jest **niemożliwe**  
- [ ] Tak — weryfikacja jest stosowana, ale jej obejście **jest możliwe** poprzez Parameter Pollution lub Method Tunneling  
- [ ] Nie — aplikacja polega wyłącznie na fakcie uwierzytelnienia użytkownika, nie sprawdzając własności konkretnego zasobu  

### Czy możliwy jest dostęp lub modyfikacja zasobu innego użytkownika poprzez zmianę identyfikatora?
- [ ] Nie — nieautoryzowany dostęp do zasobów innych użytkowników jest **niemożliwy**  
- [ ] Tak — nieautoryzowany dostęp do **odczytu** (Horyzontalne BOLA) **jest możliwy**  
- [ ] Tak — nieautoryzowana **modyfikacja** lub **usunięcie** (Horyzontalne BOLA) **jest możliwe**  

### Czy API pozwala na dostęp do obiektów administracyjnych lub systemowych poprzez podstawienie identyfikatorów?
- [ ] Nie — zasoby administracyjne są chronione przez dodatkowe warstwy autoryzacji  
- [ ] Tak — uzyskanie dostępu lub modyfikacja zasobów na poziomie systemowym (Wertykalne BOLA) **jest możliwe**  

### Czy mechanizm autoryzacji można pominąć, przenosząc identyfikator do innej części żądania?
- [ ] Nie — logika autoryzacji jest spójna niezależnie od lokalizacji parametru  
- [ ] Tak — obejście **jest możliwe**, gdy ID zostanie przeniesione ze ścieżki URL do treści JSON lub nagłówków  
- [ ] Tak — obejście **jest możliwe**, gdy podanych zostanie wiele identyfikatorów, a serwer przetwarza ten nieautoryzowany

---

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