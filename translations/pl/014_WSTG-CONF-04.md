## WSTG-CONF-04 — Przegląd starych kopii zapasowych i plików niepowiązanych pod kątem wrażliwych informacji

Przegląd starych kopii zapasowych i plików niepowiązanych polega na identyfikacji zapomnianych, tymczasowych lub ukrytych plików na serwerze WWW, które nie są przeznaczone do publicznego dostępu. Pliki te często obejmują kopie zapasowe kodu źródłowego (`.zip`, `.bak`), pliki wymiany edytorów tekstu (swap files: `.swp`, `~`) lub metadane systemów kontroli wersji (`.git`, `.svn`), które mogą prowadzić do wycieku wrażliwych informacji. Atakujący wykorzystują zautomatyzowane listy słów (wordlists) oraz narzędzia do wykrywania zasobów, aby metodą Brute Force odgadnąć popularne konwencje nazewnictwa, dążąc do eksfiltracji danych uwierzytelniających do baz danych, zakodowanych na sztywno kluczy API lub logiki ułatwiającej dalszą eksploatację. Podatność ta zazwyczaj wynika z ręcznej konserwacji serwera lub wadliwych potoków CI/CD (Pipelines), które nie oczyszczają katalogu głównego serwera (web root) w środowisku produkcyjnym.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się Wysoki, jeśli odkryte zostaną pliki konfiguracyjne, archiwa kodu źródłowego lub dane uwierzytelniające.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Czy listowanie zawartości katalogów jest włączone na serwerze WWW?
- [ ] Nie — listowanie katalogów (directory listing) jest **wyłączone** i zwraca błąd 403 Forbidden lub niestandardową stronę błędu  
- [ ] Tak — listowanie katalogów jest **włączone** w katalogach niezawierających wrażliwych danych  
- [ ] Tak — listowanie katalogów jest **włączone** w katalogach zawierających kod źródłowy lub wrażliwe pliki *(Krytyczny)*  

### Czy pliki kopii zapasowych z popularnymi rozszerzeniami (np. .bak, .old, .save) są wykrywalne?
- [ ] Nie — powszechne rozszerzenia kopii zapasowych **nie zostały odnalezione** lub są blokowane przez konfigurację serwera  
- [ ] Tak — pliki kopii zapasowych istnieją, ale **nie** zawierają wrażliwych informacji  
- [ ] Tak — pliki kopii zapasowych konfiguracji lub kodu źródłowego **są** dostępne *(Wysoki)*  

### Czy katalogi systemów kontroli wersji (np. .git, .svn) lub metadane są ujawnione?
- [ ] Nie — metadane systemów kontroli wersji **nie występują** lub są poprawnie blokowane  
- [ ] Tak — metadane istnieją, ale pełne repozytorium **nie może** zostać odtworzone  
- [ ] Tak — całe repozytorium kodu źródłowego **może** zostać wyeksfiltrowane poprzez ujawnione metadane  

### Czy w katalogu głównym aplikacji (web root) znajdują się skompresowane archiwa (np. .zip, .tar.gz, .rar)?
- [ ] Nie — nie wykryto wrażliwych plików archiwów za pomocą ataków brute-force ani enumeracji  
- [ ] Tak — znaleziono archiwa, ale są one chronione hasłem lub zawierają publiczne zasoby  
- [ ] Tak — niezaszyfrowane archiwa zawierające kod źródłowy lub dane aplikacji **są** publicznie dostępne  

### Czy aplikacja ujawnia pliki tymczasowe utworzone przez edytory tekstu lub środowiska IDE?
- [ ] Nie — pliki tymczasowe takie jak `.swp`, `~` lub `.DS_Store` **nie występują**  
- [ ] Tak — pliki tymczasowe niezawierające wrażliwych danych **są obecne**  
- [ ] Tak — pliki tymczasowe ujawniają fragmenty kodu źródłowego lub ścieżki wewnętrzne  

---