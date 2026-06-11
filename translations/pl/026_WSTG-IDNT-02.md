## WSTG-IDNT-02 — Testowanie procesu rejestracji użytkownika

Testowanie procesu rejestracji użytkownika polega na ocenie przepływu pracy (workflow), za pomocą którego tworzone są nowe tożsamości, w celu upewnienia się, że nie może on zostać nadużyty do nieautoryzowanego dostępu lub automatycznej eksploatacji. Podatności w tym procesie często objawiają się brakiem weryfikacji tożsamości (e-mail/SMS), podatnością na masowe tworzenie kont przez boty lub ujawnianiem informacji poprzez zbyt szczegółowe komunikaty o błędach, które zdradzają istniejące nazwy użytkowników. Atakujący wykorzystują te wady do przeprowadzania enumeracji użytkowników (User Enumeration), prowadzenia kampanii spamowych na dużą skalę lub manipulowania parametrami rejestracji w celu uzyskania podwyższonych uprawnień na etapie tworzenia konta. Test ten jest kluczowy dla ochrony integralności aplikacji i zapobiegania zapełnianiu systemu przez atakujących fałszywymi lub złośliwymi kontami.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom dotkliwości** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Czy wymagana jest weryfikacja tożsamości przed aktywacją nowego konta?
- [ ] Tak — rejestracja wymaga unikalnego, ograniczonego czasowo tokenu wysyłanego przez e-mail lub SMS  
- [ ] Tak — weryfikacja jest wymagana, ale tokeny są **słabe** lub **przewidywalne**  
- [ ] Nie — konta są aktywowane **natychmiastowo** bez żadnego kroku weryfikacji  

### Czy proces rejestracji zapobiega enumeracji użytkowników (User Enumeration)?
- [ ] Tak — aplikacja zwraca **identyczne** odpowiedzi zarówno dla nowych, jak i istniejących adresów e-mail/nazw użytkowników  
- [ ] Nie — komunikaty o błędach (np. „Email już w użyciu”) **pozwalają** atakującemu na mapowanie zarejestrowanych użytkowników  
- [ ] Nie — różnice w czasie odpowiedzi serwera (timing differences) **ujawniają**, czy dany użytkownik istnieje  

### Czy wdrożono mechanizmy chroniące przed automatyzacją, aby zapobiec masowej rejestracji?
- [ ] Tak — mechanizmy CAPTCHA lub proof-of-work są **obecne** i **skuteczne**  
- [ ] Tak — mechanizmy istnieją, ale możliwe jest ich **obejście** (Bypass) poprzez punkty końcowe API lub manipulację nagłówkami  
- [ ] Nie — do punktu końcowego rejestracji nie zastosowano mechanizmów `Rate Limiting` ani CAPTCHA  

### Czy parametry rejestracji mogą być manipulowane w celu eskalacji uprawnień?
- [ ] Nie — role użytkowników są przypisywane **wyłącznie** przez logikę po stronie serwera  
- [ ] Tak — parametry takie jak `role`, `admin` lub `group_id` **mogą** być modyfikowane w żądaniu w celu uzyskania wyższego poziomu dostępu  

### Czy polityka haseł jest egzekwowana podczas fazy rejestracji?
- [ ] Tak — silne wymagania dotyczące haseł są **wymuszane** po stronie serwera  
- [ ] Nie — słabe hasła (np. „password123”) są **akceptowane** przez system  

---