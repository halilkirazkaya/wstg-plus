## WSTG-SESS-05 — Testowanie pod kątem Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) to podatność, w której atakujący nakłania przeglądarkę ofiary do wykonania niepożądanego działania w innej witrynie, w której ofiara jest obecnie uwierzytelniona. Exploit ten wykorzystuje mechanizm przeglądarki polegający na automatycznym dołączaniu poświadczeń kontekstowych ("ambient" credentials), takich jak pliki cookie sesji lub nagłówki Authorization, do wychodzących żądań. Atakujący zazwyczaj celują w operacje zmieniające stan (state-changing operations), takie jak zmiana haseł, aktualizacja adresów e-mail lub wykonywanie przelewów finansowych, poprzez hostowanie złośliwych skryptów lub ukrytych formularzy w witrynie osoby trzeciej. Skuteczna eksploatacja może prowadzić do pełnego przejęcia konta (account takeover) lub nieautoryzowanej modyfikacji danych bez wiedzy lub zgody użytkownika.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się wysoki (High), jeśli podatna akcja pozwala na przejęcie konta, eskalację uprawnień (privilege escalation) lub nieautoryzowane transakcje finansowe.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Narzędzia:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Czy tokeny anti-CSRF są zaimplementowane dla żądań zmieniających stan?
- [ ] Tak — unikalne, silne kryptograficznie tokeny są wymagane dla wszystkich akcji zmieniających stan  
- [ ] Tak — tokeny są obecne, ale **nie** są unikalne dla sesji lub są przewidywalne  
- [ ] Nie — tokeny anti-CSRF **nie** zostały zaimplementowane  

### Czy walidacja tokena anti-CSRF po stronie serwera jest solidna?
- [ ] Tak — serwer rygorystycznie waliduje token i obejście (bypass) **nie jest możliwe**  
- [ ] Tak — walidacja jest wykonywana, ale **można** ją obejść poprzez usunięcie parametru tokena  
- [ ] Tak — walidacja jest wykonywana, ale **można** ją obejść poprzez podanie pustego lub fikcyjnego tokena  
- [ ] Tak — walidacja jest wykonywana, ale token **nie** jest powiązany z sesją użytkownika  

### Czy aplikacja polega na łatwych do obejścia metodach ochrony przed CSRF?
- [ ] Nie — aplikacja nie polega wyłącznie na słabych nagłówkach lub samym sprawdzeniu pochodzenia (origin checks)  
- [ ] Tak — ochrona opiera się wyłącznie na nagłówku `Referer` lub `Origin`, który **może** zostać sfałszowany lub usunięty  
- [ ] Tak — ochrona opiera się na sprawdzeniu nagłówka `X-Requested-With`, który **może** zostać pominięty poprzez błędną konfigurację CORS  

### Czy pliki cookie sesji są skonfigurowane z atrybutem `SameSite`?
- [ ] Tak — atrybut `SameSite` jest ustawiony na `Strict` lub `Lax` dla wszystkich plików cookie związanych z sesją  
- [ ] Nie — atrybut `SameSite` jest **nieobecny**, co powoduje domyślne zachowanie zależne od przeglądarki  
- [ ] Nie — `SameSite` jest jawnie ustawiony na `None` bez flagi `Secure` lub dodatkowych zabezpieczeń  

### Czy ochrona przed CSRF może zostać pominięta poprzez zmianę metody żądania HTTP?
- [ ] Nie — ochrona jest egzekwowana niezależnie od użytej metody HTTP  
- [ ] Tak — tokeny są walidowane tylko w żądaniach `POST`, ale akcja **może** zostać wykonana za pomocą `GET`  
- [ ] Tak — zmiana metody (np. na `PUT` lub `DELETE`) pozwala na pominięcie sprawdzenia tokena  

---