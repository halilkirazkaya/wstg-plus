## WSTG-INFO-01 — Wykrywanie i rekonesans w wyszukiwarkach pod kątem wycieku informacji

Wykrywanie informacji w wyszukiwarkach polega na wykorzystywaniu publicznych wyszukiwarek, stron w pamięci podręcznej (cached pages) oraz usług indeksowania w celu zidentyfikowania danych, które organizacja docelowa nieumyślnie udostępniła. Atakujący wykorzystują zaawansowane operatory wyszukiwania (Google Dorks) oraz usługi indeksowania stron trzecich do wykrywania wrażliwych plików, katalogów, portali logowania, komunikatów o błędach oraz metadanych, które nie powinny być publicznie dostępne. Ta faza rekonesansu (reconnaissance) jest krytyczna, ponieważ nie wymaga bezpośredniej interakcji z celem, co czyni ją praktycznie niewykrywalną, a jednocześnie pozwala na potencjalne ujawnienie wysokiej wartości punktów wejścia, takich jak publicznie dostępne panele administracyjne, pliki konfiguracyjne, zrzuty baz danych (database dumps) czy dokumentacja wewnętrzna.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Status testu** | Nie wykonano |
| **Poziom zagrożenia** | Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Narzędzia:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Czy wrażliwe pliki lub katalogi są indeksowane przez wyszukiwarki?
- [ ] Nie — nie znaleziono wrażliwych treści w wynikach wyszukiwania  
- [ ] Tak — pliki konfiguracyjne, kopie zapasowe (backups) lub dokumenty wewnętrzne **są** indeksowane *(Krytyczne)*  

### Czy Google Dorking ujawnia publicznie dostępne interfejsy administracyjne lub panele logowania?
- [ ] Nie — panele administracyjne i strony logowania **nie są** indeksowane  
- [ ] Tak — panele administracyjne **są** indeksowane, ale wymagają silnego uwierzytelniania wieloskładnikowego (multi-factor authentication)  
- [ ] Tak — panele administracyjne **są** indeksowane i publicznie dostępne **bez** wystarczających mechanizmów kontroli  

### Czy wersje stron w pamięci podręcznej (cache) ujawniają wrażliwe dane, które zostały usunięte?
- [ ] Nie — nie znaleziono wrażliwych danych w migawkach pamięci podręcznej  
- [ ] Tak — wersje cache zawierają poświadczenia (credentials), wewnętrzne adresy IP lub wrażliwe informacje  

### Czy plik `robots.txt` nieumyślnie ujawnia wrażliwe ścieżki?
- [ ] Nie — `robots.txt` **nie** ujawnia wrażliwych struktur katalogów  
- [ ] Tak — `robots.txt` zawiera listę wrażliwych ścieżek, które ułatwiają rekonesans atakującemu  
- [ ] Plik `robots.txt` nie istnieje  

### Czy usługi zewnętrzne (Shodan, Censys, Wayback Machine) ujawniają historyczne lub obecne punkty ekspozycji?
- [ ] Nie — brak istotnych znalezisk w zewnętrznych usługach indeksowania  
- [ ] Tak — historyczne migawki lub skany usług ujawniają wrażliwe metadane lub banery usług (service banners)  

---