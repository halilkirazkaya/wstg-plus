## WSTG-BUSL-07 — Testowanie mechanizmów obronnych przed nadużyciami aplikacji

Mechanizmy obronne przed nadużyciami aplikacji oceniają zdolność aplikacji do identyfikowania, rejestrowania i reagowania na anomalne zachowania użytkowników, które odbiegają od zamierzonej logiki biznesowej (Business Logic). W przeciwieństwie do tradycyjnych zabezpieczeń opartych na sygnaturach, mechanizmy te koncentrują się na wykrywaniu wzorców takich jak szybkie serie żądań (rapid-fire requests), Credential Stuffing czy sekwencyjna enumeracja zasobów, które sygnalizują zautomatyzowane nadużycia. Z perspektywy atakującego, test ten określa odporność aplikacji na automatyzację oraz ataki typu "low and slow", zaprojektowane w celu eksfiltracji danych lub wyczerpania zasobów bez wyzwalania standardowych alertów bezpieczeństwa. Brak wdrożenia tych zabezpieczeń często skutkuje masowym scrapingiem danych (Data Scraping), przejęciami kont (Account Takeover) lub atakami odmowy usługi (DoS) poprzez legalne, ale zasobożerne funkcje aplikacji.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Średni / Wysoki |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Czy mechanizmy Rate Limiting lub Throttling są wymuszane na wrażliwych punktach końcowych (endpoints)?
- [ ] Tak — rygorystyczny Rate Limiting **jest stosowany** i wymuszany na wszystkich wrażliwych punktach końcowych *(Najbardziej bezpieczne)*  
- [ ] Tak — Rate Limiting **jest stosowany**, ale można go pominąć poprzez rotację adresów IP lub manipulację nagłówkami  
- [ ] Tak — Rate Limiting **jest stosowany** tylko w punktach końcowych uwierzytelniania, pozostawiając inne niezabezpieczone  
- [ ] Nie — Rate Limiting ani Throttling **nie są stosowane** w żadnych funkcjach aplikacji  

### Czy aplikacja wykrywa i blokuje wzorce interakcji zautomatyzowanych?
- [ ] Tak — analiza behawioralna lub mechanizmy CAPTCHA **są włączone** i skutecznie blokują boty  
- [ ] Tak — zabezpieczenia przed automatyzacją (Anti-automation) **są włączone**, ale ich obejście **jest możliwe** przy użyciu przeglądarek headless lub cyklicznej zmiany sesji (Session Cycling)  
- [ ] Nie — aplikacja **nie potrafi** odróżnić użytkownika (człowieka) od zautomatyzowanego skryptu  

### Czy anomalne zachowania są rejestrowane i czy następuje po nich reakcja obronna?
- [ ] Tak — nadużycie wyzwala automatyczne blokowanie i powiadamia zespół ds. bezpieczeństwa  
- [ ] Tak — nadużycie jest rejestrowane w logach na potrzeby audytu, ale **nie są** podejmowane żadne automatyczne działania obronne  
- [ ] Nie — aplikacja **nie rejestruje** ani nie reaguje na nietypowe wzorce aktywności  

### Czy mechanizmy obronne mogą zostać pominięte poprzez manipulację identyfikatorami po stronie klienta lub nagłówkami?
- [ ] Nie — mechanizmy obronne opierają się na stanie po stronie serwera i zweryfikowanej telemetrii  
- [ ] Tak — mechanizmy kontrolne **mogą** zostać pominięte poprzez czyszczenie plików cookie lub rotację ciągów `User-Agent`  
- [ ] Tak — mechanizmy kontrolne **mogą** zostać pominięte poprzez wstrzykiwanie nagłówków takich jak `X-Forwarded-For` lub `X-Real-IP`  

---