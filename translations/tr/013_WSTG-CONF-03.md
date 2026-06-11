## WSTG-CONF-03 — Hassas Bilgiler İçin Dosya Uzantısı İşleme Testi

Dosya uzantısı işleme testi, web sunucusunun veya uygulama sunucusunun kısıtlanması veya yürütülmesi gereken dosyaları sunarak hassas bilgileri ifşa edip etmediğinin belirlenmesini içerir. Saldırganlar sıklıkla, web kök dizininde bırakılmış olabilecek ve belirli işleyici eşlemelerinin (handler mappings) eksikliği nedeniyle düz metin olarak sunulan yedek dosyaları, yapılandırma dosyaları ve kaynak kodu parçacıklarını (örneğin; `.bak`, `.old`, `.env`, `.inc`) araştırırlar. Bu zafiyet önemlidir çünkü veri tabanı kimlik bilgilerinin, API anahtarlarının ve dahili iş mantığının ifşa edilmesine yol açarak altyapının daha derinlemesine istismar edilmesini (Exploit) kolaylaştırabilir. Bu tür ifşalar genellikle halka açık dizinlerde, yükleme klasörlerinde veya sunucu üzerindeki hatalı yapılandırılmış MIME-type ayarları aracılığıyla gerçekleşir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Kimlik bilgilerini içeren yapılandırma dosyalarına veya tam kaynak koduna erişilebiliyorsa önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Hassas yedekleme veya geçici dosya uzantılarına (örneğin; `.bak`, `.old`, `.swp`, `~`) erişilebiliyor mu?
- [ ] Hayır — sunucu, yaygın yedekleme uzantıları için 403 Forbidden veya 404 Not Found döndürüyor  
- [ ] Evet — sunucu yedekleme dosyalarının indirilmesine izin veriyor ancak bunlar hassas veri **içermiyor**  
- [ ] Evet — sunucu, hassas veriler veya kaynak kodu içeren yedekleme dosyalarının indirilmesine izin veriyor *(Yüksek)*  

### Sunucu, ortam veya sistem yapılandırma dosyalarını (örneğin; `.env`, `.config`, `.yml`, `.ini`) ifşa ediyor mu?
- [ ] Hayır — kısıtlanmış yapılandırma dosyalarına **erişilemiyor** ve 403/404 döndürülüyor  
- [ ] Evet — hassas yapılandırma dosyalarına **erişilebiliyor** ve dahili sırlar veya kimlik bilgileri ifşa ediliyor  

### Alternatif uzantılar eklenerek (örneğin; `.php.txt`, `.jsp.old`, `.aspx.bak`) kaynak kodu elde edilebiliyor mu?
- [ ] Hayır — uzantı manipülasyonuna bakılmaksızın uygulama kodu düz metin olarak **işlenmiyor**  
- [ ] Evet — eklenen uzantı için sunucuda bir işleyici (handler) bulunmadığından kaynak kodu **ifşa ediliyor**  

### Yönetim veya meta veri dizinlerinin (örneğin; `.git/`, `.svn/`, `.DS_Store`) halka açık erişimi engellenmiş mi?
- [ ] Hayır — bu dizinler/dosyalar web kök dizininde **mevcut değil**  
- [ ] Evet — sunucu tarafı yeniden yazma kuralları (rewrite rules) veya izinler nedeniyle erişim **mümkün değil**  
- [ ] Evet — meta veri dizinlerine **erişilebiliyor** ve tam kaynak kodunun yeniden yapılandırılmasına olanak tanıyor  

### Sunucu, birden fazla uzantıya sahip dosya isteklerini (örneğin; `file.php.jpg`) nasıl ele alıyor?
- [ ] Hayır — sunucu, dahili uzantının güvenliğine doğru şekilde öncelik veriyor veya isteği engelliyor  
- [ ] Evet — sunucu dosyayı ilk uzantı olarak işliyor ancak yürütme için atlatma (Bypass) **mümkün değil**  
- [ ] Evet — sunucu son uzantıyı görmezden geliyor, kod yürütülmesine veya kaynak ifşasına izin verilmesi **mümkün**  

---