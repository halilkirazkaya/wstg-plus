## WSTG-CONF-11 — Testowanie przechowywania w chmurze (Cloud Storage)

Testowanie pod kątem błędnych konfiguracji przechowywania w chmurze (Cloud Storage) obejmuje identyfikację i audyt publicznie dostępnych usług przechowywania obiektów (Object Storage), takich jak Amazon S3, Azure Blobs lub Google Cloud Storage. Zasoby te często zawierają wrażliwe dane, w tym kopie zapasowe, kod źródłowy, dane osobowe (PII) lub pliki konfiguracyjne, które są narażone z powodu nadmiernie permisywnych list kontroli dostępu (ACL) lub polityk zarządzania tożsamością i dostępem (IAM). Atakujący enumerują nazwy kontenerów (Buckets) poprzez rekonesans plików JavaScript, źródła HTML oraz techniki Brute Force w celu znalezienia punktów wejścia do eksfiltracji danych lub nieautoryzowanej modyfikacji plików. Eksploitacja może prowadzić do znaczących skutków, w tym pełnych wycieków danych lub możliwości serwowania złośliwych plików z zaufanej domeny.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Status testu** | Nie przeprowadzono |
| **Poziom zagrożenia** | Wysoki / Krytyczny* |

> *Poziom zagrożenia staje się krytyczny, jeśli dostępne są dane osobowe (PII), dane uwierzytelniające lub produkcyjne kopie zapasowe, lub jeśli włączony jest nieuwierzytelniony dostęp do zapisu.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Czy punkty końcowe przechowywania w chmurze (Buckets/Containers) zostały zidentyfikowane i wyenumerowane?
- [ ] Nie — nie użyto żadnych punktów końcowych przechowywania w chmurze lub nie są one wykrywalne  
- [ ] Tak — punkty końcowe zostały zidentyfikowane poprzez analizę kodu lub monitorowanie ruchu  
- [ ] Tak — punkty końcowe zostały odkryte za pomocą techniki Brute Force opartej na konwencji nazewnictwa  

### Czy nieuwierzytelnieni użytkownicy mogą wyświetlić zawartość magazynu chmurowego (Listing)?
- [ ] Nie — listowanie kontenera (Bucket Listing) jest **wyłączone** i zwraca 403 Forbidden lub 404 Not Found  
- [ ] Tak — listowanie jest **włączone**, ale nie ma wrażliwych plików  
- [ ] Tak — listowanie jest **włączone**, a wrażliwe nazwy plików/metadane są widoczne  

### Czy możliwe jest pobieranie wrażliwych plików ze zidentyfikowanych kontenerów?
- [ ] Nie — pliki są chronione przez polityki IAM lub wymagane jest prawidłowe uwierzytelnienie  
- [ ] Tak — pliki są dostępne, ale wymagają podpisanego adresu URL (Signed URL), którego **nie można** łatwo odgadnąć  
- [ ] Tak — wrażliwe pliki (kopie zapasowe, `.env`, PII) są **publicznie dostępne** do pobrania *(Krytyczny)*  

### Czy dozwolone jest nieuwierzytelnione przesyłanie lub modyfikowanie plików?
- [ ] Nie — nieuwierzytelnione przesyłanie (Upload) lub modyfikacje **nie są możliwe**  
- [ ] Tak — wymagane jest uwierzytelnione przesyłanie, ale obejście (Bypass) **jest możliwe** ze względu na słabe warunki polityki  
- [ ] Tak — nieuwierzytelnione zapisywanie, nadpisywanie lub usuwanie plików **jest możliwe** *(Krytyczny)*  

### Czy polityki współdzielenia zasobów między źródłami (CORS) w punkcie końcowym magazynu są restrykcyjne?
- [ ] Tak — CORS jest **wyłączony** lub ograniczony do określonych, zaufanych źródeł (Origins)  
- [ ] Nie — CORS jest **włączony** z symbolem wieloznacznym (Wildcard) `*`, co pozwala na nieautoryzowany dostęp z poziomu przeglądarki  

---