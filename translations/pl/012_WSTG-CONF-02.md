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