## WSTG-ATHN-07 — Testowanie pod kątem słabej polityki haseł

Testowanie pod kątem słabych polityk haseł polega na ocenie wymagań dotyczących złożoności, długości i entropii wymuszanych przez aplikację podczas tworzenia konta i aktualizacji poświadczeń. Niewystarczające polityki bezpośrednio zagrażają poufności kont użytkowników, czyniąc je podatnymi na zautomatyzowane ataki słownikowe, próby Brute Force oraz Credential Stuffing. Podatności te zazwyczaj występują w punktach końcowych rejestracji, procesach resetowania hasła oraz administracyjnych panelach zarządzania użytkownikami. Z perspektywy atakującego, pobłażliwa polityka znacząco redukuje koszt obliczeniowy i czas wymagany do skutecznego przejęcia poświadczeń przy użyciu popularnych list słów lub łamania opartego na wzorcach.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Średni / Niski* |

> *Poziom krytyczności staje się Wysoki (High), jeśli w aplikacji brakuje mechanizmów blokady konta (Account Lockout) lub ograniczania liczby żądań (Rate Limiting) w celu zapobiegania automatycznemu zgadywaniu haseł.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Narzędzia:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Czy aplikacja wymusza minimalną długość hasła?
- [ ] Tak — minimalna długość to 12+ znaków *(Najbardziej bezpieczne)*  
- [ ] Tak — minimalna długość wynosi od 8 do 11 znaków  
- [ ] Tak — minimalna długość jest **mniejsza niż** 8 znaków  
- [ ] Nie — minimalna długość nie jest wymuszana  

### Czy wymuszane są wymagania dotyczące złożoności (wielkie litery, małe litery, cyfry, symbole)?
- [ ] Tak — wiele klas znaków jest obowiązkowych, a ich obejście **nie jest możliwe**  
- [ ] Tak — klasy znaków są sugerowane, ale **nie są** wymuszane  
- [ ] Nie — nie stosuje się żadnych wymagań dotyczących złożoności  

### Czy aplikacja blokuje popularne/słabe hasła lub hasła zawierające dane specyficzne dla użytkownika?
- [ ] Tak — znane słabe hasła (np. "password123") oraz hasła oparte na nazwie użytkownika **nie mogą** być użyte  
- [ ] Tak — popularne hasła są blokowane, ale dane specyficzne dla użytkownika (np. nazwa użytkownika, e-mail) **mogą** być użyte  
- [ ] Nie — każdy ciąg znaków spełniający wymagania dotyczące długości/złożoności jest akceptowany  

### Czy wymagania dotyczące złożoności lub długości mogą zostać pominięte poprzez modyfikację po stronie klienta?
- [ ] Nie — polityki są rygorystycznie wymuszane po **stronie serwera (server-side)**  
- [ ] Tak — polityki są wymuszane przez JavaScript i **mogą** zostać pominięte poprzez przechwycenie żądania  

### Czy polityka haseł jest jasno komunikowana użytkownikowi bez ujawniania szczegółów implementacji?
- [ ] Tak — polityka jest widoczna podczas wprowadzania danych i wyświetla ogólne komunikaty o błędach  
- [ ] Tak — polityka jest widoczna, ale komunikaty o błędach ujawniają konkretne wyrażenia regularne (regex) lub luki w logice  
- [ ] Nie — polityka **nie jest** widoczna, co wymusza jej odkrywanie metodą prób i błędów  

---