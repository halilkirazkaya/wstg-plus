## WSTG-ATHN-03 — Testowanie pod kątem słabych mechanizmów blokady konta

Mechanizm blokady konta jest krytycznym elementem strategii obrony w głąb (defense-in-depth), zaprojektowanym w celu mitygacji ataków typu Brute Force oraz Credential Stuffing poprzez dezaktywację konta po określonej liczbie nieudanych prób uwierzytelnienia. Bez solidnej polityki blokad, napastnicy mogą używać zautomatyzowanych narzędzi do systematycznego odgadywania haseł dla wielu kont (Password Spraying) lub atakować pojedyncze konto o wysokiej wartości aż do skutku. Podatność ta zazwyczaj występuje w portalach logowania, formularzach resetowania hasła oraz endpointach API, które nie posiadają Rate Limiting lub ochrony na poziomie konta. Z perspektywy atakującego, słaby lub nieobecny mechanizm blokady pozwala na nieskończoną liczbę prób łamania hasła, podczas gdy zbyt agresywny mechanizm może zostać wykorzystany do wywołania rozproszonej odmowy usługi (DoS) przeciwko legalnym użytkownikom poprzez celowe blokowanie ich kont.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Status testu** | Nie wykonano |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się Wysoki, jeśli aplikacja przetwarza wrażliwe dane osobowe (PII), dane finansowe lub jeśli konta administracyjne są podatne na ataki Brute Force bez żadnych dodatkowych mechanizmów kontrolnych.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Narzędzia:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Czy mechanizm blokady konta jest wymuszany po określonej liczbie nieudanych prób?
- [ ] Tak — konto jest blokowane po zdefiniowanym, rozsądnym progu (np. 5-10 prób)  
- [ ] Tak — konto jest blokowane, ale dopiero po nadmiernie wysokiej liczbie prób  
- [ ] Nie — konto **nie może** zostać zablokowane, co pozwala na nieograniczony atak Brute Force  

### Czy mechanizm blokady można pominąć za pomocą powszechnych technik unikania zabezpieczeń?
- [ ] Nie — blokada jest wymuszana po stronie serwera (server-side) i zmiany po stronie klienta (client-side) **nie mają** na nią wpływu  
- [ ] Tak — blokadę **można** pominąć poprzez rotację adresów IP lub użycie puli serwerów proxy  
- [ ] Tak — blokadę **można** pominąć poprzez manipulację nagłówkami HTTP, takimi jak `X-Forwarded-For` lub `User-Agent`  
- [ ] Tak — blokadę **można** pominąć poprzez czyszczenie plików cookie lub rozpoczynanie nowych sesji  

### Czy mechanizm blokady zapewnia ścieżkę odzyskiwania konta lub wygaśnięcia blokady?
- [ ] Tak — konto jest automatycznie odblokowywane po określonym czasie lub poprzez weryfikację e-mail  
- [ ] Tak — odblokowanie konta wymaga interwencji administratora  
- [ ] Nie — blokada jest stała i nie posiada jasnej ścieżki odzyskiwania, co zwiększa ryzyko DoS  

### Czy aplikacja rozróżnia prawidłową i nieprawidłową nazwę użytkownika podczas blokady?
- [ ] Nie — odpowiedź aplikacji jest identyczna zarówno dla istniejących, jak i nieistniejących użytkowników  
- [ ] Tak — aplikacja ujawnia, czy konto jest zablokowane, co pozwala na enumerację użytkowników (Username Enumeration)  

### Czy mechanizm blokady jest podatny na atak odmowy usługi (DoS)?
- [ ] Nie — mechanizmy kontrolne, takie jak CAPTCHA lub Rate Limiting oparty na IP, zapobiegają masowemu blokowaniu kont  
- [ ] Tak — atakujący **może** systematycznie blokować wszystkich znanych użytkowników, podając błędne hasła  

---