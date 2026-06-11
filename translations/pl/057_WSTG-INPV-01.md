## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Reflected Cross Site Scripting (XSS) występuje, gdy aplikacja dołącza niezaufane dane do odpowiedzi HTTP bez wystarczającej walidacji lub kodowania, co powoduje wykonanie ładunku (Payload) w kontekście przeglądarki ofiary. Atakujący dostarczają złośliwe ładunki (Payloads) za pomocą socjotechniki, zazwyczaj poprzez spreparowane adresy URL lub formularze, w celu przejęcia sesji użytkownika, eksfiltracji wrażliwych plików cookie lub wykonania nieautoryzowanych działań w imieniu użytkownika. Podatność ta jest powszechnie spotykana w parametrach wyszukiwania, komunikatach o błędach i wszelkich punktach końcowych (endpoints), które odzwierciedlają dane wejściowe bezpośrednio w interfejsie użytkownika. Eksploatacja opiera się na interakcji ofiary ze złośliwym linkiem, co czyni go nietrwałym, ale wysoce skutecznym wektorem ataku (attack vector) wymierzonym w konkretnych administratorów lub uwierzytelnionych użytkowników.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki (High) |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Czy dane wejściowe dostarczone przez użytkownika są odzwierciedlane w treści odpowiedzi (response body)?
- [ ] Nie — dane wejściowe **nigdy** nie są odzwierciedlane użytkownikowi  
- [ ] Tak — dane wejściowe są odzwierciedlane, ale poprawnie zakodowane i obejście (Bypass) **nie jest możliwe**  
- [ ] Tak — dane wejściowe są odzwierciedlane **bez** żadnego kodowania ani sanityzacji  

### Czy zaimplementowano kodowanie wyjściowe uwzględniające kontekst (context-aware output encoding)?
- [ ] Tak — odpowiednie kodowanie (HTML, Attribute, JavaScript) jest **stosowane** w oparciu o specyficzny kontekst odzwierciedlenia  
- [ ] Tak — kodowanie jest **stosowane**, ale jest niewystarczające dla danego kontekstu (np. kodowanie HTML wewnątrz tagu `<script>`)  
- [ ] Nie — kodowanie **nie jest stosowane**  

### Czy można obejść walidację danych wejściowych lub filtry Web Application Firewall (WAF)?
- [ ] Nie — filtry skutecznie blokują wszystkie powszechne i zaciemnione (obfuscated) ładunki XSS  
- [ ] Tak — filtry są obecne, ale obejście (**Bypass**) jest możliwe przy użyciu kodowania znaków, niestandardowych tagów lub poliglottów (polyglots)  
- [ ] Nie — brak mechanizmów filtracji lub walidacji  

### Jaki jest kontekst wykonywania odzwierciedlonych danych wejściowych?
- [ ] Bezpieczny — dane wejściowe są odzwierciedlane w miejscu niewykonywalnym (np. wewnątrz standardowych tagów `<div>` lub `<span>`)  
- [ ] Ryzykowny — dane wejściowe są odzwierciedlane wewnątrz atrybutów HTML lub w parametrach `src`/`href` tagów  
- [ ] Krytyczny — dane wejściowe są odzwierciedlane bezpośrednio w blokach `<script>`, programach obsługi zdarzeń (event handlers) lub literałach szablonowych (template literals)  

### Czy obecne są nowoczesne nagłówki bezpieczeństwa przeglądarki w celu złagodzenia skutków XSS?
- [ ] Tak — `Content-Security-Policy` (CSP) jest **włączony** z restrykcyjną polityką script-src  
- [ ] Tak — `Content-Security-Policy` (CSP) jest **włączony**, ale zawiera `unsafe-inline` lub słabe białe listy (whitelists)  
- [ ] Nie — brak nagłówków CSP lub przestarzałych nagłówków `X-XSS-Protection`  

---