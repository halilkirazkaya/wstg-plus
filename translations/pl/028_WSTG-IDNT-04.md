## WSTG-IDNT-04 — Testowanie pod kątem wyliczania kont i zgadywalnych kont użytkowników

Wyliczanie kont (Account Enumeration) występuje, gdy aplikacja ujawnia istnienie lub brak konta użytkownika poprzez różnice w odpowiedziach HTTP, komunikatach o błędach lub mierzalnych różnicach czasowych. Atakujący wykorzystują to zachowanie, systematycznie przesyłając listy nazw użytkowników w celu zidentyfikowania prawidłowych kont, które następnie stają się celem ataków typu Credential Stuffing, Brute Force lub socjotechniki. Ta podatność powszechnie występuje w portalach logowania, funkcjach resetowania hasła, formularzach rejestracji oraz punktach końcowych API obsługujących identyfikatory specyficzne dla użytkownika. Z perspektywy atakującego, skuteczne wyliczenie (enumeration) znacząco zawęża powierzchnię ataku, przekształcając próbę ataku typu "blind Brute Force" w ukierunkowaną eksploatację znanych, poprawnych tożsamości.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Status testu** | Niewykonano |
| **Poziom zagrożenia** | Niski / Średni |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Czy komunikaty o błędach uwierzytelniania są spójne we wszystkich scenariuszach?
- [ ] Tak — ogólne komunikaty o błędach są używane zarówno dla nieprawidłowych nazw użytkowników, jak i nieprawidłowych haseł  
- [ ] Tak — komunikaty są podobne, ale **występują** niewielkie różnice w interpunkcji lub wielkości liter  
- [ ] Nie — komunikaty o błędach jawnie informują: „Użytkownik nie został znaleziony” lub „Nieprawidłowe hasło” *(Podatność)*  

### Czy proces resetowania hasła lub funkcja „zapomniałem hasła” ujawnia istnienie konta?
- [ ] Nie — aplikacja zwraca ogólny komunikat niezależnie od tego, czy adres e-mail/nazwa użytkownika istnieje  
- [ ] Tak — zabezpieczenia są wdrożone, ale obejście (bypass) **jest możliwe** poprzez analizę długości odpowiedzi lub kodów statusu  
- [ ] Tak — aplikacja jawnie potwierdza, czy wysłano wiadomość e-mail z resetowaniem hasła, czy też konto **nie istnieje**  

### Czy proces rejestracji jest chroniony przed wyliczaniem użytkowników?
- [ ] Nie — rejestracja nie ujawnia informacji lub wykorzystuje CAPTCHA/Rate Limiting w celu zapobiegania masowym sprawdzeniom  
- [ ] Tak — zabezpieczenia są wdrożone, ale obejście (bypass) **jest możliwe** poprzez analizę czasu odpowiedzi lub różnic w odpowiedziach API  
- [ ] Tak — aplikacja natychmiast powiadamia użytkownika, jeśli nazwa użytkownika lub adres e-mail **są już zajęte**  

### Czy różnice czasowe są mierzalne podczas porównywania poprawnych i niepoprawnych nazw użytkowników?
- [ ] Nie — czasy odpowiedzi są spójne niezależnie od poprawności konta dzięki zastosowaniu porównań o stałym czasie (constant-time)  
- [ ] Tak — różnice czasowe istnieją, ale są zbyt małe, aby można je było wiarygodnie zmierzyć przez sieć  
- [ ] Tak — mierzalne rozbieżności czasowe **są obecne** (np. z powodu wykonywania hashowania haseł tylko dla poprawnych użytkowników)  

### Czy aplikacja stosuje przewidywalne lub łatwe do odgadnięcia konwencje nazewnictwa kont?
- [ ] Nie — identyfikatory kont są losowe (UUID) lub zdefiniowane przez użytkownika i złożone  
- [ ] Tak — identyfikatory kont są zgodne ze wzorcem (np. `emp001`, `emp002`) i wyliczanie (enumeration) **jest możliwe**  
- [ ] Tak — identyfikatory są kolejnymi liczbami całkowitymi, co sprawia, że pełne wyliczenie bazy danych **jest możliwe**  

---