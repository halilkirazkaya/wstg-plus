## WSTG-CLNT-13 — Testowanie pod kątem Cross-Site Script Inclusion (XSSI)

Cross-Site Script Inclusion (XSSI) występuje, gdy aplikacja internetowa eksportuje wrażliwe dane wewnątrz dynamicznie generowanego pliku JavaScript lub odpowiedzi JSONP, które strona kontrolowana przez atakującego może następnie dołączyć za pomocą tagu `<script>`. Ponieważ dołączanie skryptów (script inclusion) omija politykę Same-Origin Policy (SOP), atakujący może wykonać skrypt we własnym kontekście i nadpisać obiekty globalne lub zmienne w celu eksfiltracji wrażliwych informacji. Podatność ta zazwyczaj objawia się w uwierzytelnionych punktach końcowych (endpoints), które zwracają dane specyficzne dla użytkownika, takie jak adresy e-mail, klucze API lub metadane sesji w formacie skryptu. Z perspektywy atakującego, eksploatacja polega na przygotowaniu złośliwej strony, która odwołuje się do podatnego skryptu i przechwytuje wynikowe zmiany zmiennych globalnych lub wywołania funkcji.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Średni / Wysoki |

> *Poziom zagrożenia staje się wysoki, jeśli wyciekłe dane obejmują tokeny CSRF, identyfikatory sesji lub wysoce wrażliwe dane osobowe (PII).

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Czy jakiekolwiek uwierzytelnione punkty końcowe zwracają dynamiczny JavaScript lub JSONP zawierający wrażliwe informacje?
- [ ] Nie — wszystkie skrypty są statyczne i **nie mogą** zawierać danych specyficznych dla użytkownika  
- [ ] Tak — istnieją dynamiczne skrypty, ale **nie zawierają** one wrażliwych informacji  
- [ ] Tak — dynamiczne skrypty zawierają dane PII lub tokeny i **są** dostępne między domenami (across origins)  

### Czy nagłówek `X-Content-Type-Options: nosniff` jest zaimplementowany w odpowiedziach zawierających wrażliwe skrypty?
- [ ] Tak — nagłówek jest **stosowany** i zapobiega sniffingowi typów MIME (MIME-type sniffing)  
- [ ] Nie — brakuje nagłówka, co potencjalnie pozwala na wykonanie treści niebędącej skryptem jako skrypt  

### Czy wrażliwe skrypty są chronione przez mechanizmy anty-XSSI, takie jak prefiksy niewykonywalne lub niestandardowe nagłówki?
- [ ] Tak — skrypty wymagają niestandardowego nagłówka lub tokena, którego **nie można** przesłać za pomocą standardowego tagu `<script>`  
- [ ] Tak — skrypty używają prefiksów niewykonywalnych (np. `)]}'`), których **nie można** łatwo ominąć  
- [ ] Nie — skrypty są bezpośrednio dostępne za pomocą żądań GET przy użyciu poświadczeń otoczenia (ambient credentials) *(Krytyczne)*  

### Czy możliwa jest eksfiltracja wrażliwych danych poprzez nadpisywanie zmiennych globalnych lub prototypów?
- [ ] Nie — wrażliwe dane mają ograniczony zasięg (scope) lub są chronione przez nowoczesne funkcje przeglądarki  
- [ ] Tak — wrażliwe dane są przypisane do zmiennych globalnych i **mogą** zostać przechwycone  
- [ ] Tak — dane wyciekają poprzez wywołania zwrotne JSONP (callbacks) które **mogą** zostać przechwycone  

---