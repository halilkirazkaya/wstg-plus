## WSTG-IDNT-03 — Testowanie procesu aprowizacji kont (Account Provisioning Process)

Proces aprowizacji kont (Account Provisioning) zarządza sposobem tworzenia, weryfikacji i przypisywania uprawnień nowym tożsamościom w środowisku aplikacji. Atakujący biorą ten proces za cel, aby przeprowadzać zautomatyzowane masowe rejestracje w celu rozsyłania spamu lub ataków Denial-of-Service (DoS), omijać etapy weryfikacji tożsamości w celu tworzenia fałszywych kont lub manipulować parametrami, aby przyznać sobie podwyższone uprawnienia podczas fazy rejestracji. Podatności często objawiają się w formularzach samodzielnej rejestracji (Self-service), systemach dostępnych tylko na zaproszenie lub administracyjnych konsolach aprowizacji, gdzie błędy logiki biznesowej (Business Logic Flaws) pozwalają na nieautoryzowane tworzenie kont lub eskalację uprawnień (Privilege Escalation). Z perspektywy atakującego, skompromitowanie procesu aprowizacji zapewnia legalny punkt zaczepienia, który może zostać wykorzystany jako baza do głębszej eksploatacji, eksfiltracji danych lub ataków socjotechnicznych (Social Engineering).


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Status testu** | Nie wykonano |
| **Poziom ważności** | Średni / Wysoki* |

> *Poziom ważności staje się krytyczny (Critical), jeśli atakujący może utworzyć konta administracyjne lub obejść globalne ograniczenia rejestracji.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### Czy samodzielna rejestracja jest ściśle kontrolowana lub ograniczona do autoryzowanych użytkowników?
- [ ] Tak — rejestracja jest **wyłączona** lub wymaga ręcznego zatwierdzenia przez administratora  
- [ ] Tak — rejestracja jest otwarta, ale ograniczona do **autoryzowanych** domen e-mail lub tokenów zaproszeń  
- [ ] Nie — rejestracja jest **otwarta** dla każdego użytkownika bez ograniczeń dotyczących domen lub tokenów  

### Czy wymagany jest solidny mechanizm weryfikacji tożsamości (np. e-mail/SMS) przed aktywacją konta?
- [ ] Tak — weryfikacja jest **wymagana** i **nie może** zostać pominięta  
- [ ] Tak — weryfikacja jest **wymagana**, ale **można** ją pominąć poprzez manipulację parametrami lub przewidywalne tokeny  
- [ ] Nie — konta są **aktywne natychmiast** po przesłaniu formularza bez żadnej weryfikacji  

### Czy użytkownik może manipulować parametrami ról lub uprawnień podczas żądania rejestracji?
- [ ] Nie — role są **przypisywane po stronie serwera (Server-side)** i nie istnieją parametry po stronie klienta (Client-side)  
- [ ] Tak — parametry ról istnieją, ale manipulacja nimi jest **niemożliwa** ze względu na walidację po stronie serwera  
- [ ] Tak — parametry ról/uprawnień (np. `role=admin`, `is_staff=true`) **mogą** być modyfikowane w celu uzyskania podwyższonych uprawnień *(Krytyczne)*  

### Czy wdrożono limity zapytań (Rate Limiting) lub mechanizmy przeciwko automatyzacji, aby zapobiec masowemu tworzeniu kont?
- [ ] Tak — limity Rate Limiting i mechanizmy CAPTCHA są **aktywne** i skuteczne  
- [ ] Tak — mechanizmy kontrolne istnieją, ale ich **obejście jest możliwe** poprzez spoofing nagłówków lub usługi rozwiązywania CAPTCHA  
- [ ] Nie — **nie zastosowano** żadnych limitów Rate Limiting ani mechanizmów przeciwko automatyzacji  

### Czy proces aprowizacji wycieka informacje o istniejących kontach (Enumeracja użytkowników — User Enumeration)?
- [ ] Nie — komunikaty odpowiedzi są **identyczne** dla istniejących i nieistniejących użytkowników  
- [ ] Tak — różne odpowiedzi lub różnice w czasie odpowiedzi (Timing Variations) **pozwalają** na enumerację istniejących użytkowników  

---