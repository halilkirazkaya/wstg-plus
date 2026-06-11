## WSTG-ATHZ-02 — Testowanie pod kątem omijania schematów autoryzacji

Omijanie schematów autoryzacji występuje, gdy aplikacja nie wymusza mechanizmów kontroli dostępu, co umożliwia atakującemu uzyskanie dostępu do zasobów lub funkcji poza jego zamierzonymi uprawnieniami. Luka ta zazwyczaj objawia się jako pozioma eskalacja uprawnień (Horizontal Privilege Escalation), w której atakujący uzyskuje dostęp do danych należących do innego użytkownika o tym samym poziomie uprawnień, lub pionowa eskalacja uprawnień (Vertical Privilege Escalation), gdzie użytkownik o niskich uprawnieniach zyskuje możliwości administracyjne. Atakujący wykorzystują te błędy poprzez manipulowanie parametrami żądań, takimi jak identyfikatory zasobów (Resource IDs), modyfikowanie tokenów sesyjnych lub wykorzystywanie nagłówków HTTP, takich jak `X-Original-URL`, w celu obejścia ograniczeń opartych na ścieżkach. Zapewnienie solidnej autoryzacji jest kluczowe dla zachowania poufności danych i zapobiegania nieautoryzowanym działaniom administracyjnym, które mogłyby naruszyć bezpieczeństwo całego systemu.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Wysoki / Krytyczny |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Narzędzia:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Czy zablokowano poziomą eskalację uprawnień między użytkownikami o tej samej roli?
- [ ] Tak — kontrole autoryzacji **są stosowane** przy każdym żądaniu zasobu, a ich obejście **nie jest możliwe**  
- [ ] Tak — kontrole autoryzacji **są stosowane**, ale można je obejść poprzez IDOR lub manipulację parametrami  
- [ ] Nie — użytkownicy **mogą** uzyskać dostęp do danych należących do innych użytkowników poprzez zwykłą zmianę identyfikatora zasobu *(Wysoki)*  

### Czy zablokowano pionową eskalację uprawnień dla użytkowników o niskich uprawnieniach?
- [ ] Nie — aplikacja posiada tylko jeden poziom uprawnień  
- [ ] Tak — funkcje administracyjne **nie są** dostępne dla użytkowników o niskich uprawnieniach  
- [ ] Tak — funkcje administracyjne są ukryte w interfejsie użytkownika, ale **można** uzyskać do nich dostęp poprzez bezpośredni adres URL  
- [ ] Nie — użytkownicy o niskich uprawnieniach **mogą** wykonywać czynności administracyjne poprzez manipulowanie rolami lub uprawnieniami *(Krytyczny)*  

### Czy autoryzację można obejść za pomocą manipulacji metodami HTTP (HTTP verb tampering)?
- [ ] Nie — mechanizmy kontroli dostępu są wymuszane niezależnie od użytej metody HTTP  
- [ ] Tak — zmiana metody (np. z `GET` na `POST` lub `PUT`) **omija** kontrole autoryzacji  

### Czy punkty końcowe administracyjne lub o ograniczonym dostępie są dostępne bez uwierzytelnienia?
- [ ] Nie — wszystkie wrażliwe punkty końcowe wymagają poprawnej sesji i odpowiednich uprawnień  
- [ ] Tak — niektóre administracyjne punkty końcowe są dostępne, jeśli atakujący zna bezpośredni adres URL  
- [ ] Tak — nieautoryzowany dostęp do wrażliwych funkcji **jest możliwy** *(Krytyczny)*  

### Czy nagłówki HTTP pozwalają na obejście autoryzacji opartej na ścieżkach?
- [ ] Nie — aplikacja **nie jest** podatna na obejścia oparte na nagłówkach  
- [ ] Tak — nagłówki takie jak `X-Original-URL`, `X-Rewrite-URL` lub `X-Forwarded-For` **mogą** zostać użyte do obejścia mechanizmów kontroli dostępu  

---