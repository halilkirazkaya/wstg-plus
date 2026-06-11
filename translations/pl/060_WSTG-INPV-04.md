## WSTG-INPV-04 — Testowanie pod kątem HTTP Parameter Pollution

HTTP Parameter Pollution (HPP) występuje, gdy aplikacja otrzymuje wiele parametrów HTTP o tej samej nazwie i przetwarza je w sposób niespójny lub niebezpieczny. Dostarczając zduplikowane parametry, atakujący może manipulować wewnętrzną logiką aplikacji, potencjalnie omijając Web Application Firewalls (WAF), filtry walidacji danych wejściowych lub mechanizmy kontroli dostępu. Podatność ta jest silnie uzależniona od używanego serwera WWW lub frameworka aplikacji, który może wybrać pierwsze, ostatnie lub połączone (skonkatenowane) wystąpienie powtarzających się parametrów. Eksploatacja zazwyczaj celuje w endpointy przekazujące parametry do wewnętrznych interfejsów API, zapytań do baz danych lub mechanizmów przekierowań URL, co prowadzi do skutków od Cross-Site Scripting (XSS) po nieautoryzowaną eksfiltrację danych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Status testu** | Nie przeprowadzono |
| **Krytyczność** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Jak aplikacja obsługuje wiele parametrów HTTP o tej samej nazwie?
- [ ] Nie — zduplikowane parametry są **odrzucane** lub ignorowane przez serwer  
- [ ] Tak — serwer konsekwentnie wybiera tylko **pierwsze** lub **ostatnie** wystąpienie  
- [ ] Tak — serwer **łączy (konkatenuje)** wartości, potencjalnie umożliwiając wstrzyknięcie (injection)  

### Czy HPP można wykorzystać do obejścia zabezpieczeń, takich jak WAF lub filtry danych wejściowych?
- [ ] Nie — zabezpieczenia prawidłowo sprawdzają **wszystkie** wystąpienia parametru  
- [ ] Tak — WAF lub filtr sprawdza tylko **pierwsze** wystąpienie, pozwalając złośliwemu **drugiemu** wystąpieniu na przejście  
- [ ] Tak — konkatenacja pozwala atakującemu na **podzielenie** Payloadu pomiędzy wiele parametrów w celu uniknięcia wykrycia  

### Czy aplikacja odzwierciedla zanieczyszczone parametry w odpowiedzi bez odpowiedniej sanitacji?
- [ ] Nie — parametry są sanitowane lub kodowane przed odzwierciedleniem w interfejsie użytkownika (UI)  
- [ ] Tak — zanieczyszczenie skutkuje odzwierciedloną treścią, ale sanitacja **jest stosowana**  
- [ ] Tak — zanieczyszczenie skutkuje odzwierciedloną treścią, a sanitacja **nie jest stosowana**, co prowadzi do XSS lub błędów logicznych  

### Czy parametry przekazywane do wewnętrznych wywołań API lub systemów back-end są podatne na zanieczyszczenie?
- [ ] Nie — żądania back-endowe wykorzystują bezpieczne, niedynamiczne metody konstruowania  
- [ ] Tak — HPP pozwala atakującemu na **wstrzyknięcie** lub **nadpisanie** parametrów w strukturze żądania back-endowego  

### Czy aplikacja wykazuje inne zachowanie, gdy parametry są zanieczyszczone w żądaniach GET w porównaniu do POST?
- [ ] Nie — zachowanie jest spójne dla wszystkich metod HTTP  
- [ ] Tak — tylko parametry GET są podatne na zanieczyszczenie  
- [ ] Tak — tylko parametry POST (body) są podatne na zanieczyszczenie  

---