## WSTG-CONF-10 — Subdomain Takeover

Przejęcie subdomeny (Subdomain Takeover) występuje, gdy wpis DNS (zazwyczaj CNAME, ale sporadycznie rekordy A lub MX) wskazuje na wycofany z eksploatacji lub nieistniejący zasób u zewnętrznego dostawcy usług chmurowych lub hostingowych. Atakujący identyfikują te „wiszące” (dangling) rekordy DNS, szukając konkretnych komunikatów o błędach od dostawców takich jak AWS, GitHub lub Azure, które wskazują, że zasób nie jest już zajęty. Poprzez zarejestrowanie porzuconej nazwy zasobu na platformie dostawcy, atakujący uzyskuje pełną kontrolę nad subdomeną, co pozwala mu na hostowanie złośliwych treści, eksfiltrację ciasteczek sesyjnych (session cookies) przypisanych do domeny głównej oraz obchodzenie zabezpieczeń Content Security Policy (CSP) lub CORS. Podatność ta jest szczególnie niebezpieczna, ponieważ wykorzystuje zaufanie związane z legalną strukturą domenową organizacji w celu ułatwienia wysokiej skali ataków phishingowych i kradzieży danych.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Status testu** | Nie przeprowadzono |
| **Poziom zagrożenia** | Wysoki / Krytyczny* |

> *Poziom zagrożenia staje się krytyczny, jeśli subdomena może zostać użyta do przejęcia ciasteczek sesyjnych z domeny głównej lub obejścia przekierowań SSO/OAuth.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Narzędzia:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### Czy aplikacja korzysta z zewnętrznych usług hostingowych lub chmurowych za pośrednictwem subdomen?
- [ ] Nie — wszystkie subdomeny wskazują na przestrzeń adresową IP kontrolowaną przez organizację  
- [ ] Tak — subdomeny wskazują na zewnętrznych dostawców (S3, GitHub Pages, Heroku itp.)  

### Czy istnieją jakiekolwiek „wiszące” (dangling) rekordy DNS wskazujące na nieistniejące zasoby?
- [ ] Nie — wszystkie zidentyfikowane rekordy DNS wskazują na aktywne, kontrolowane zasoby  
- [ ] Tak — rekordy CNAME lub A istnieją dla zasobów, które **już nie istnieją** u dostawcy  
- [ ] Tak — rekordy DNS zwracają status NXDOMAIN lub strony błędów specyficzne dla dostawcy *(np. „NoSuchBucket”)*  

### Czy zidentyfikowany wiszący zasób może zostać pomyślnie zajęty?
- [ ] Nie — dostawca posiada zabezpieczenia przed zajmowaniem porzuconych nazw lub wymagana jest weryfikacja konta  
- [ ] Tak — nazwa zasobu **może** zostać zarejestrowana na platformie zewnętrznej przez dowolnego użytkownika  

### Czy domena główna używa ciasteczek o zasięgu „Domain”, które mogłyby zostać przejęte?
- [ ] Nie — ciasteczka mają ściśle określony zasięg dla konkretnych subdomen lub używają flag `Host-Only`  
- [ ] Tak — wrażliwe ciasteczka (identyfikatory sesji, JWT) są przypisane do domeny nadrzędnej i **mogą** zostać wyeksfiltrowane za pośrednictwem przejętej subdomeny  

### Czy subdomena może zostać użyta do obejścia nagłówków bezpieczeństwa lub procesów uwierzytelniania?
- [ ] Nie — brak zależności bezpieczeństwa od zidentyfikowanych subdomen  
- [ ] Tak — subdomena znajduje się na białej liście w dyrektywach CSP `script-src` lub `connect-src`  
- [ ] Tak — subdomena jest prawidłowym URI przekierowania (redirect URI) dla konfiguracji OAuth lub SSO  

---