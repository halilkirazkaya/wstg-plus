## WSTG-INPV-10 — IMAP SMTP Injection

Podatności typu IMAP and SMTP injection powstają, gdy aplikacja internetowa niewłaściwie filtruje dane dostarczane przez użytkownika przed ich włączeniem do poleceń przesyłanych do serwera pocztowego. Poprzez wstrzyknięcie znaków Carriage Return i Line Feed (CRLF), atakujący może zakończyć zamierzone polecenie i dołączyć dowolne instrukcje pocztowe, takie jak dodanie dodatkowych odbiorców (CC/BCC) lub modyfikacja treści wiadomości. Błędy te najczęściej występują w bramkach web-to-mail, formularzach kontaktowych oraz klientach pocztowych komunikujących się bezpośrednio z serwerami poczty za pomocą protokołów takich jak IMAP4 lub SMTP. Skuteczna eksploatacja pozwala na nieautoryzowany relay spamu, eksfiltrację poufnej zawartości e-maili, a w niektórych przypadkach na całkowite naruszenie integralności serwera pocztowego.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Status testu** | Niewykonany |
| **Severity** | Wysoki |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Czy aplikacja przetwarza dane wejściowe dostarczone przez użytkownika w celu konstruowania poleceń IMAP lub SMTP?
- [ ] Nie — aplikacja **nie** wchodzi w interakcję z serwerami pocztowymi poprzez dane wejściowe użytkownika  
- [ ] Tak — aplikacja wchodzi w interakcję z serwerami pocztowymi, ale korzysta z bezpiecznego API lub biblioteki  
- [ ] Tak — aplikacja konstruuje surowe polecenia pocztowe przy użyciu parametrów kontrolowanych przez użytkownika  

### Czy dane wejściowe są walidowane w celu zapobiegania wstrzykiwaniu sekwencji Carriage Return (`\r`) i Line Feed (`\n`)?
- [ ] Tak — znaki CRLF są **ściśle** filtrowane, kodowane lub odrzucane  
- [ ] Tak — dane wejściowe są walidowane względem ścisłej białej listy (allowlist) znaków  
- [ ] Nie — znaki CRLF **nie** są filtrowane, co pozwala na zakończenie polecenia i wstrzyknięcie (injection)  

### Czy atakujący może wstrzyknąć dodatkowe nagłówki wiadomości, takie jak `Bcc:` lub `Cc:`?
- [ ] Nie — modyfikacja nagłówków **nie jest możliwa** ze względu na ścisłe ograniczenia danych wejściowych  
- [ ] Tak — wstrzyknięcie dodatkowych nagłówków **jest możliwe** poprzez sekwencje CRLF  
- [ ] Tak — relay poczty do domen zewnętrznych **jest włączony** i możliwy do wykorzystania *(Krytyczne)*  

### Czy aplikacja pozwala na manipulację poleceniami IMAP w celu uzyskania dostępu do nieautoryzowanych skrzynek pocztowych?
- [ ] Nie — aplikacja **nie** korzysta z IMAP lub ściśle ogranicza dostęp do skrzynki dla uwierzytelnionego użytkownika  
- [ ] Tak — IMAP command injection **jest możliwe**, ale ograniczone do enumeracji metadanych  
- [ ] Tak — IMAP command injection pozwala na odczyt lub usunięcie wiadomości e-mail innych użytkowników  

### Czy serwer pocztowy backendu jest podatny na command pipelining?
- [ ] Nie — serwer pocztowy odrzuca polecenia wsadowe lub wiele poleceń w jednej sesji  
- [ ] Tak — serwer pocztowy przetwarza wiele poleceń wstrzykniętych za pośrednictwem pojedynczego pola wejściowego  

---