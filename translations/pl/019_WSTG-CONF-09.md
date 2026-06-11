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