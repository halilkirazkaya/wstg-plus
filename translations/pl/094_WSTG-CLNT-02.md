## WSTG-CLNT-02 — Testowanie wykonywania skryptów JavaScript

Testowanie wykonywania skryptów JavaScript koncentruje się na identyfikacji przypadków, w których aplikacja niewłaściwie obsługuje dane dostarczone przez użytkownika, co prowadzi do wykonywania nieautoryzowanych skryptów w kontekście przeglądarki ofiary. Ta podatność zazwyczaj skutkuje wystąpieniem Cross-Site Scripting (XSS), co pozwala atakującym na eksfiltrację ciasteczek sesyjnych, manipulację Document Object Model (DOM) lub wykonywanie działań w imieniu użytkownika. Testerzy penetracyjni badają punkty wyjścia (sinks), takie jak `innerHTML`, `document.write()` i `eval()`, w których mogą być przetwarzane niezutylizowane dane pochodzące ze źródeł (sources), takich jak fragmenty URL, `localStorage` lub `window.name`. Skuteczna eksploatacja narusza integralność i poufność sesji użytkownika i może służyć jako punkt wyjścia (pivot) dla bardziej złożonych ataków po stronie klienta (client-side).


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Narzędzia:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### Czy aplikacja przetwarza dane dostarczone przez użytkownika w niebezpiecznych punktach wyjścia (JavaScript sinks)?
- [ ] Nie — wszystkie dane są poddawane sanityzacji lub używane w bezpiecznych punktach wyjścia, takich jak `textContent`  
- [ ] Tak — dane są przetwarzane w niebezpiecznych punktach wyjścia, ale zastosowano sanityzację/kodowanie  
- [ ] Tak — dane są przetwarzane w niebezpiecznych punktach wyjścia, a sanityzacja nie została zastosowana  

### Czy obecna jest polityka Content Security Policy (CSP) w celu ograniczenia wykonywania skryptów?
- [ ] Tak — restrykcyjna polityka CSP jest włączona i jej obejście (bypass) nie jest możliwe  
- [ ] Tak — polityka CSP jest włączona, ale obejście jest możliwe poprzez `unsafe-inline` lub niezabezpieczone sieci CDN  
- [ ] Nie — polityka CSP nie jest włączona  

### Czy skrypty JavaScript mogą być wykonywane poprzez parametry URL lub fragmenty (DOM-based XSS)?
- [ ] Nie — dane wejściowe są prawidłowo zakodowane i wykonanie skryptu nie jest możliwe  
- [ ] Tak — dane wejściowe są odzwierciedlone w DOM, ale wykonanie jest uniemożliwione przez mechanizmy ochronne nowoczesnych frameworków  
- [ ] Tak — dane wejściowe są bezpośrednio wykonywane w przeglądarce, a eksploatacja (Exploit) jest możliwa  

### Czy programy obsługi zdarzeń (event handlers) lub schematy URI są podatne na wstrzykiwanie skryptów (script injection)?
- [ ] Nie — dane wejściowe są poddawane sanityzacji w celu usunięcia atrybutów zdarzeń oraz schematów `javascript:`  
- [ ] Tak — programy obsługi zdarzeń mogą zostać wstrzyknięte, ale filtry ograniczają złożoność ładunku (Payload)  
- [ ] Tak — możliwe jest wykonanie dowolnego kodu JavaScript poprzez atrybuty `onerror`, `onload` lub `href`  

---