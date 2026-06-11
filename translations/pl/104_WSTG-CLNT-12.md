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