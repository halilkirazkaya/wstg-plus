## WSTG-CONF-05 — Enumeracja interfejsów administracyjnych infrastruktury i aplikacji

Interfejsy administracyjne stanowią warstwę zarządzania (management control plane) aplikacji oraz jej podstawowej infrastruktury, często przyznając uprawniony dostęp do konfiguracji systemu, danych użytkowników i operacji po stronie serwera. Atakujący priorytetyzują enumerację tych interfejsów poprzez ataki typu Brute Force na katalogi, analizę pliku `robots.txt` lub obserwację przewidywalnych wzorców URL, aby znaleźć punkty wejścia, które mogą nie posiadać solidnego uwierzytelniania lub opierać się na domyślnych danych uwierzytelniających. Wykrycie wyeksponowanego panelu administracyjnego znacząco zwiększa ryzyko całkowitej kompromitacji systemu, nieautoryzowanej modyfikacji danych lub zakłócenia usług. Interfejsy te często znajdują się na niestandardowych portach lub subdomenach, co czyni je głównymi celami dla zautomatyzowanych skanerów i rekonesansu manualnego.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Status testu** | Nie przeprowadzono |
| **Dotkliwość** | Średnia / Wysoka* |

> *Dotkliwość staje się wysoka, jeśli interfejs pozwala na zmiany konfiguracji w skali całego systemu, zarządzanie użytkownikami lub eksfiltrację danych bez uwierzytelniania wieloskładnikowego (MFA).

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Czy interfejsy administracyjne są możliwe do wykrycia poprzez fuzzing katalogów lub rekonesans?
- [ ] Nie — żadne interfejsy administracyjne nie są wyeksponowane ani możliwe do wykrycia  
- [ ] Tak — interfejsy są możliwe do wykrycia, ale dostęp **jest ograniczony** do wewnętrznych zakresów adresów IP  
- [ ] Tak — interfejsy **są** możliwe do wykrycia i publicznie dostępne  

### Czy na wykrytych portalach administracyjnych wymuszane jest uwierzytelnianie?
- [ ] Tak — uwierzytelnianie wieloskładnikowe (MFA) **jest wymuszane**  
- [ ] Tak — uwierzytelnianie jednoskładnikowe **jest wymuszane**  
- [ ] Nie — uwierzytelnianie nie jest wymagane do uzyskania dostępu do interfejsu *(Krytyczne)*  

### Czy ścieżki administracyjne są ukryte lub niestandardowe, aby zapobiec wykryciu?
- [ ] Tak — ścieżki wykorzystują niestandardowe, zrandomizowane lub nieprzewidywalne nazewnictwo  
- [ ] Nie — ścieżki wykorzystują powszechne konwencje nazewnictwa (np. `/admin`, `/manager`, `/console`) i **mogą** być łatwo odgadnięte  

### Czy interfejs zapewnia dostęp do wrażliwych funkcjonalności systemu lub aplikacji?
- [ ] Nie — interfejs jest panelem statusu typu "read-only" o minimalnym wpływie  
- [ ] Tak — interfejs **może** modyfikować konfigurację aplikacji lub dane użytkowników  
- [ ] Tak — interfejs **może** wykonywać polecenia na poziomie systemu lub zarządzać bazą danych  

---