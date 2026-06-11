## WSTG-INFO-08 — Identyfikacja frameworka aplikacji webowej (Fingerprinting)

Identyfikacja frameworka (fingerprinting) aplikacji webowej polega na rozpoznaniu konkretnego stosu technologicznego oraz wersji oprogramowania użytych do budowy i serwowania aplikacji. Atakujący analizują nagłówki odpowiedzi HTTP, pliki cookie specyficzne dla frameworka, przewidywalne struktury plików oraz unikalne artefakty JavaScript, aby określić, czy cel wykorzystuje platformy takie jak React, Angular, Django czy Spring. Ta faza rekonesansu jest kluczowa, ponieważ pozwala testerowi zawęzić powierzchnię ataku do znanych podatności (CVE) oraz typowych błędów konfiguracyjnych specyficznych dla danego frameworka. Dzięki zrozumieniu podstawowej architektury, atakujący może przejść od testów ogólnych do wysoce ukierunkowanych strategii eksploatacji (Exploit).

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Informacyjny (Informational) |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Czy nagłówki odpowiedzi HTTP ujawniają nazwę lub wersję frameworka?
- [ ] Nie — nagłówki takie jak `X-Powered-By` lub `Server` nie są obecne lub zostały oczyszczone (sanitized)  
- [ ] Tak — nazwa frameworka jest obecna, ale wersja jest ukryta  
- [ ] Tak — zarówno nazwa frameworka, jak i jego wersja są ujawnione  

### Czy pliki cookie lub struktury katalogów specyficzne dla frameworka są identyfikowalne?
- [ ] Nie — powszechne artefakty frameworka (np. ścieżki `.js`, nazwy plików cookie) są zaciemnione (obfuscated) lub zmienione  
- [ ] Tak — pliki cookie specyficzne dla frameworka (np. `JSESSIONID`, `PHPSESSID`, `csrftoken`) są identyfikowalne  
- [ ] Tak — struktury katalogów (np. `/wp-content/`, `_next/static/`) są widoczne i potwierdzają użyty framework  

### Czy komunikaty o błędach lub komentarze w kodzie źródłowym ujawniają szczegóły dotyczące frameworka?
- [ ] Nie — obsługa błędów jest ogólna, a komentarze zostały usunięte  
- [ ] Tak — zrzuty stosu (stack traces) specyficzne dla frameworka są włączone i widoczne w odpowiedziach  
- [ ] Tak — kod źródłowy HTML zawiera komentarze lub tagi metadanych identyfikujące framework  

### Czy ze zidentyfikowaną wersją frameworka powiązane są znane podatności?
- [ ] Nie — zidentyfikowany framework jest aktualny i nie posiada znanych publicznych podatności  
- [ ] Tak — wersja frameworka jest nieaktualna i można zidentyfikować konkretne podatności CVE  

---