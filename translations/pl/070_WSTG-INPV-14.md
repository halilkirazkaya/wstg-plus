## WSTG-INPV-14 — Testowanie pod kątem podatności inkubowanych (Incubated Vulnerabilities)

Podatności inkubowane (Incubated Vulnerabilities), określane również jako podatności trwałe (persistent) lub drugiego rzędu (second-order), występują, gdy złośliwy ładunek (Payload) jest przechowywany przez aplikację w stanie „uśpionym”, a następnie wykonywany lub przetwarzany w innym kontekście. Ten wzorzec ataku zazwyczaj celuje w systemy back-endowe, konsole administracyjne lub zautomatyzowane narzędzia raportujące, które pobierają dane ze współdzielonej bazy danych lub systemu plików bez odpowiedniej ponownej walidacji lub kodowania wyjściowego (output encoding). Pentesterzy muszą śledzić cykl życia danych wejściowych użytkownika od punktu wejścia („inkubatora”) do każdego miejsca, w którym te dane są ostatecznie renderowane lub wykorzystywane przez inne komponenty lub aplikacje pomocnicze. Skuteczna eksploatacja (Exploit) często prowadzi do skutków o wysokim priorytecie, takich jak przejęcie sesji administracyjnej (administrative session hijacking) poprzez Stored XSS lub eksfiltracja danych za pomocą SQL Injection drugiego rzędu w wewnętrznych modułach raportowych.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Status testu** | Nieprzeprowadzono |
| **Severity** | Wysoka |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Narzędzia:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Czy istnieją punkty końcowe (endpoints), w których dane wejściowe kontrolowane przez użytkownika są przechowywane w celu późniejszego pobrania przez innych użytkowników lub procesy?
- [ ] Nie — aplikacja **nie** przechowuje danych wejściowych użytkownika do późniejszego wykorzystania  
- [ ] Tak — dane wejściowe **są** przechowywane w polach bazy danych, plikach dziennika (log files) lub ustawieniach konfiguracji  

### Czy walidacja danych wejściowych (input validation) lub sanityzacja (sanitization) jest stosowana przed zapisaniem danych do warstwy trwałego przechowywania?
- [ ] Tak — ścisła walidacja oparta na białej liście (allow-list) **jest stosowana** w punkcie wejścia  
- [ ] Tak — ogólna sanityzacja **jest stosowana**, ale obejście (bypass) **jest możliwe** poprzez kodowanie lub niestandardowe znaki  
- [ ] Nie — dane wejściowe są przechowywane w ich surowej, niewalidowanej formie  

### Czy przechowywane dane są odpowiednio kodowane lub sanityzowane, gdy są renderowane w interfejsach administracyjnych lub pomocniczych?
- [ ] Tak — kodowanie wyjściowe (output encoding) uwzględniające kontekst **jest stosowane** wszędzie tam, gdzie dane są renderowane  
- [ ] Tak — kodowanie **jest stosowane** w niektórych miejscach, ale **brakuje** go w innych (np. wewnętrzny panel administracyjny)  
- [ ] Nie — dane są renderowane bez żadnego kodowania ani sanityzacji, co pozwala na wykonanie ładunku (Payload)  

### Czy ładunki (Payloads) przechowywane w bazie danych mogą wpływać na procesy wsadowe (batch processes) back-endu lub systemy monitorowania typu out-of-band (OOB)?
- [ ] Nie — procesy back-endowe używają bezpiecznych metod parsowania lub zapytań parametryzowanych  
- [ ] Tak — przechowywane ładunki **mogą** wywoływać interakcje w logice back-endu (np. CSV injection, command injection w logach)  
- [ ] Tak — przechowywane ładunki **mogą** wywoływać żądania out-of-band (OOB) do serwerów kontrolowanych przez atakującego  

---