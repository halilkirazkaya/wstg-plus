## WSTG-CONF-03 — Testowanie obsługi rozszerzeń plików pod kątem informacji wrażliwych

Testowanie obsługi rozszerzeń plików polega na identyfikacji sytuacji, w których serwer WWW lub serwer aplikacji ujawnia informacje wrażliwe, udostępniając pliki, które powinny być zastrzeżone lub wykonywane. Atakujący często poszukują plików kopii zapasowych (backup), plików konfiguracyjnych oraz fragmentów kodu źródłowego (np. `.bak`, `.old`, `.env`, `.inc`), które mogą zostać pozostawione w katalogu głównym serwera (web root) i udostępnione jako czysty tekst (plain text) z powodu braku odpowiednich mapowań procedur obsługi (handler mappings). Podatność ta jest istotna, ponieważ może prowadzić do ujawnienia poświadczeń bazy danych, kluczy API oraz wewnętrznej logiki biznesowej, ułatwiając dalszą eksploatację (exploit) infrastruktury. Takie wycieki zazwyczaj występują w katalogach publicznych, folderach przesyłania plików (upload) lub wynikają z błędnej konfiguracji typów MIME (MIME-type) na serwerze.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się Wysoki, jeśli dostępne są pliki konfiguracyjne zawierające poświadczenia lub pełny kod źródłowy.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Czy wrażliwe rozszerzenia kopii zapasowych lub plików tymczasowych (np. `.bak`, `.old`, `.swp`, `~`) są dostępne?
- [ ] Nie — serwer zwraca 403 Forbidden lub 404 Not Found dla powszechnych rozszerzeń kopii zapasowych  
- [ ] Tak — serwer pozwala na pobieranie plików kopii zapasowych, ale **nie zawierają** one danych wrażliwych  
- [ ] Tak — serwer pozwala na pobieranie plików kopii zapasowych zawierających dane wrażliwe lub kod źródłowy *(Wysoki)*  

### Czy serwer ujawnia pliki konfiguracyjne środowiska lub systemu (np. `.env`, `.config`, `.yml`, `.ini`)?
- [ ] Nie — dostęp do zastrzeżonych plików konfiguracyjnych **nie jest możliwy** i zwracane jest 403/404  
- [ ] Tak — wrażliwe pliki konfiguracyjne **są** dostępne i ujawniają wewnętrzne tajemnice lub poświadczenia  

### Czy kod źródłowy można pobrać poprzez dodanie alternatywnych rozszerzeń (np. `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] Nie — kod aplikacji **nie jest** renderowany jako czysty tekst, niezależnie od manipulacji rozszerzeniem  
- [ ] Tak — kod źródłowy jest ujawniany, ponieważ serwer **nie posiada** procedury obsługi (handler) dla dodanego rozszerzenia  

### Czy katalogi administracyjne lub metadane (np. `.git/`, `.svn/`, `.DS_Store`) są zablokowane dla dostępu publicznego?
- [ ] Nie — te katalogi/pliki **nie istnieją** w katalogu głównym serwera  
- [ ] Tak — dostęp **nie jest możliwy** ze względu na reguły przepisania (rewrite rules) lub uprawnienia po stronie serwera  
- [ ] Tak — katalogi metadanych **są** dostępne i pozwalają na pełną rekonstrukcję kodu źródłowego  

### Jak serwer obsługuje żądania dotyczące plików z wieloma rozszerzeniami (np. `file.php.jpg`)?
- [ ] Nie — serwer poprawnie priorytetyzuje bezpieczeństwo wewnętrznego rozszerzenia lub blokuje żądanie  
- [ ] Tak — serwer przetwarza plik zgodnie z pierwszym rozszerzeniem, ale obejście (bypass) w celu wykonania kodu **nie jest możliwe**  
- [ ] Tak — serwer ignoruje końcowe rozszerzenie, co **umożliwia** wykonanie kodu lub ujawnienie źródła  

---