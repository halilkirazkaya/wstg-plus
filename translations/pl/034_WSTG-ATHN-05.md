## WSTG-ATHN-05 — Testowanie pod kątem podatności funkcji "Zapamiętaj hasło"

Funkcjonalność „Zapamiętaj mnie” (Remember Me) lub trwałe logowanie ma na celu utrzymanie stanu uwierzytelnienia użytkownika po zrestartowaniu przeglądarki poprzez przechowywanie tokena lub poświadczeń po stronie klienta. Jeśli ta funkcja jest źle zaimplementowana — na przykład poprzez przechowywanie haseł otwartym tekstem, słabych skrótów (hashes) lub tokenów o niskiej entropii w plikach cookie — naraża to użytkownika na przejęcie konta poprzez Cross-Site Scripting (XSS), dostęp fizyczny lub przechwycenie ruchu sieciowego. Pentesterzy oceniają entropię tych trwałych tokenów, flagi bezpieczeństwa stosowane w mechanizmie przechowywania oraz cykl życia sesji, aby upewnić się, że wygoda trwałego dostępu nie omija krytycznych mechanizmów kontrolnych, takich jak uwierzytelnianie wieloskładnikowe (MFA) lub unieważnianie sesji (session invalidation). Eksploatacja często polega na pozyskiwaniu tych długożyjących tokenów w celu uzyskania nieautoryzowanego dostępu do kont bez konieczności podawania oryginalnych poświadczeń.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Status testu** | Nie wykonano |
| **Poziom dotkliwości** | Średni / Wysoki* |

> *Poziom dotkliwości staje się Wysoki, jeśli poświadczenia otwartym tekstem lub niesolone skróty (unsalted hashes) są przechowywane w trwałym pliku cookie lub jeśli token omija uwierzytelnianie wieloskładnikowe (MFA).

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Narzędzia:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### Czy aplikacja implementuje funkcję „Zapamiętaj mnie” lub „Pozostań zalogowany”?
- [ ] Nie — funkcja **nie istnieje** w aplikacji  
- [ ] Tak — funkcja istnieje i wykorzystuje kryptograficznie silne, nieprzewidywalne tokeny  
- [ ] Tak — funkcja istnieje, ale używa przewidywalnych tokenów lub tokenów o niskiej entropii *(Średni)*  
- [ ] Tak — funkcja istnieje i przechowuje poświadczenia otwartym tekstem lub hasła zakodowane w base64 *(Wysoki)*  

### Czy flagi bezpieczeństwa są poprawnie nałożone na trwały plik cookie?
- [ ] Tak — flagi `HttpOnly`, `Secure` oraz `SameSite` są **nałożone**  
- [ ] Tak — niektóre flagi są nałożone, ale brakuje flagi `HttpOnly`  
- [ ] Tak — niektóre flagi są nałożone, ale brakuje flagi `Secure` przy połączeniu HTTPS  
- [ ] Nie — żadne flagi bezpieczeństwa **nie są nałożone** na trwały plik cookie  

### Czy token „Zapamiętaj mnie” pozostaje ważny po ręcznym wylogowaniu?
- [ ] Nie — token jest unieważniany po stronie serwera natychmiast po wylogowaniu  
- [ ] Tak — token pozostaje ważny, ale wymaga hasła do wykonania wrażliwych operacji  
- [ ] Tak — token pozostaje ważny i pozwala na pełny dostęp do konta **bez** ponownego uwierzytelnienia *(Średni)*  

### Czy trwała sesja jest przerywana po zmianie hasła?
- [ ] Tak — wszystkie trwałe tokeny są unieważniane, gdy użytkownik zmienia hasło  
- [ ] Nie — token „Zapamiętaj mnie” pozostaje ważny po tym, jak **przeprowadzono** reset/zmianę hasła *(Wysoki)*  

### Czy trwały token omija uwierzytelnianie wieloskładnikowe (MFA) przy kolejnych wizytach?
- [ ] Nie — MFA jest wymagane nawet w obecności tokena „Zapamiętaj mnie”  
- [ ] Tak — MFA jest omijane, ale token jest powiązany ze specyficznym odciskiem palca urządzenia (device fingerprint)  
- [ ] Tak — MFA jest **całkowicie omijane** przy użyciu wyłącznie trwałego pliku cookie z dowolnego urządzenia *(Wysoki)*  

---