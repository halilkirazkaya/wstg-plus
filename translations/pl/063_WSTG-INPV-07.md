## WSTG-INPV-07 — XML Injection

XML Injection (Wstrzykiwanie XML) występuje, gdy aplikacja włącza dane dostarczone przez użytkownika do dokumentu lub strumienia XML bez odpowiedniej walidacji, sanitizacji lub escapowania znaków specjalnych XML. Poprzez wstrzykiwanie określonych tagów XML lub elementów strukturalnych, atakujący może manipulować logiką dokumentu, omijać mechanizmy uwierzytelniania lub naruszać integralność danych przetwarzanych przez backend. Ta podatność jest powszechnie spotykana w usługach sieciowych opartych na protokole SOAP, API REST akceptujących XML, asercjach SAML oraz aplikacjach, które dynamicznie generują pliki konfiguracyjne XML lub raporty. Z perspektywy atakującego, celem jest wyjście poza zamierzony kontekst danych w celu zmiany struktury dokumentu, co potencjalnie prowadzi do nieautoryzowanej eskalacji uprawnień (privilege escalation) lub wykonania niezamierzonej logiki biznesowej.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Status testu** | Nieprzeprowadzony |
| **Krytyczność** | Wysoka / Średnia |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Narzędzia:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### Czy aplikacja akceptuje i przetwarza dane wejściowe w formacie XML od użytkownika?
- [ ] Nie — aplikacja **nie** przetwarza danych wejściowych XML  
- [ ] Tak — dane XML są przetwarzane przez API (SOAP/REST)  
- [ ] Tak — dane XML są przetwarzane poprzez przesyłanie plików (SVG, DOCX itp.)  

### Czy znaki specjalne XML (np. `<`, `>`, `&`, `'`, `"`) są odpowiednio escapowane lub sanitizowane?
- [ ] Tak — wszystkie znaki specjalne są **odpowiednio** escapowane lub sanitizowane *(Najbezpieczniejsze)*  
- [ ] Tak — stosowane jest filtrowanie, ale obejście **jest możliwe** przy użyciu kodowania  
- [ ] Nie — surowe dane wejściowe są osadzane bezpośrednio w strukturach XML **bez** sanitizacji  

### Czy walidacja schematu (XSD/DTD) po stronie serwera jest wymuszana dla wszystkich danych wejściowych XML?
- [ ] Tak — rygorystyczna walidacja schematu jest **włączona** i zapobiega zmianom strukturalnym  
- [ ] Tak — walidacja jest **włączona**, ale **nie** zapobiega wstrzykiwaniu tagów w polach danych  
- [ ] Nie — walidacja schematu jest **wyłączona** lub nie została zaimplementowana  

### Czy atakujący może wstrzyknąć nowe tagi XML w celu zmiany struktury dokumentu lub logiki?
- [ ] Nie — struktura pozostaje nienaruszona niezależnie od danych wejściowych  
- [ ] Tak — tagi mogą być wstrzykiwane, ale **nie mogą** zmieniać logiki biznesowej  
- [ ] Tak — wstrzykiwanie tagów **jest możliwe** i pozwala na obejście logiki lub modyfikację danych *(Krytyczne)*  

### Czy można manipulować parserem XML przy użyciu sekcji CDATA lub komentarzy XML?
- [ ] Nie — parser poprawnie obsługuje lub odrzuca znaczniki CDATA/komentarzy  
- [ ] Tak — wstrzyknięcie `<![CDATA[...]]>` lub `<!-- ... -->` **jest możliwe** w celu ukrycia danych lub obejścia filtrów  

---