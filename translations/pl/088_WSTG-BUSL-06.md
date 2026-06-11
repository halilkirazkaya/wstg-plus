## WSTG-BUSL-06 — Testowanie pod kątem omijania przepływów pracy

Omijanie przepływu pracy (Workflow) polega na manipulowaniu logiką aplikacji w celu pominięcia zamierzonych sekwencji lub wymagań wstępnych w procesach wieloetapowych. Atakujący identyfikują wrażliwe punkty końcowe (endpoints) — takie jak potwierdzenia płatności, zatwierdzenia administracyjne lub ekrany zakończenia MFA — i próbują uzyskać do nich bezpośredni dostęp, pomijając obowiązkowe kroki pośrednie. Ta podatność występuje, gdy logika po stronie serwera nie utrzymuje solidnej maszyny stanów, polegając zamiast tego na założeniu, że użytkownik podąża ścieżką wyznaczoną przez interfejs użytkownika (UI). Skuteczna eksploatacja może prowadzić do krytycznych błędów logiki biznesowej (Business Logic), w tym nieautoryzowanych transakcji, eskalacji uprawnień (Privilege Escalation) oraz omijania mechanizmów weryfikacji tożsamości.


| Pole | Wartość |
|---|---|
| **ID WSTG** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się wysoki, jeśli obejście pozwala na nieautoryzowane transakcje finansowe, eskalację uprawnień lub dostęp administracyjny.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Czy aplikacja zawiera wieloetapowe procesy biznesowe?
- [ ] Nie — aplikacja składa się wyłącznie z akcji opartych na pojedynczych żądaniach  
- [ ] Tak — obecne są procesy wieloetapowe (np. koszyki zakupowe, formularze typu wizard, rejestracja)  

### Czy sekwencja kroków jest ściśle egzekwowana przez serwer?
- [ ] Tak — śledzenie stanu po stronie serwera zapewnia, że kroki **nie mogą** zostać pominięte  
- [ ] Tak — mechanizmy kontrolne po stronie klienta sugerują sekwencję, ale serwer **nie waliduje** postępu  
- [ ] Nie — aplikacja **nie wymusza** żadnej konkretnej kolejności operacji  

### Czy atakujący może dotrzeć do końcowego etapu procesu poprzez bezpośrednie wywołanie docelowego adresu URL lub punktu końcowego (endpoint)?
- [ ] Nie — bezpośredni dostęp do kroków końcowych **nie jest możliwy** bez wcześniejszego ukończenia etapów poprzedzających  
- [ ] Tak — kroki końcowe (np. `/api/v1/checkout/complete`) **mogą** zostać wywołane poprzez bezpośrednie żądanie  

### Czy aplikacja weryfikuje, czy dane wymagane z poprzednich kroków są obecne i prawidłowe?
- [ ] Tak — serwer waliduje wszystkie dane z poprzednich kroków przed kontynuowaniem  
- [ ] Nie — serwer **jest** podatny na „pomijanie” kroków, w których dane są oczekiwane, ale nie są weryfikowane  

### Czy mechanizmy kontroli bezpieczeństwa, takie jak MFA lub weryfikacja tożsamości, mogą zostać pominięte poprzez manipulację przepływem pracy?
- [ ] Nie — krytyczne mechanizmy kontrolne **nie mogą** zostać obejściowe poprzez manipulację przepływem  
- [ ] Tak — obejście MFA lub weryfikacji tożsamości **jest możliwe** poprzez przejście bezpośrednio do kroku następującego po weryfikacji  

---