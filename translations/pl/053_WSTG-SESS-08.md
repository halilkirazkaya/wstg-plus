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