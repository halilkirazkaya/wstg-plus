## WSTG-CONF-01 — Testowanie konfiguracji infrastruktury sieciowej

Testowanie konfiguracji infrastruktury sieciowej polega na identyfikowaniu słabości w bazowych komponentach serwerowych i sieciowych obsługujących aplikację internetową. Atakujący biorą na cel błędnie skonfigurowane usługi, przestarzałe protokoły i niepotrzebnie otwarte porty, aby uzyskać punkt wejścia, przemieszczać się bocznie lub przeprowadzić rekonesans. Częste znaleziska obejmują niebezpieczne wersje TLS, szczegółowe banery usług (verbose banners) oraz wystawione interfejsy administracyjne, które powinny być ograniczone wyłącznie do sieci wewnętrznych. Wykorzystując te wady na poziomie infrastruktury, przeciwnik może naruszyć poufność danych w tranzycie lub uzyskać nieautoryzowany dostęp do systemu operacyjnego hosta.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Niski / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Narzędzia:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Czy na hoście aplikacji znajdują się niepotrzebne otwarte porty lub wystawione usługi?
- [ ] Nie — dostępne są tylko wymagane porty (np. 443)  
- [ ] Tak — dodatkowe usługi są wystawione, ale posiadają bezpieczną konfigurację  
- [ ] Tak — zbędne usługi (np. FTP, Telnet, SMB) są wystawione publicznie  

### Czy banery usług lub nagłówki ujawniają wrażliwe informacje o wersjach?
- [ ] Nie — banery są usunięte lub ogólne  
- [ ] Tak — banery ujawniają oprogramowanie serwera (np. Apache/nginx), ale nie numery wersji  
- [ ] Tak — szczegółowe banery ujawniają konkretne wersje oprogramowania, ułatwiając eksploatację (Exploit)  

### Czy konfiguracja TLS jest zgodna ze współczesnymi najlepszymi praktykami bezpieczeństwa?
- [ ] Tak — włączone są wyłącznie protokoły TLS 1.2/1.3 z silnymi zestawami szyfrów (cipher suites)  
- [ ] Tak — starsze protokoły (TLS 1.0/1.1) są włączone  
- [ ] Nie — słabe szyfry lub niebezpieczne protokoły (SSLv2/v3) są włączone  

### Czy interfejsy administracyjne lub do zarządzania są ograniczone do autoryzowanych sieci?
- [ ] Tak — interfejsy (np. SSH, RDP, panele sterowania) nie są dostępne z publicznego Internetu  
- [ ] Nie — interfejsy są publicznie dostępne, ale wymagają uwierzytelniania wieloskładnikowego (MFA)  
- [ ] Nie — interfejsy są publicznie dostępne przy użyciu uwierzytelniania jednoskładnikowego lub domyślnych poświadczeń  

---