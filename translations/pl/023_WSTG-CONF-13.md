## WSTG-CONF-13 — Path Confusion

Path Confusion (pomieszanie ścieżek) powstaje w wyniku rozbieżności w sposobie, w jaki różne komponenty internetowe, takie jak serwery reverse proxy, load balancery i serwery aplikacji backendowej, parsują i interpretują ścieżki URL. Atakujący wykorzystują te niespójności, wstrzykując określone znaki, takie jak średniki, zakodowane ukośniki (encoded slashes) lub dot-segments, aby oszukać filtry bezpieczeństwa i uzyskać dostęp do zastrzeżonych punktów końcowych (endpoints) lub zasobów statycznych. Podatność ta może prowadzić do krytycznych błędów bezpieczeństwa, w tym ominięcia uwierzytelniania (authentication bypass), nieautoryzowanego dostępu do danych oraz zatruwania pamięci podręcznej WWW (Web Cache Poisoning), ponieważ warstwa front-end może stosować reguły bezpieczeństwa do ścieżki, którą back-end interpretuje inaczej. Skuteczna eksploatacja zazwyczaj następuje na granicy architektonicznej, gdzie logika routingu i normalizacji żądań różni się w obrębie stosu technologicznego.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Status testu** | Nieprzeprowadzony |
| **Poziom krytyczności** | Średni / Wysoki* |

> *Poziom krytyczności staje się wysoki, jeśli path confusion skutkuje ominięciem uwierzytelniania lub kontroli dostępu do wrażliwych punktów końcowych administracyjnych.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Czy różne warstwy architektury interpretują separatory ścieżek (np. `;`, `#`, `?`) w sposób spójny?
- [ ] Tak — wszystkie warstwy normalizują i interpretują separatory ścieżek identycznie  
- [ ] Nie — istnieją niespójności, ale **nie mogą** one zostać wykorzystane do uzyskania dostępu do zastrzeżonych zasobów  
- [ ] Nie — rozbieżności pozwalają atakującemu na ominięcie filtrów front-end i dotarcie do logiki back-end **jest możliwe**  

### Czy mechanizmy kontroli dostępu mogą zostać pominięte przy użyciu sekwencji path traversal lub zakodowanych znaków?
- [ ] Nie — normalizacja jest stosowana spójnie przed sprawdzeniem zabezpieczeń  
- [ ] Tak — ominięcie jest **możliwe** poprzez sekwencje dot-segment (np. `/admin/..;/`)  
- [ ] Tak — ominięcie jest **możliwe** poprzez znaki zakodowane w adresie URL (np. `%2f`, `%2e%2e%2f`)  

### Czy aplikacja jest podatna na Web Cache Poisoning poprzez path confusion?
- [ ] Nie — logika buforowania nie jest podatna na niejednoznaczność ścieżek ani dodatkowe informacje o ścieżce  
- [ ] Tak — złośliwa treść **może** zostać umieszczona w pamięci podręcznej dla legalnych ścieżek z powodu rozbieżności w mapowaniu  
- [ ] Tak — wrażliwe informacje **mogą** być buforowane w katalogach publicznych poprzez techniki `RCD` (Relative Path Overwrite)  

### Czy serwer bezpiecznie obsługuje „dodatkowe informacje o ścieżce” (PathInfo)?
- [ ] Nie — funkcja jest **wyłączona** lub nie istnieje  
- [ ] Tak — `PathInfo` jest obsługiwane i **nie zakłóca** działania filtrów bezpieczeństwa  
- [ ] Nie — `PathInfo` pozwala atakującemu na dołączenie danych, które dezorientują logikę routingu aplikacji  

---