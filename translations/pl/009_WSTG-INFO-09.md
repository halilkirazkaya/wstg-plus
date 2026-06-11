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