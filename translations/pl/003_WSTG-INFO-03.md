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