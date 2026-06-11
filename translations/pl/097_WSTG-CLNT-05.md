## WSTG-CLNT-05 — Testowanie pod kątem CSS Injection

CSS Injection występuje, gdy aplikacja zezwala na wpływ danych wejściowych użytkownika na kaskadowe arkusze stylów (CSS) bez odpowiedniej sanityzacji lub escapowania. Choć często postrzegane jako problem kosmetyczny, atakujący mogą wykorzystać CSS Injection do eksfiltracji wrażliwych danych, takich jak tokeny CSRF lub identyfikatory sesji, używając selektorów atrybutów i właściwości background-image do wywoływania żądań zewnętrznych. Luka ta zazwyczaj występuje w punktach końcowych (API), gdzie preferencje użytkownika (np. niestandardowe motywy, kolory czcionek) są odzwierciedlane wewnątrz bloków `<style>` lub atrybutów `style` inline. Z perspektywy atakującego, udany Exploit może prowadzić do UI redressing, phishingu poprzez nieautoryzowaną modyfikację układu strony lub dyskretnej eksfiltracji danych w środowiskach, w których rygorystyczne polityki Content Security Policy (CSP) mogłyby w innym przypadku blokować ataki oparte na JavaScript.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się wysoki (High), jeśli punkt iniekcji pozwala na eksfiltrację wrażliwych atrybutów, takich jak tokeny CSRF, lub jeśli może zostać wykorzystany do UI redressing w krytycznych procesach.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Czy aplikacja odzwierciedla dane wejściowe kontrolowane przez użytkownika w kontekstach CSS?
- [ ] Nie — dane wejściowe **nie są** odzwierciedlane w tagach style, atrybutach ani plikach CSS  
- [ ] Tak — dane są odzwierciedlane, ale ściśle ograniczone do bezpiecznych wartości alfanumerycznych  
- [ ] Tak — dane są odzwierciedlane wewnątrz atrybutu `style` lub bloku `<style>`  

### Czy metaznaki i funkcje specyficzne dla CSS są odpowiednio poddawane sanityzacji?
- [ ] Tak — znaki takie jak `{`, `}`, `:` oraz funkcje takie jak `url()` są **poprawnie escapowane**  
- [ ] Tak — sanityzacja jest wdrożona, ale możliwe jest jej obejście (bypass) poprzez kodowanie lub komentarze CSS  
- [ ] Nie — żadna sanityzacja nie jest stosowana wobec metaznaków CSS  

### Czy możliwa jest eksfiltracja danych przy użyciu selektorów CSS?
- [ ] Nie — selektory **nie mogą** wyzwalać żądań zewnętrznych ani powodować wycieku danych  
- [ ] Tak — częściowa eksfiltracja danych **jest możliwa** poprzez selektory atrybutów i `background-image`  
- [ ] Tak — pełna eksfiltracja wrażliwych tokenów **jest możliwa** przy użyciu zautomatyzowanych technik Brute Force  

### Czy polityka Content Security Policy (CSP) mityguje ryzyko CSS Injection?
- [ ] Tak — dyrektywa `style-src` jest ograniczona do 'self', a `img-src` lub `connect-src` **są ograniczone**  
- [ ] Tak — CSP istnieje, ale używa 'unsafe-inline' lub zezwala na domeny zewnętrzne poprzez symbole wieloznaczne  
- [ ] Nie — nie wdrożono CSP w celu zapobiegania ładowaniu zewnętrznych zasobów przez CSS  

### Czy podatność może zostać wykorzystana do przeprowadzenia UI redressing lub phishingu?
- [ ] Nie — zakres iniekcji jest zbyt ograniczony, aby znacząco zmodyfikować układ strony  
- [ ] Tak — modyfikacja interfejsu użytkownika (UI) **jest możliwa**, co pozwala na nakładanie złośliwych elementów  
- [ ] Tak — możliwa jest pełna kontrola nad układem strony, co ułatwia przeprowadzenie wysoce przekonujących ataków phishingowych  

---