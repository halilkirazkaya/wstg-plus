## WSTG-ATHN-09 — Testowanie słabych mechanizmów zmiany lub resetowania hasła

Mechanizmy zmiany i resetowania hasła są krytycznymi komponentami zarządzania uwierzytelnianiem, które w przypadku niewłaściwej implementacji pozwalają atakującym na przejęcie kont użytkowników. Luki te zazwyczaj obejmują przewidywalne tokeny resetowania, brak blokad kont podczas prób Brute Force, niebezpieczne dostarczanie tokenów lub możliwość zmiany hasła bez weryfikacji bieżącego hasła. Atakujący wykorzystują te błędy, przechwytując lub odgadując linki odzyskiwania, wykonując Host Header Injection w celu przekierowania wiadomości e-mail z resetowaniem lub wykorzystując Session Fixation w celu utrzymania dostępu po zmianie hasła. Zapewnienie, że procesy te wymagają silnej weryfikacji i wykorzystują kryptograficzną losowość, jest niezbędne dla zachowania integralności konta i zapobiegania nieautoryzowanemu dostępowi.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Status testu** | Nie przeprowadzono |
| **Krytyczność** | Wysoka / Krytyczna |

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Czy bieżące hasło jest wymagane do ustawienia nowego hasła podczas standardowej zmiany?
- [ ] Tak — bieżące hasło **jest wymagane** i weryfikowane  
- [ ] Tak — bieżące hasło **jest wymagane**, ale można je pominąć poprzez Parameter Pollution lub manipulację parametrami  
- [ ] Nie — bieżące hasło **nie jest** wymagane, co umożliwia przejęcie konta poprzez Session Hijacking *(Wysoki)*  

### Czy tokeny resetowania hasła są bezpieczne kryptograficznie i nieprzewidywalne?
- [ ] Tak — tokeny charakteryzują się wysoką entropią, są losowe i **nieprzewidywalne**  
- [ ] Tak — tokeny są generowane, ale wykazują niską entropię lub przewidywalne wzorce (np. oparte na znaczniku czasu)  
- [ ] Nie — tokeny są **przewidywalne** lub oparte na statycznych danych użytkownika, takich jak adres e-mail/ID zakodowane w Base64 *(Krytyczny)*  

### Czy link do resetowania hasła może zostać przekierowany poprzez Host Header Injection?
- [ ] Nie — aplikacja ignoruje lub waliduje nagłówek `Host` podczas generowania linku  
- [ ] Tak — aplikacja używa nagłówka `Host` lub `X-Forwarded-Host` do konstruowania linków, ale walidacja **jest obecna**  
- [ ] Tak — linki **mogą** zostać przekierowane do domeny kontrolowanej przez atakującego poprzez manipulację nagłówkami *(Wysoki)*  

### Czy token resetowania ma ograniczony czas życia i jest natychmiast unieważniany?
- [ ] Tak — tokeny wygasają szybko i są unieważniane **natychmiast** po użyciu lub zmianie hasła  
- [ ] Tak — tokeny wygasają po długim czasie (np. 24+ godziny) lub pozwalają na **wielokrotne użycie**  
- [ ] Nie — tokeny **nigdy nie wygasają** lub pozostają ważne po pomyślnej zmianie hasła  

### Czy punkty końcowe resetowania i zmiany hasła posiadają mechanizmy Rate Limiting lub blokady?
- [ ] Tak — rygorystyczny Rate Limiting oraz mechanizmy CAPTCHA **są stosowane**, aby zapobiec enumeracji i atakom Brute Force  
- [ ] Tak — istnieją ograniczone mechanizmy kontrolne, ale ich **ominięcie jest możliwe** poprzez rotację adresów IP lub Header Spoofing  
- [ ] Nie — Rate Limiting **nie jest stosowany**, co pozwala na masową enumerację kont lub Brute Force tokenów  

---