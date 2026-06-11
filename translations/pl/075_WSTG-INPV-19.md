## WSTG-INPV-19 — Testowanie podatności na Server-Side Request Forgery (SSRF)

Podatność Server-Side Request Forgery (SSRF) występuje, gdy atakujący może wpłynąć na aplikację webową, aby ta wykonywała nieautoryzowane żądania z serwera do zasobów wewnętrznych lub zewnętrznych. Poprzez manipulację parametrami oczekującymi adresów URL, nazw hostów lub adresów IP, atakujący może obejść mechanizmy kontroli na poziomie sieci, takie jak zapory sieciowe (firewalle) i listy ACL, aby skanować segmenty sieci wewnętrznej, uzyskiwać dostęp do usług metadanych w chmurze (IMDS) lub wchodzić w interakcję z podatnymi wewnętrznymi interfejsami API i bazami danych. Ta podatność zazwyczaj objawia się w funkcjonalnościach obejmujących pobieranie obrazów, generowanie plików PDF, webhooks lub przesyłanie plików za pomocą adresu URL. Z perspektywy atakującego, SSRF jest potężnym prymitywem wykorzystywanym do przemieszczania się w głąb izolowanych środowisk (pivot), eksfiltracji poufnych danych konfiguracyjnych lub potencjalnego uzyskania zdalnego wykonania kodu (Remote Code Execution, RCE) poprzez interakcję z usługami wewnętrznymi, takimi jak Redis lub Memcached.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Status testu** | Nie wykonano |
| **Krytyczność** | Wysoka / Krytyczna |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Narzędzia:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### Czy aplikacja przetwarza adresy URL lub nazwy hostów dostarczone przez użytkownika?
- [ ] Nie — żadna funkcjonalność nie przyjmuje zewnętrznych adresów URL ani nazw hostów  
- [ ] Tak — funkcjonalność istnieje, ale dane wejściowe **są rygorystycznie walidowane** względem listy dozwolonych (allow-list)  
- [ ] Tak — funkcjonalność istnieje i przyjmuje adresy URL dostarczone przez użytkownika  

### Czy stosowane są mechanizmy walidacji lub czarne listy (deny-lists) dla żądanego celu?
- [ ] Tak — wymuszana jest rygorystyczna **lista dozwolonych** (allow-list) zaufanych domen/adresów IP  
- [ ] Tak — stosowana jest **lista blokowanych** (deny-list) (np. blokowanie `127.0.0.1`, `169.254.169.254`)  
- [ ] Nie — do danych wejściowych **nie jest stosowana żadna walidacja** ani filtrowanie  

### Czy możliwe jest obejście filtrów poprzez kodowanie, przekierowania lub DNS rebinding?
- [ ] Nie — filtry są solidne i bezpiecznie obsługują przekierowania oraz rozwiązywanie nazw DNS  
- [ ] Tak — obejście **jest możliwe** przy użyciu kodowania URL lub alternatywnych formatów adresów IP (hex, octal)  
- [ ] Tak — obejście **jest możliwe** poprzez przekierowania HTTP 3xx do celów wewnętrznych  
- [ ] Tak — obejście **jest możliwe** poprzez ataki DNS rebinding w celu rozwiązania nazw na wewnętrzne adresy IP  

### Czy aplikacja może połączyć się z wewnętrznymi usługami metadanych lub interfejsami lokalnymi?
- [ ] Nie — sieć lokalna i punkty końcowe metadanych w chmurze są **nieosiągalne**  
- [ ] Tak — dostęp do localhost (`127.0.0.1`) lub usług pętli zwrotnej (loopback) **jest możliwy**  
- [ ] Tak — dostęp do usług metadanych w chmurze (np. AWS/GCP/Azure IMDS) **jest możliwy** *(Krytyczne)*  

### Jaki jest charakter odpowiedzi SSRF?
- [ ] Blind — użytkownik nie otrzymuje żadnej odpowiedzi ani metadanych  
- [ ] Partial — metadane odpowiedzi (nagłówki, rozmiar, czas trwania) są zwracane użytkownikowi  
- [ ] Full — pełna treść odpowiedzi z zasobu wewnętrznego **jest odzwierciedlona** w odpowiedzi dla użytkownika  

---