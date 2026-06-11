## WSTG-CLNT-09 — Testowanie pod kątem Clickjackingu

Clickjacking, znany również jako UI redressing, występuje, gdy atakujący wykorzystuje przezroczyste lub nieprzezroczyste warstwy, aby nakłonić użytkownika do kliknięcia w przycisk lub link na innej stronie, podczas gdy użytkownik zamierzał kliknąć w stronę najwyższego poziomu. Luka ta narusza integralność działań użytkownika, co potencjalnie prowadzi do nieautoryzowanej modyfikacji danych, zmian na koncie lub transferów finansowych wykonywanych w imieniu ofiary. Atakujący zazwyczaj wykorzystują tę podatność poprzez osadzenie docelowej strony internetowej wewnątrz elementu `<iframe>` na kontrolowanej, złośliwej stronie i nałożenie na nią zwodniczej treści. Występuje to najczęściej na stronach wykonujących wrażliwe akcje zmieniające stan (state-changing actions), takie jak przyciski „Usuń konto”, formularze resetowania hasła lub panele konfiguracji administracyjnej.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Status testu** | Not Performed |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się Wysoki, jeśli za pomocą Clickjackingu można wywołać wrażliwe akcje (np. transfery środków, usunięcie konta lub Privilege Escalation).

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Narzędzia:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Czy aplikacja implementuje nagłówki po stronie serwera w celu zapobiegania nieautoryzowanemu ramkowaniu (framing)?
- [ ] Tak — `X-Frame-Options` lub `Content-Security-Policy` **są stosowane** i zapobiegają ramkowaniu *(Najbezpieczniej)*  
- [ ] Tak — nagłówki są obecne, ale **błędnie skonfigurowane** (np. `X-Frame-Options: ALLOW-FROM`, który nie posiada szerokiego wsparcia w przeglądarkach)  
- [ ] Nie — żadne nagłówki chroniące przed ramkowaniem **nie są stosowane**  

### Czy dyrektywa `frame-ancestors` w `Content-Security-Policy` (CSP) jest zaimplementowana?
- [ ] Tak — `frame-ancestors 'none'` lub `'self'` **jest stosowana**  
- [ ] Tak — dyrektywa `frame-ancestors` jest obecna, ale **zezwala** na niezaufane lub zbyt szerokie źródła (origins)  
- [ ] Nie — dyrektywa jest **nieobecna** lub CSP nie zostało zaimplementowane  

### Czy nagłówek `X-Frame-Options` (XFO) jest poprawnie skonfigurowany pod kątem wsparcia dla starszych przeglądarek (legacy)?
- [ ] Tak — `DENY` lub `SAMEORIGIN` **jest stosowane**  
- [ ] Tak — XFO jest obecny, ale **ignorowany** przez nowoczesne przeglądarki, ponieważ istnieje dyrektywa CSP `frame-ancestors`  
- [ ] Nie — nagłówek XFO jest **nieobecny** lub ustawiony na niebezpieczną wartość  

### Czy zabezpieczenia przed ramkowaniem można obejść za pomocą znanych technik?
- [ ] Nie — obejście **nie jest możliwe** przy użyciu standardowych technik (double-framing itp.)  
- [ ] Tak — zabezpieczenie **może** zostać obejścięte z powodu słabego wyrażenia regularnego (regex) lub logiki w białej liście `frame-ancestors`  
- [ ] Tak — zabezpieczenie **może** zostać obejścięte, ponieważ aplikacja polega wyłącznie na technice frame-busting opartej na JavaScript  

### Czy wrażliwa akcja jest podatna na eksploatację poprzez Proof-of-Concept Clickjackingu?
- [ ] Nie — ramkowanie jest blokowane lub żadne wrażliwe akcje nie są dostępne  
- [ ] Tak — UI redressing **jest możliwy**, ale wymaga złożonej interakcji użytkownika  
- [ ] Tak — UI redressing **jest możliwy** i pozwala na natychmiastowe akcje zmieniające stan *(Krytyczne)*  

---