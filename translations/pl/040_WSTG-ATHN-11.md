## WSTG-ATHN-11 — Testowanie uwierzytelniania wieloskładnikowego (MFA)

Testowanie uwierzytelniania wieloskładnikowego (Multi-Factor Authentication - MFA) ocenia odporność dodatkowej warstwy bezpieczeństwa, zaprojektowanej w celu zapobiegania nieautoryzowanemu dostępowi, nawet w przypadku przejęcia głównych danych uwierzytelniających. Atakujący często próbują obejść MFA (bypass), identyfikując punkty końcowe (endpoints), w których nie jest ono konsekwentnie wymuszane, takie jak starsze wersje API, backendy aplikacji mobilnych lub przepływy pracy związane z resetowaniem hasła. Eksploatacja często obejmuje manipulowanie odpowiedziami serwera, ataki brute-force na kody o krótkim czasie trwania (OTP), czy wykorzystywanie błędów typu race condition oraz luk w zarządzaniu sesją, które pozwalają użytkownikowi przejść do stanu uwierzytelnionego bez podania drugiego składnika.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### Czy MFA jest wymuszane konsekwentnie we wszystkich portalach uwierzytelniania?
- [ ] Tak — MFA jest wymagane dla wszystkich prób logowania przez WWW, urządzenia mobilne i interfejsy API.
- [ ] Tak — ale MFA **nie jest wymuszane** na konkretnych punktach końcowych (np. przestarzałe `/api/v1/login` lub portale dedykowane dla urządzeń mobilnych).
- [ ] Nie — MFA **nie zostało zaimplementowane** w aplikacji *(Krytyczne)*.

### Czy krok weryfikacji MFA można obejść poprzez bezpośrednią nawigację do adresu URL lub manipulację odpowiedzią?
- [ ] Nie — bezpośrednia nawigacja lub manipulacja parametrami **nie pozwalają** na obejście wyzwania (challenge).
- [ ] Tak — nawigacja bezpośrednio do wewnętrznych adresów URL panelu sterowania **jest możliwa** bez ukończenia wyzwania MFA.
- [ ] Tak — manipulacja odpowiedzią serwera (np. zmiana kodu `401 Unauthorized` na `200 OK`) **jest możliwa** w celu uzyskania dostępu.

### Czy proces weryfikacji kodu MFA jest chroniony przed atakami brute-force?
- [ ] Tak — po wielu nieudanych próbach OTP stosowane jest rygorystyczne ograniczanie liczby żądań (Rate Limiting) lub blokada konta.
- [ ] Tak — ograniczanie liczby żądań istnieje, ale **można je obejść** poprzez rotację adresów IP lub manipulację nagłówkami.
- [ ] Nie — nie wdrożono ograniczania liczby żądań, a zautomatyzowane łamanie kodów metodą brute-force **jest możliwe**.

### Czy aplikacja utrzymuje bezpieczny stan sesji podczas przejścia MFA?
- [ ] Tak — tokeny sesji o wysokich uprawnieniach są wydawane **dopiero po** pomyślnym ukończeniu MFA.
- [ ] Nie — w pełni funkcjonalny plik cookie sesji jest wydawany **przed** ukończeniem MFA, co pozwala na dostęp do niektórych funkcji.
- [ ] Nie — aplikacja używa statycznego identyfikatora lub przewidywalnego przejścia sesji, które **może zostać przejęte** (Session Hijacking).

### Czy alternatywne czynniki MFA (SMS, e-mail, kody zapasowe) są podatne na eksploitację?
- [ ] Nie — wszystkie czynniki wykorzystują bezpieczne, nieprzewidywalne i ograniczone czasowo wartości.
- [ ] Tak — kody zapasowe są przewidywalne lub **mogą zostać wyliczone** (enumeration).
- [ ] Tak — funkcjonalność „Wyślij kod ponownie” (Resend Code) może zostać nadużyta do przeprowadzenia ataków SMS/Email Flooding lub ujawnienia częściowych danych kontaktowych.

---