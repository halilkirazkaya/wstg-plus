## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Podatności typu Directory Traversal oraz File Inclusion powstają, gdy aplikacja wykorzystuje dane wejściowe kontrolowane przez użytkownika do konstruowania ścieżek do plików lub katalogów bez wystarczającej walidacji lub sanityzacji. Atakujący wykorzystują te błędy, wstrzykując sekwencje takie jak `../` w celu wyjścia poza zamierzony katalog, co potencjalnie pozwala na dostęp do wrażliwych plików systemowych, danych konfiguracyjnych lub kodu źródłowego aplikacji. W poważniejszych przypadkach, obejmujących Local File Inclusion (LFI) lub Remote File Inclusion (RFI), atakujący może doprowadzić do zdalnego wykonania kodu poprzez dołączenie złośliwych skryptów lub wykorzystanie technik takich jak log poisoning i PHP wrappers. Podatności te zazwyczaj występują w parametrach używanych do dynamicznego ładowania treści, silnikach szablonów lub punktach końcowych służących do pobierania obrazów, gdzie logika po stronie serwera operuje na ścieżkach systemu plików.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom zagrożenia** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Narzędzia:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Czy parametry przyjmują nazwy plików lub ścieżki do przetwarzania po stronie serwera?
- [ ] Nie — żadne parametry nie wydają się wchodzić w interakcję z systemem plików  
- [ ] Tak — parametry istnieją, ale używają ścisłej allowlista identyfikatorów plików  
- [ ] Tak — parametry przyjmują bezpośrednie nazwy plików lub ścieżki  

### Czy dane wejściowe są poddawane sanityzacji w celu zapobiegania sekwencjom directory traversal?
- [ ] Tak — dane wejściowe są walidowane względem ścisłej allowlista i sekwencje traversal **nie są możliwe**  
- [ ] Tak — dane wejściowe są sanityzowane poprzez usuwanie sekwencji `../`, ale rekurencyjny bypass **jest możliwy**  
- [ ] Nie — żadna sanityzacja ani walidacja **nie jest stosowana** do danych wejściowych powiązanych ze ścieżkami  

### Czy możliwe jest uzyskanie dostępu do plików poza ograniczonym katalogiem za pomocą sekwencji traversal?
- [ ] Nie — aplikacja lub system operacyjny zapobiega dostępowi do plików poza zdefiniowanym zakresem  
- [ ] Tak — dostęp do plików wewnątrz katalogu głównego WWW (web root) **jest możliwy**  
- [ ] Tak — dostęp do wrażliwych plików systemowych (np. `/etc/passwd`, `C:\Windows\win.ini`) **jest możliwy** *(Krytyczny)*  

### Czy serwer pozwala na dołączanie zdalnych adresów URL (RFI)?
- [ ] Nie — zdalne dołączanie plików (RFI) jest **wyłączone** na poziomie serwera/aplikacji  
- [ ] Tak — zdalne pliki **mogą** być dołączane, ale ich wykonanie **nie jest możliwe**  
- [ ] Tak — zdalne pliki **mogą** być dołączane i wykonywane na serwerze *(Krytyczny)*  

### Czy filtry mogą zostać pominięte przy użyciu kodowania lub znaków specjalnych?
- [ ] Nie — filtry skutecznie obsługują różne rodzaje kodowania i bajty zerowe (null bytes)  
- [ ] Tak — bypass **jest możliwy** przy użyciu URL encoding, double URL encoding lub 16-bit Unicode  
- [ ] Tak — bypass **jest możliwy** przy użyciu wstrzyknięcia bajtu zerowego (`%00`) lub wrapperów systemu plików (np. `php://filter`)  

---