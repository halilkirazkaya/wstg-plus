## WSTG-SESS-02 — Testowanie atrybutów plików cookie

Zarządzanie sesją często opiera się na plikach cookie HTTP w celu utrzymania stanu, co sprawia, że atrybuty bezpieczeństwa tych plików są głównym celem atakujących. Pominięcie lub błędna konfiguracja atrybutów takich jak `HttpOnly`, `Secure` i `SameSite` naraża tokeny sesyjne na kradzież poprzez ataki typu Cross-Site Scripting (XSS), przechwycenie w kanałach nieszyfrowanych lub ataki Cross-Site Request Forgery (CSRF). Pentesterzy badają te flagi, aby ustalić, czy atakujący może wyekstrahować identyfikatory sesji z modelu obiektowego dokumentu (DOM) przeglądarki lub wymusić na przeglądarce ich wysłanie w żądaniach między źródłami (cross-origin). Prawidłowa konfiguracja atrybutów jest fundamentalnym środkiem strategii głębokiej obrony (defense-in-depth), zapewniającym, że wrażliwe tokeny pozostają ograniczone do autoryzowanych, bezpiecznych kontekstów.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Status testu** | Nie przeprowadzono |
| **Poziom krytyczności** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Narzędzia:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analiza implementacji flag plików cookie
| Atrybut | Obecny | Brak |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Czy flaga `HttpOnly` jest stosowana dla wrażliwych plików cookie sesji?
- [ ] Tak — zastosowano dla **wszystkich** wrażliwych plików cookie sesji *(Najbardziej bezpieczne)*  
- [ ] Tak — zastosowano tylko dla niektórych plików cookie sesji  
- [ ] Nie — flaga **nie została zastosowana**, co umożliwia dostęp poprzez skrypty po stronie klienta *(Krytyczne)*  

### Czy flaga `Secure` jest wymuszana dla plików cookie przesyłanych przez HTTPS?
- [ ] Tak — zastosowano dla **wszystkich** plików cookie przesyłanych przez TLS  
- [ ] Nie — flaga **nie została zastosowana**, co pozwala na przesyłanie plików cookie otwartym tekstem przez HTTP  

### Czy atrybut `SameSite` jest używany do mitygacji ryzyka CSRF?
- [ ] Tak — ustawiony na `Strict` lub `Lax` dla plików cookie sesji  
- [ ] Tak — ustawiony na `None`, ale z obecną flagą `Secure`  
- [ ] Nie — atrybutu **brakuje** lub jest ustawiony na `None` **bez** flagi `Secure`  

### Czy atrybuty `Domain` i `Path` są ograniczone do minimalnego niezbędnego zakresu?
- [ ] Tak — ograniczone do **konkretnej** subdomeny i ścieżki aplikacji  
- [ ] Nie — zakres jest zbyt **szeroki** (np. ustawiony na domenę nadrzędną lub root `/`), co zwiększa powierzchnię ataku  

### Czy wrażliwe pliki cookie sesji wygasają prawidłowo poprzez atrybuty `Expires` lub `Max-Age`?
- [ ] Tak — ustawiono odpowiednie daty wygaśnięcia  
- [ ] Nie — pliki cookie są trwałe (persistent) z nadmiernie **długim** czasem życia  
- [ ] Nie — pliki cookie są tylko sesyjne, ale **brakuje** wymuszenia limitu czasu (timeout) po stronie serwera  

---