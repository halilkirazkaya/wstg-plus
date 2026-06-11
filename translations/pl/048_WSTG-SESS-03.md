## WSTG-SESS-03 — Session Fixation

Utrwalanie sesji (Session Fixation) występuje, gdy aplikacja nie unieważnia lub nie rotuje identyfikatora sesji po pomyślnym uwierzytelnieniu użytkownika, co pozwala atakującemu na narzucenie ofierze znanego tokena sesji. Jeśli aplikacja zachowuje identyfikator sesji sprzed uwierzytelnienia po zalogowaniu, atakujący może dostarczyć ofierze specjalnie przygotowany link zawierający ustalony identyfikator sesji, a następnie przejąć uwierzytelnioną sesję. Podatność ta zazwyczaj objawia się w formularzach logowania lub poprzez identyfikatory sesji akceptowane w parametrach URL oraz plikach cookies. Z perspektywy atakującego, eksploatacja polega na "ustaleniu" (fixingu) sesji za pomocą złośliwego linku lub podatności typu header injection i oczekiwaniu, aż ofiara poda dane uwierzytelniające, co skutecznie eliminuje potrzebę kradzieży aktywnego tokena.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Status Testu** | Nie przeprowadzono |
| **Krytyczność** | Wysoka |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Czy identyfikator sesji zmienia się po pomyślnym uwierzytelnieniu?
- [ ] Tak — wydawany jest nowy identyfikator sesji, a stary zostaje unieważniony *(Najbezpieczniejsze)*  
- [ ] Tak — wydawany jest nowy identyfikator sesji, ale stary pozostaje ważny  
- [ ] Nie — identyfikator sesji pozostaje taki sam przed i po uwierzytelnieniu *(Krytyczne)*  

### Czy aplikacja akceptuje identyfikatory sesji przekazywane w parametrach URL?
- [ ] Nie — identyfikatory sesji są zarządzane wyłącznie (exclusively) przez pliki cookies  
- [ ] Tak — identyfikatory sesji są akceptowane w adresie URL, ale są ignorowane lub rotowane podczas logowania  
- [ ] Tak — identyfikatory sesji w adresie URL są akceptowane i zachowywane po uwierzytelnieniu  

### Czy identyfikator sesji zdefiniowany przez atakującego może zostać narzucony aplikacji?
- [ ] Nie — aplikacja odrzuca nieprawidłowe lub nieistniejące identyfikatory sesji dostarczone przez użytkownika  
- [ ] Tak — aplikacja akceptuje i inicjuje sesję przy użyciu dowolnego identyfikatora podanego w ciasteczku  
- [ ] Tak — aplikacja akceptuje i inicjuje sesję przy użyciu dowolnego identyfikatora podanego w parametrze URL  

### Czy sesje sprzed uwierzytelnienia są poprawnie kończone?
- [ ] Tak — anonimowa sesja jest w pełni niszczona (fully destroyed) podczas zdarzenia logowania  
- [ ] Nie — anonimowa sesja nie jest niszczona, co umożliwia potencjalne zbieranie sesji (session harvesting)  

### Czy ciasteczko sesyjne jest chronione przed wstrzyknięciem po stronie klienta?
- [ ] Tak — flagi `HttpOnly` i `Secure` są stosowane, aby zapobiec utrwalaniu sesji za pomocą skryptów  
- [ ] Nie — flagi nie są stosowane, co pozwala na ustawianie identyfikatorów sesji poprzez Cross-Site Scripting (XSS)  

---