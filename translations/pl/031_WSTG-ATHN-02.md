## WSTG-ATHN-02 — Testowanie pod kątem domyślnych danych uwierzytelniających

Testowanie pod kątem domyślnych danych uwierzytelniających (Default Credentials) polega na identyfikacji interfejsów administracyjnych, usług lub komponentów aplikacji, które nadal korzystają z fabrycznie ustawionych lub dostarczonych przez dostawcę nazw użytkowników i haseł. Ta podatność jest celem o wysokim priorytecie dla atakujących, ponieważ często zapewnia bezpośrednią ścieżkę do pełnego przejęcia systemu, dostępu administracyjnego lub zdalnego wykonania kodu (Remote Code Execution) przy minimalnym wysiłku. Zazwyczaj występuje w gotowym oprogramowaniu (off-the-shelf software), platformach CMS, narzędziach do zarządzania bazami danych oraz zintegrowanym sprzęcie, takim jak urządzenia IoT lub urządzenia sieciowe. Eksploatacja (Exploit) jest zazwyczaj przeprowadzana poprzez porównanie zidentyfikowanych wersji oprogramowania z publicznymi bazami domyślnych haseł lub przy użyciu zautomatyzowanych narzędzi do Credential Stuffing podczas początkowych faz rozpoznania i eksploatacji.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Krytyczny / Wysoki |

**Odnośniki:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Narzędzia:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Czy interfejsy administracyjne lub do zarządzania są wystawione do Internetu lub niezaufanych segmentów sieci?
- [ ] Nie — żadne interfejsy zarządzania nie są dostępne  
- [ ] Tak — interfejsy są obecne, ale ograniczone przez mechanizmy kontroli na poziomie sieci (np. VPN, IP Allowlist)  
- [ ] Tak — interfejsy **są** publicznie dostępne i wymagają uwierzytelnienia  

### Czy domyślne dane uwierzytelniające dostarczone przez dostawcę zostały zmienione dla wszystkich zidentyfikowanych komponentów?
- [ ] Tak — wszystkie domyślne dane uwierzytelniające **zostały zmienione** we wszystkich komponentach *(Najbezpieczniejsze)*  
- [ ] Tak — dane uwierzytelniające **zostały zmienione** dla głównej aplikacji, ale komponenty poboczne (np. CMS, DB) pozostają domyślne  
- [ ] Nie — domyślne dane uwierzytelniające **są aktywne** na jednym lub większej liczbie możliwych do zidentyfikowania interfejsów *(Krytyczne)*  

### Czy aplikacja wymusza zmianę hasła przy pierwszym logowaniu dla nowych lub administracyjnych kont?
- [ ] Tak — zmiana hasła **jest obowiązkowa** przed podjęciem jakichkolwiek działań administracyjnych  
- [ ] Nie — aplikacja **pozwala** na korzystanie z domyślnych danych uwierzytelniających bezterminowo  

### Czy mechanizmy blokady konta lub ograniczania liczby żądań (Rate Limiting) są aktywne, aby zapobiec zautomatyzowanemu testowaniu domyślnych danych uwierzytelniających?
- [ ] Tak — stosowane jest rygorystyczne Rate Limiting lub blokada konta (Lockout)  
- [ ] Tak — mechanizmy kontrolne są obecne, ale możliwe jest ich **obejście** poprzez manipulację nagłówkami lub rotację adresów IP  
- [ ] Nie — żadne mechanizmy Rate Limiting ani blokady konta **nie są włączone**  

### Czy wtyczki podmiotów trzecich, oprogramowanie pośredniczące (Middleware) lub narzędzia administracyjne (np. phpMyAdmin, Tomcat Manager) używają unikalnych danych uwierzytelniających?
- [ ] Nie — komponenty podmiotów trzecich nie występują w środowisku  
- [ ] Tak — wszystkie komponenty podmiotów trzecich używają unikalnych, niedomyślnych danych uwierzytelniających  
- [ ] Nie — co najmniej jeden komponent podmiotu trzeciego **jest dostępny** przy użyciu znanych domyślnych danych uwierzytelniających  

---