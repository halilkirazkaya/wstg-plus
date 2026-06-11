## WSTG-BUSL-01 — Testowanie walidacji danych logiki biznesowej

Testowanie walidacji danych logiki biznesowej koncentruje się na zdolności aplikacji do identyfikowania i odrzucania danych, które są poprawne pod względem składniowym, ale nieprawidłowe pod względem logicznym w kontekście domeny biznesowej. Atakujący wykorzystują te błędy, przesyłając nieoczekiwane wartości — takie jak ujemne ilości w koszyku zakupowym, daty z przeszłości dla wydarzeń przyszłych lub ekstremalnie duże liczby całkowite — w celu obejścia ograniczeń i manipulacji stanem aplikacji. Podatności te często występują w wieloetapowych przepływach pracy (workflows), procesach zamówień oraz modułach zarządzania kontem, gdzie logika po stronie serwera nie weryfikuje integralności kontekstowej danych dostarczonych przez użytkownika. Skuteczna eksploatacja może prowadzić do strat finansowych, naruszenia integralności oraz nieautoryzowanego dostępu do zastrzeżonych funkcji lub danych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Wysoki / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Czy aplikacja wymusza logiczne limity zakresu dla wejściowych danych numerycznych?
- [ ] Tak — aplikacja poprawnie odrzuca wartości ujemne, zerowe lub nadmiernie duże, gdy są one niewłaściwe *(Najbardziej bezpieczne)*  
- [ ] Tak — aplikacja odrzuca niektóre nieprawidłowe zakresy, ale **jest** podatna na przepełnienie liczby całkowitej (integer overflow) lub obejście granic (boundary bypass)  
- [ ] Nie — aplikacja akceptuje każdą wartość numeryczną niezależnie od kontekstu biznesowego *(Krytyczne)*  

### Czy weryfikacja walidacji po stronie serwera jest spójna we wszystkich warstwach aplikacji?
- [ ] Tak — walidacja jest stosowana spójnie zarówno w warstwie API/usługi, jak i bazy danych  
- [ ] Tak — walidacja **jest stosowana** w interfejsie użytkownika (UI/Frontend), ale obejście poprzez bezpośrednie żądanie API **jest możliwe**  
- [ ] Nie — walidacja **nie jest stosowana** lub jest niespójna, co pozwala na logiczne uszkodzenie danych  

### Czy logika biznesowa może zostać naruszona poprzez manipulację danymi w wieloetapowych procesach (workflows)?
- [ ] Nie — stan jest utrzymywany w bezpieczny sposób na serwerze, a dane z poprzednich kroków **nie mogą** zostać zmodyfikowane  
- [ ] Tak — walidacja odbywa się w początkowych krokach, ale końcowe przesłanie danych **nie weryfikuje** ponownie integralności danych  
- [ ] Tak — obejście procesu (workflow bypass) **jest możliwe** poprzez pomijanie kroków lub przesyłanie danych poza kolejnością  

### Czy aplikacja prawidłowo obsługuje dane typu „edge case”, które omijają ograniczenia biznesowe?
- [ ] Tak — ograniczenia są solidne i obsługują nietypowe, ale poprawne składniowo dane wejściowe (np. lata przestępne, zmiany stref czasowych)  
- [ ] Tak — niektóre przypadki graniczne (edge cases) są obsługiwane, ale określone kombinacje **mogą** prowadzić do nieoczekiwanego zachowania  
- [ ] Nie — przypadki graniczne prowadzą bezpośrednio do błędów logiki, takich jak darmowe zakupy lub nieautoryzowane zmiany statusu  

---