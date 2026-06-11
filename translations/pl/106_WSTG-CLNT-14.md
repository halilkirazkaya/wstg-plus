## WSTG-CLNT-14 — Testowanie pod kątem Reverse Tabnabbing

Reverse Tabnabbing to podatność po stronie klienta, w której strona otwierana za pomocą `target="_blank"` zachowuje funkcjonalne odniesienie do strony nadrzędnej poprzez obiekt `window.opener`. Pozwala to kontrolowanej przez atakującego witrynie docelowej na programowe przekierowanie oryginalnej, zaufanej karty do złośliwego adresu URL, zazwyczaj strony phishingowej zaprojektowanej w celu wykradania danych uwierzytelniających. Atak wykorzystuje naturalne zaufanie użytkownika do oryginalnej karty, ponieważ prawdopodobieństwo zauważenia, że strona, którą właśnie opuścił, przeszła do innej domeny, jest niskie. Nowoczesne praktyki bezpieczeństwa wymagają stosowania atrybutów `rel="noopener"` lub `rel="noreferrer"` w znacznikach anchor (kotwicach) lub ustawienia właściwości `opener` na null w JavaScript, aby zapobiec tej komunikacji międzyokiennej.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Niski / Średni* |

> *Poziom zagrożenia jest Niski w większości nowoczesnych przeglądarek, ponieważ obecnie domyślnie ustawiają one `target="_blank"` na `rel="noopener"`. Poziom zagrożenia staje się Średni, jeśli aplikacja jest skierowana do starszych przeglądarek lub używa `window.open()` bez ustawienia właściwości `opener` na null.

**Odnośniki:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Czy występują linki, które otwierają się w nowym oknie lub karcie?
- [ ] Nie — żadne linki nie używają `target="_blank"` ani równoważnego przekierowania opartego na JavaScript  
- [ ] Tak — `target="_blank"` jest używane wyłącznie w wewnętrznych, zaufanych linkach  
- [ ] Tak — `target="_blank"` jest używane w linkach zewnętrznych lub adresach URL dostarczonych przez użytkownika  

### Czy relacja `window.opener` jest ograniczona w znacznikach anchor?
- [ ] Tak — `rel="noopener"` lub `rel="noreferrer"` jest **konsekwentnie stosowane** do wszystkich linków z `target="_blank"` *(Najbezpieczniej)*  
- [ ] Tak — atrybuty są stosowane, ale **brakuje ich** w linkach o wysokim ryzyku lub generowanych przez użytkownika  
- [ ] Nie — atrybuty **nie są stosowane**, co pozwala stronie podrzędnej na dostęp do `window.opener`  

### Czy aplikacja jest podatna na Reverse Tabnabbing poprzez JavaScriptowe `window.open()`?
- [ ] Nie — `window.open()` nie jest używane lub jawnie ustawia właściwość `opener` na null  
- [ ] Tak — `window.open()` jest używane, a odniesienie `opener` **pozostaje dostępne** dla okna podrzędnego  

### Czy atakujący może kontrolować cel linku, który otwiera się w nowej karcie?
- [ ] Nie — wszystkie cele linków są wpisane na sztywno i wskazują na zaufane domeny  
- [ ] Tak — cele linków są dostarczane przez użytkownika (np. profile w mediach społecznościowych, linki w biogramach) i **nie są** sanitizowane pod kątem zabezpieczeń przed tabnabbingiem  

---