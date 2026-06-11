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