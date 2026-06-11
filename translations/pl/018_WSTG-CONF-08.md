## WSTG-CONF-08 — Testowanie polityki Cross-Domain w aplikacjach RIA

Polityki cross-domain w aplikacjach typu Rich Internet Application (RIA) obejmują pliki takie jak `crossdomain.xml` oraz `clientaccesspolicy.xml`, które definiują, w jaki sposób klienci webowi — w szczególności Adobe Flash i Microsoft Silverlight — obsługują żądania cross-origin. Pliki te są kluczowe dla bezpieczeństwa, ponieważ jawnie nadpisują mechanizm Same-Origin Policy (SOP), określając, które domeny zewnętrzne mają zezwolenie na interakcję z aplikacją i odczytywanie jej odpowiedzi. Zbyt liberalne konfiguracje, takie jak użycie symboli wieloznacznych (wildcards) w atrybucie `domain`, pozwalają atakującym na umieszczenie złośliwych obiektów RIA w zewnętrznej witrynie, co może umożliwić eksfiltrację wrażliwych danych lub wykonywanie działań w imieniu uwierzytelnionych użytkowników. Pentesterzy zazwyczaj lokalizują te pliki w katalogu głównym serwera (web root), aby ocenić poziom zaufania przyznany zewnętrznym źródłom (origins) i zidentyfikować potencjalne wycieki danych typu cross-origin.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się Wysoki, jeśli aplikacja przetwarza wrażliwe dane użytkowników lub identyfikatory sesji, które mogą zostać eksfiltrowane za pomocą zbyt liberalnej polityki.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Czy pliki polityki cross-domain (`crossdomain.xml` lub `clientaccesspolicy.xml`) znajdują się w katalogu głównym?
- [ ] Nie — nie odnaleziono plików w katalogu głównym (root)
- [ ] Tak — plik `crossdomain.xml` (Flash) jest obecny
- [ ] Tak — plik `clientaccesspolicy.xml` (Silverlight) jest obecny
- [ ] Tak — oba pliki polityki RIA są obecne

### Czy atrybut domeny `allow-access-from` jest ograniczony do zaufanych źródeł?
- [ ] Nie — pliki istnieją, ale nie zawierają wpisów `allow-access-from`
- [ ] Tak — konkretne, zaufane domeny są jawnie dopuszczone (whitelisted) i obejście nie jest możliwe
- [ ] Tak — domeny są na białej liście, ale obejmują niezaufane podmioty trzecie lub subdomeny
- [ ] Tak — użyto symbolu wieloznacznego `*`, co pozwala na dostęp z dowolnego źródła (Krytyczne)

### Czy polityka zezwala na niezabezpieczoną komunikację przez HTTP dla zasobów HTTPS?
- [ ] Nie — ustawiono `secure="true"` (lub jest to wartość domyślna), co uniemożliwia źródłom HTTP dostęp do danych HTTPS
- [ ] Tak — jawnie ustawiono `secure="false"`, co umożliwia atakującym typu MITM eksfiltrację bezpiecznych danych

### Czy nagłówki HTTP są skonfigurowane zbyt liberalnie w polityce cross-domain?
- [ ] Nie — `allow-http-request-headers-from` jest ograniczone lub nie zostało włączone
- [ ] Tak — określone nagłówki są dozwolone dla zaufanych domen
- [ ] Tak — użyto symbolu wieloznacznego `*` dla nagłówków, co pozwala atakującym na wysyłanie niestandardowych nagłówków z dowolnej domeny

### Czy konfiguracja `site-control` (master-policy) jest restrykcyjna?
- [ ] Nie — `permitted-cross-domain-policies` jest ustawione na `none` lub `master-only`
- [ ] Tak — polityka zezwala innym plikom na serwerze na definiowanie własnych reguł (`all`)

---