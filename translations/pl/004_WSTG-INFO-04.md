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