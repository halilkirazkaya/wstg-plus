## WSTG-BUSL-08 — Testowanie przesyłania nieoczekiwanych typów plików

Przesyłanie nieoczekiwanych typów plików pozwala atakującym na obejście ograniczeń logiki biznesowej poprzez wysyłanie plików, których aplikacja nie zamierza przetwarzać, co potencjalnie prowadzi do zdalnego wykonania kodu (Remote Code Execution - RCE), Cross-Site Scripting (XSS) lub odmowy usługi (Denial of Service - DoS). Atakujący badają te podatności, modyfikując rozszerzenia plików, typy MIME i Magic Bytes, aby oszukać logikę walidacji po stronie serwera i skłonić ją do zaakceptowania złośliwej zawartości. Zjawisko to występuje zazwyczaj przy wgrywaniu zdjęć profilowych, w systemach zarządzania dokumentami lub załącznikach do zgłoszeń serwisowych (support tickets), gdzie walidacja po stronie backendu jest niewystarczająca. Udana eksploatacja może zagrozić serwerowi hostującemu, jeśli przesłany plik zostanie wykonany, lub prowadzić do ataków po stronie klienta, jeśli plik zostanie zaserwowany innym użytkownikom z nieprawidłowym nagłówkiem `Content-Type`.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Status testu** | Nie przeprowadzono |
| **Krytyczność** | Wysoka / Krytyczna |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Narzędzia:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Czy aplikacja ogranicza przesyłanie plików do określonego zestawu rozszerzeń?
- [ ] Tak — wymuszana jest ścisła biała lista (whitelist) rozszerzeń i obejście **nie jest możliwe**  
- [ ] Tak — biała lista rozszerzeń istnieje, ale obejście **jest możliwe** za pomocą podwójnych rozszerzeń lub bajtów null (null bytes)  
- [ ] Nie — każde rozszerzenie pliku **może** zostać przesłane  

### Czy zawartość pliku jest walidowana poza samym rozszerzeniem?
- [ ] Tak — logika po stronie serwera weryfikuje Magic Bytes i przeprowadza głęboką inspekcję treści  
- [ ] Tak — logika po stronie serwera weryfikuje typ MIME wyłącznie za pomocą nagłówka `Content-Type`  
- [ ] Nie — po sprawdzeniu rozszerzenia **nie jest stosowana** żadna walidacja zawartości  

### Czy atakujący może wykonać kod poprzez przesłanie web shella lub skryptu?
- [ ] Nie — przesłane pliki są przechowywane w katalogu bez uprawnień do wykonywania lub w kontenerze danych (S3/Azure Blob)  
- [ ] Tak — pliki są przechowywane w katalogu dostępnym przez WWW, ale wykonywanie **jest wyłączone** poprzez konfigurację serwera  
- [ ] Tak — przesłane złośliwe skrypty **mogą** być bezpośrednio wykonane za pomocą przewidywalnej ścieżki URL *(Krytyczna)*  

### Czy ataki po stronie klienta, takie jak XSS, są możliwe poprzez przesłane typy plików?
- [ ] Nie — pliki są serwowane z nagłówkami `Content-Disposition: attachment` i `X-Content-Type-Options: nosniff`  
- [ ] Tak — pliki SVG lub HTML **mogą** zostać przesłane i wyrenderowane w przeglądarce, co prowadzi do XSS  
- [ ] Tak — metadane obrazu (EXIF) **są przetwarzane** i odzwierciedlane w interfejsie użytkownika bez sanityzacji  

---