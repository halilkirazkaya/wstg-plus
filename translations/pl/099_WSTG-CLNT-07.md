## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) jest mechanizmem przeglądarkowym, który pozwala serwerowi na jawne zezwolenie na dostęp do swoich zasobów z różnych źródeł (origins), skutecznie rozluźniając politykę Same-Origin Policy (SOP). Błędne konfiguracje występują, gdy aplikacja nadmiernie ufa dowolnym źródłom, często poprzez odzwierciedlanie nagłówka `Origin` w nagłówku odpowiedzi `Access-Control-Allow-Origin` lub poprzez stosowanie źle zaimplementowanych białych list opartych na wyrażeniach regularnych (regex). Z perspektywy atakującego, permisywna polityka CORS może zostać wykorzystana poprzez umieszczenie złośliwego skryptu na kontrolowanej domenie, który wykonuje uwierzytelnione żądania do podatnej aplikacji, co pozwala atakującemu na eksfiltrację wrażliwych danych, takich jak tokeny CSRF lub PII, bezpośrednio z treści odpowiedzi. Podatność ta jest najbardziej krytyczna w przypadku punktów końcowych API, które przetwarzają uwierzytelnione sesje i zwracają niepubliczne informacje.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Średni / Wysoki |

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Narzędzia:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Czy aplikacja implementuje nagłówki CORS na uwierzytelnionych punktach końcowych?
- [ ] Nie — CORS **nie jest włączony**; przeglądarka domyślnie stosuje rygorystyczną politykę Same-Origin Policy  
- [ ] Tak — nagłówki CORS są obecne i ograniczone do rygorystycznej **białej listy** (whitelist)  
- [ ] Tak — nagłówki CORS są obecne, ale skonfigurowane w sposób **permisywny**  

### W jaki sposób serwer obsługuje nagłówek `Access-Control-Allow-Origin` (ACAO)?
- [ ] ACAO jest ustawiony na konkretną, **zaufaną** domenę statyczną  
- [ ] ACAO jest ustawiony na `*` (wildcard) i poświadczenia (credentials) **nie mogą** być przesyłane  
- [ ] ACAO jest ustawiony na `*` (wildcard) i poświadczenia **mogą** być przesyłane *(Uwaga: większość przeglądarek blokuje taką konfigurację)*  
- [ ] ACAO **dynamicznie odzwierciedla** wartość nagłówka `Origin` z żądania  

### Czy nagłówek `Access-Control-Allow-Credentials` (ACAC) jest ustawiony na `true`?
- [ ] Nie — ACAC **nie występuje** lub jest ustawiony na `false`  
- [ ] Tak — ACAC jest ustawiony na `true`, ale ACAO jest **ograniczone** do zaufanych źródeł  
- [ ] Tak — ACAC jest ustawiony na `true`, a ACAO **odzwierciedla** dowolne źródła *(Krytyczne)*  

### Czy biała lista źródeł może zostać pominięta poprzez manipulację subdomeną lub źródłem null?
- [ ] Nie — biała lista jest rygorystycznie egzekwowana i **nie może** zostać pominięta  
- [ ] Tak — biała lista akceptuje **dowolną** subdomenę celu (np. `attacker.target.com`)  
- [ ] Tak — biała lista akceptuje źródło `null` poprzez sandboksowane ramki iframe  
- [ ] Tak — biała lista jest podatna na obejścia wyrażeń regularnych (np. `target.com.attacker.com` lub `target.com-safe.com`)  

### Czy konfiguracja CORS pozwala na eksfiltrację wrażliwych danych?
- [ ] Nie — wystawione punkty końcowe **nie zawierają** wrażliwych danych ani danych specyficznych dla sesji  
- [ ] Tak — uwierzytelnione punkty końcowe zwracają PII, poświadczenia lub tokeny CSRF, które **mogą** zostać odczytane przez nieuprawnione źródło  

---