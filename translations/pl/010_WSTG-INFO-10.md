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