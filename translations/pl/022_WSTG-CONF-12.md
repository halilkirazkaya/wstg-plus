## WSTG-CONF-12 — Content Security Policy (CSP)

Content Security Policy (CSP) jest krytycznym mechanizmem głębokiej obrony (defense-in-depth) wdrażanym za pomocą nagłówków odpowiedzi HTTP w celu mitygacji ryzyk takich jak Cross-Site Scripting (XSS), clickjacking oraz ataki typu data injection. Poprzez definiowanie, które zasoby dynamiczne mogą być ładowane, dobrze skonfigurowana polityka CSP ogranicza zdolność atakującego do wykonywania nieautoryzowanych skryptów lub eksfiltracji wrażliwych danych do zewnętrznych domen. Pentesterzy oceniają politykę pod kątem nadmiernie permisywnych dyrektyw, użycia niebezpiecznych słów kluczowych, takich jak `'unsafe-inline'`, oraz polegania na niezabezpieczonych sieciach CDN, które mogą ułatwiać obejścia (bypass). Słaba lub brakująca polityka CSP nie tworzy bezpośrednio podatności, ale znacząco zwiększa wpływ udanych ataków typu injection poprzez brak zapewnienia drugorzędnej warstwy ochrony.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Niski / Średni |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Narzędzia:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Czy nagłówek Content Security Policy (CSP) jest obecny w odpowiedziach aplikacji?
- [ ] Tak — nagłówek `Content-Security-Policy` jest **obecny** i **wymuszany**  
- [ ] Tak — nagłówek `Content-Security-Policy-Report-Only` jest **obecny** w celach testowych  
- [ ] Nie — nagłówek CSP nie jest **obecny** w odpowiedziach  

### Czy dyrektywy `script-src` lub `default-src` są odpowiednio ograniczone?
- [ ] Tak — dyrektywy wykorzystują rygorystyczne białe listy, wartości nonce lub skróty (hash), a obejście (bypass) **nie jest możliwe**  
- [ ] Tak — dyrektywy są **prawidłowo skonfigurowane**, ale biała lista zawiera znane sieci CDN, które można obejść  
- [ ] Nie — dyrektywy używają symboli wieloznacznych (`*`) lub schematów `data:`, co sprawia, że obejście jest **możliwe**  

### Czy polityka zezwala na wykonywanie niebezpiecznych skryptów wbudowanych (inline) lub funkcji eval?
- [ ] Nie — `'unsafe-inline'` oraz `'unsafe-eval'` **nie występują**  
- [ ] Tak — `'unsafe-inline'` jest **obecny**, ale chroniony przez wartość `nonce` lub `hash`  
- [ ] Tak — `'unsafe-inline'` lub `'unsafe-eval'` są **włączone** bez dodatkowych zabezpieczeń *(Krytyczne)*  

### Czy aplikacja jest chroniona przed atakami clickjacking za pomocą dyrektywy `frame-ancestors`?
- [ ] Tak — dyrektywa `frame-ancestors` jest ustawiona na `'none'` lub `'self'`  
- [ ] Tak — dyrektywa `frame-ancestors` jest **włączona**, ale zezwala na określone, zaufane domeny stron trzecich  
- [ ] Nie — dyrektywa `frame-ancestors` jest **nieobecna**, aplikacja polega na przestarzałym nagłówku `X-Frame-Options` lub brakuje ochrony  

### Czy istnieje mechanizm raportowania naruszeń CSP?
- [ ] Tak — dyrektywa `report-uri` lub `report-to` jest **skonfigurowana** i aktywna  
- [ ] Nie — raportowanie naruszeń jest **wyłączone** lub **nieskonfigurowane**  

---