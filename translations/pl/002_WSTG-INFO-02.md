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