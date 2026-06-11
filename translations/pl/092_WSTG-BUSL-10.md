## WSTG-BUSL-10 — Testowanie funkcjonalności płatności

Testowanie funkcjonalności płatności polega na identyfikowaniu błędów logiki biznesowej, które pozwalają atakującemu na ominięcie Payment Gateways, manipulowanie sumami zamówień lub podszywanie się pod sygnały pomyślnej transakcji. Podatności te zazwyczaj występują w komunikacji między aplikacją, przeglądarką klienta a zewnętrznymi procesorami płatności, takimi jak Stripe, PayPal lub wewnętrznymi systemami księgowymi. Atakujący może próbować modyfikować parametry ceny podczas przesyłania, przesyłać ujemne ilości produktów w celu obniżenia całkowitego kosztu lub powtarzać (Replay) wywołania zwrotne (Callbacks) pomyślnych transakcji, aby zrealizować zamówienia bez faktycznej płatności. Skuteczna eksploatacja prowadzi do bezpośrednich strat finansowych, rozbieżności w stanach magazynowych oraz potencjalnego naruszenia integralności e-commerce aplikacji.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Status testu** | Nie przeprowadzono |
| **Dotkliwość** | Wysoka / Krytyczna |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Czy kwota transakcji jest walidowana po stronie serwera przed przetworzeniem?
- [ ] Tak — kwota jest rygorystycznie walidowana względem bazy danych produktów po stronie serwera *(Najbezpieczniejsze)*  
- [ ] Tak — kwota jest walidowana, ale obejście logiki **jest możliwe** poprzez Parameter Pollution lub zmianę waluty  
- [ ] Nie — kwota transakcji **może** zostać zmodyfikowana w żądaniu po stronie klienta i jest akceptowana przez backend  

### Czy ilość pozycji może być manipulowana w celu uzyskania zerowej lub ujemnej kwoty całkowitej?
- [ ] Nie — ujemne lub zerowe ilości są odrzucane przez logikę walidacji aplikacji  
- [ ] Tak — ujemne ilości są akceptowane w koszyku, ale suma jest korygowana podczas płatności  
- [ ] Tak — ujemne ilości **mogą** być użyte do obniżenia całkowitej sumy zamówienia lub uzyskania kredytu *(Krytyczne)*  

### Czy aplikacja bezpiecznie obsługuje wywołania zwrotne (Callbacks) lub Webhooks bramki płatniczej?
- [ ] Tak — wywołania zwrotne wymagają ważnego podpisu kryptograficznego, którego **nie można** sfałszować  
- [ ] Tak — podpisy są sprawdzane, ale klucz (Secret) jest słaby lub weryfikacja **może** zostać pominięta  
- [ ] Nie — aplikacja polega na nieuwierzytelnionych lub niezweryfikowanych wywołaniach zwrotnych w celu potwierdzenia statusu płatności  

### Czy aplikacja jest podatna na Race Conditions podczas etapu finalizacji zamówienia lub płatności?
- [ ] Nie — integralność transakcyjna i mechanizmy blokowania bazy danych są **włączone**  
- [ ] Tak — Race Conditions **są możliwe**, ale nie wpływają na ostateczną cenę ani realizację  
- [ ] Tak — współbieżne żądania **mogą** prowadzić do wielokrotnej realizacji dla pojedynczej płatności lub "podwójnego wydatkowania" (Double-spending) kart podarunkowych  

### Czy kody rabatowe lub punkty lojalnościowe podlegają manipulacji lub ponownemu użyciu?
- [ ] Nie — kody są walidowane pod kątem jednorazowego użytku, a ograniczenia logiczne są **stosowane**  
- [ ] Tak — kody mogą być używane wielokrotnie lub stosowane do niekwalifikujących się pozycji, ale wpływ jest ograniczony  
- [ ] Tak — atakujący **mogą** stosować wiele sprzecznych kodów lub omijać daty wygaśnięcia i limity użycia  

---