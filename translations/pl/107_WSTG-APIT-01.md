## WSTG-APIT-01 — Rekonesans API

Rekonesans API to systematyczny proces identyfikacji i mapowania powierzchni ataku API w celu odkrycia punktów końcowych (endpoints), obsługiwanych metod oraz podstawowych struktur danych. Atakujący przeprowadzają tę fazę, aby odkryć nieudokumentowane (shadow) API, przestarzałe wersje z podatnościami typu legacy oraz publicznie dostępne pliki dokumentacji, takie jak definicje Swagger lub OpenAPI, które ujawniają wewnętrzną logikę biznesową. Poprzez analizę przewidywalnych wzorców URL, plików JavaScript po stronie klienta oraz wyników brute-force katalogów, przeciwnik może zbudować kompleksową mapę API, aby zidentyfikować cele dla ataków typu Broken Object Level Authorization (BOLA) lub Mass Assignment. Rekonesans ten zazwyczaj celuje w powszechne ścieżki, takie jak `/api/v1/`, `/swagger.json` lub `/graphql`, aby uzyskać mapę drogową funkcjonalności backendu aplikacji.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Status testu** | Nie przeprowadzono |
| **Poziom zagrożenia** | Informacyjny / Niski |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Narzędzia:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Czy pliki dokumentacji API (Swagger, OpenAPI, WSDL) są publicznie dostępne?
- [ ] Nie — dokumentacja API nie jest dostępna lub dostęp do niej jest ograniczony przez uwierzytelnianie  
- [ ] Tak — dokumentacja jest dostępna, ale wymaga prawidłowych danych uwierzytelniających  
- [ ] Tak — dokumentacja API (np. `/swagger-ui.html`) jest **publicznie dostępna** bez uwierzytelniania  

### Czy za pomocą fuzzingu można odkryć nieudokumentowane lub „ukryte” (shadow) punkty końcowe API?
- [ ] Nie — fuzzing katalogów i punktów końcowych nie ujawnił żadnych nieudokumentowanych ścieżek  
- [ ] Tak — znaleziono nieudokumentowane punkty końcowe, ale **nie można** uzyskać do nich dostępu bez autoryzacji  
- [ ] Tak — znaleziono nieudokumentowane punkty końcowe i **można** uzyskać do nich dostęp bez prawidłowej autoryzacji  

### Czy aplikacja ujawnia wiele wersji API (np. /v1/, /v2/, /beta/)?
- [ ] Nie — dostępna jest tylko bieżąca, zabezpieczona wersja API  
- [ ] Tak — istnieją starsze wersje, ale stosują te **same** mechanizmy kontroli bezpieczeństwa co wersja bieżąca  
- [ ] Tak — starsze wersje (np. `/v1/`) są dostępne i **brakuje** w nich mechanizmów kontroli bezpieczeństwa obecnych w nowszych wersjach  

### Czy zasoby po stronie klienta (JavaScript/aplikacje mobilne) wyciekają struktury punktów końcowych API lub klucze?
- [ ] Nie — kod po stronie klienta **nie zawiera** zahardkodowanych ścieżek API ani poufnych kluczy  
- [ ] Tak — kod po stronie klienta zawiera mapy punktów końcowych API, ale **nie zawiera** poufnych kluczy  
- [ ] Tak — klucze API, sekrety lub adresy URL punktów końcowych przeznaczonych wyłącznie do użytku wewnętrznego **są** zahardkodowane w zasobach po stronie klienta  

### Czy na znanych punktach końcowych można wykryć ukryte parametry lub nagłówki?
- [ ] Nie — fuzzing parametrów (np. za pomocą `Arjun`) **nie ujawnił** ukrytych danych wejściowych  
- [ ] Tak — wykryto ukryte parametry (np. `debug=true`, `admin=1`), ale **nie są** one funkcjonalne  
- [ ] Tak — wykryto ukryte parametry i **mogą** one zostać użyte do zmiany zachowania aplikacji  

---