## WSTG-BUSL-02 — Testowanie możliwości fałszowania żądań

Fałszowanie żądań w kontekście logiki biznesowej polega na manipulowaniu parametrami zdefiniowanymi przez aplikację w celu wykonania działań wykraczających poza zamierzony przepływ pracy (workflow) lub poziom uprawnień. Atakujący biorą na cel procesy wieloetapowe, takie jak systemy realizacji zamówień (checkout) lub rejestracja kont, gdzie stan jest utrzymywany za pomocą parametrów po stronie klienta, których serwer nie weryfikuje ponownie względem zaufanego źródła po stronie backendu. Poprzez zmianę ukrytych pól, wartości JSON lub parametrów REST, takich jak ceny, ilości lub wewnętrzne flagi statusu, atakujący może ominąć wymagania dotyczące płatności, przeprowadzić eskalację uprawnień (privilege escalation) lub wywołać niezamierzone zmiany stanu. Ta podatność zazwyczaj objawia się w stanowych formularzach internetowych lub interfejsach API, gdzie serwer bezkrytycznie ufa reprezentacji stanu biznesowego przesłanej przez klienta podczas transakcji.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom krytyczności** | Wysoki / Krytyczny* |

> *Poziom krytyczności staje się Krytyczny, jeśli sfałszowane żądanie pozwala na straty finansowe, nieautoryzowane działania administracyjne lub pełne przejęcie konta.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Czy aplikacja weryfikuje parametry transakcyjne względem serwerowego „źródła prawdy” (source of truth)?
- [ ] Tak — wszystkie wrażliwe parametry (cena, rola, ID) są weryfikowane w bazie danych, a ich obejście jest **niemożliwe**  
- [ ] Tak — walidacja jest stosowana, ale można ją ominąć poprzez Parameter Pollution lub alternatywne kodowanie  
- [ ] Nie — aplikacja ufa wartościom po stronie klienta w celu określenia kosztu lub wyniku transakcji *(Krytyczny)*  

### Czy wartości krytyczne dla biznesu (np. ilości, kwoty) mogą być modyfikowane na wartości nieautoryzowane?
- [ ] Nie — wartości ujemne, zerowe lub nadmiernie wysokie są odrzucane przez serwer  
- [ ] Tak — serwer akceptuje zmodyfikowane wartości, ale tylko w ograniczonym zakresie  
- [ ] Tak — wartości ujemne lub zerowe **mogą** zostać przesłane, co prowadzi do strat finansowych lub obejścia logiki  

### Czy ukryte pola lub parametry API są używane do utrzymywania stanu w procesach wieloetapowych?
- [ ] Nie — stan jest utrzymywany w bezpieczny sposób za pomocą sesji po stronie serwera lub podpisanych tokenów  
- [ ] Tak — stan jest przekazywany przez klienta, ale kontrole integralności (HMAC/Signatures) są **włączone** i skuteczne  
- [ ] Tak — stan jest przekazywany przez klienta, a kontrole integralności są **wyłączone** lub mogą zostać usunięte  

### Czy możliwe jest sfałszowanie żądań pomijających obowiązkowe kroki pośrednie w przepływie pracy?
- [ ] Nie — serwer wymusza ścisłą sekwencję operacji dla każdej transakcji  
- [ ] Tak — kroki **mogą** zostać pominięte, ale ostateczne działanie nadal wymaga prawidłowych danych z poprzednich kroków  
- [ ] Tak — ostateczne żądanie „wyślij” (submit) lub „zakończ” (complete) **może** zostać sfałszowane bezpośrednio w celu ominięcia autoryzacji lub kroków płatności  

---