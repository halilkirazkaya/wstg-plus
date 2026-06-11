## WSTG-SESS-09 — Testowanie pod kątem przejmowania sesji (Session Hijacking)

Przejmowanie sesji (Session Hijacking) występuje, gdy atakujący przechwytuje, przewiduje lub wymusza (Session Fixation) prawidłowy identyfikator sesji (SID), aby uzyskać nieautoryzowany dostęp do aktywnej sesji użytkownika. Podatność ta zazwyczaj wynika z niewystarczającego bezpieczeństwa warstwy transportowej, braku flag bezpieczeństwa w plikach cookie lub przewidywalnych algorytmów generowania SID, co pozwala atakującemu na całkowite obejście uwierzytelniania. Z perspektywy atakującego, skuteczne wykorzystanie podatności umożliwia pełne podszycie się pod ofiarę bez konieczności posiadania jej danych uwierzytelniających, co często osiąga się poprzez sniffing sieciowy, Cross-Site Scripting (XSS) lub ataki typu session fixation.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Narzędzia:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Czy identyfikatory sesji są chronione podczas przesyłania?
- [ ] Tak — flaga `Secure` jest **włączona** i HSTS jest rygorystycznie wymuszany  
- [ ] Tak — flaga `Secure` jest **włączona**, ale HSTS jest **wyłączony**  
- [ ] Nie — flaga `Secure` **nie jest stosowana**, co pozwala na przechwycenie SID przez niezaszyfrowane kanały *(Wysoki)*  

### Czy identyfikator sesji jest chroniony przed dostępem skryptów po stronie klienta?
- [ ] Tak — flaga `HttpOnly` jest **stosowana** do wszystkich plików cookie powiązanych z sesją  
- [ ] Nie — flaga `HttpOnly` **nie jest stosowana**, co pozwala na eksfiltrację SID poprzez XSS *(Krytyczny)*  

### Czy aplikacja implementuje powiązanie sesji z atrybutami klienta?
- [ ] Tak — sesja jest powiązana z atrybutami klienta (IP/User-Agent) i ponowne użycie **nie jest możliwe**  
- [ ] Tak — powiązanie sesji (Session Binding) istnieje, ale możliwe jest **obejście** poprzez spoofing nagłówków  
- [ ] Nie — brak powiązania sesji; SID **jest ważny** przy użyciu z dowolnego źródła  

### Czy identyfikator sesji jest wystarczająco losowy, aby zapobiec przewidywaniu?
- [ ] Tak — SID wykorzystuje kryptograficznie bezpieczny generator liczb pseudolosowych (CSPRNG)  
- [ ] Nie — długość SID jest niewystarczająca lub następuje po **przewidywalnej** sekwencji/wzorcu  

### Czy sesje współbieżne są zarządzane w celu ograniczenia okna ataku?
- [ ] Nie — aplikacja rygorystycznie wymusza pojedynczą aktywną sesję na użytkownika  
- [ ] Tak — wiele współbieżnych sesji jest **włączonych**, ale są one monitorowane  
- [ ] Tak — możliwe jest posiadanie **nieograniczonej liczby współbieżnych sesji** na różnych urządzeniach/lokalizacjach  

---