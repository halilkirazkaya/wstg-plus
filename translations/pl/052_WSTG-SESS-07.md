## WSTG-SESS-07 — Testowanie limitu czasu sesji (Session Timeout)

Testowanie limitu czasu sesji ocenia, czy aplikacja skutecznie kończy sesję użytkownika po uprzednio zdefiniowanym okresie bezczynności lub całkowitym czasie jej trwania. Mechanizm ten jest kluczowy dla mitygacji ryzyka Session Hijacking, szczególnie na współdzielonych stacjach roboczych lub w scenariuszach, w których atakujący przechwytuje identyfikator sesji poprzez przechwytywanie ruchu sieciowego lub Cross-Site Scripting (XSS). Pentesterzy analizują zarówno Idle Timeout (limit czasu bezczynności), który aktywuje się po okresie braku aktywności, jak i Absolute Timeout (bezwzględny limit czasu), który ogranicza całkowity czas życia sesji bez względu na aktywność. Z perspektywy atakującego, nieokreślone lub nadmiernie długie sesje zapewniają wydłużone okno czasowe na utrzymanie nieautoryzowanego dostępu i pozwalają uniknąć konieczności ponownego uwierzytelnienia.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom ważności** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Czy aplikacja implementuje limit czasu bezczynności sesji (idle session timeout)?
- [ ] Tak — sesja wygaśnie po krótkim okresie (np. 15-30 minut) bezczynności  
- [ ] Tak — sesja wygaśnie, ale czas trwania limitu jest **nadmiernie długi** (np. > 24 godziny)  
- [ ] Nie — sesja **pozostaje aktywna** w nieskończoność podczas bezczynności  

### Czy limit czasu sesji jest wymuszany po stronie serwera (server-side)?
- [ ] Tak — serwer odrzuca żądania z wygasłymi tokenami niezależnie od stanu po stronie klienta  
- [ ] Nie — limit czasu jest wymuszany **wyłącznie** za pomocą JavaScript po stronie klienta lub meta-refresh  

### Czy aplikacja implementuje bezwzględny limit czasu sesji (absolute session timeout)?
- [ ] Tak — sesja jest kończona po ustalonym maksymalnym czasie trwania, niezależnie od aktywności  
- [ ] Nie — sesja **może** być przedłużana w nieskończoność, dopóki trwa ciągła aktywność  

### Czy identyfikator sesji jest unieważniany na serwerze po wygaśnięciu limitu czasu?
- [ ] Tak — token sesji **nie może** zostać ponownie użyty po osiągnięciu progu limitu czasu  
- [ ] Nie — token sesji **nadal jest ważny** na serwerze, nawet po przekierowaniu klienta do strony logowania  

### Czy sesja może być utrzymywana w nieskończoność przy użyciu zautomatyzowanych żądań heartbeat?
- [ ] Nie — Absolute Timeout lub dodatkowe mechanizmy kontrolne **zapobiegają** nieskończonemu przedłużaniu sesji  
- [ ] Tak — okresowe żądania w tle (np. AJAX) **mogą** utrzymywać sesję w nieskończoność  

---