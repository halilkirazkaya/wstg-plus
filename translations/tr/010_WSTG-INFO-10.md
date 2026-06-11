## WSTG-INFO-10 — Uygulama Mimarisini Haritalama

Uygulama mimarisini haritalama; web uygulamasını destekleyen temel teknolojilerin, altyapı bileşenlerinin ve veri akış yollarının belirlenmesini içerir. Test uzmanları; HTTP yanıt başlıklarını (response headers), hata mesajlarını, çerez (cookie) formatlarını ve dosya uzantılarını analiz ederek web sunucuları, uygulama sunucuları, veritabanları ve kullanılan üçüncü taraf entegrasyonlarının bir şemasını yeniden oluşturabilirler. Bu bilgi, platforma özgü exploit'lerin seçilmesine ve yük dengeleyiciler (load balancers) ile WAF'lar gibi yapılandırılmış ara cihazlardaki potansiyel darboğazların veya hatalı yapılandırmaların belirlenmesine olanak tanıdığı için sonraki saldırıların özelleştirilmesinde temel teşkil eder. Bir saldırgan, genel taramadan spesifik yazılım yığınının (software stack) ve birbiriyle bağlantılı bileşenlerinin hedeflenmiş sömürüsüne geçmek için bu yapısal haritayı kullanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgi Düzeyi / Düşük* |

> *Dahili ağ topolojisi veya özel IP adresleri ifşa edilirse Önem Derecesi Orta (Medium) olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Web sunucusu ve uygulama sunucusu teknolojileri doğru bir şekilde tanımlanabiliyor mu?
- [ ] Hayır — etkili sıkılaştırma (hardening) veya karartma (obfuscation) nedeniyle tanımlama **mümkün değil**  
- [ ] Evet — teknoloji türü biliniyor ancak spesifik sürümler **belirlenemiyor**  
- [ ] Evet — teknoloji ve spesifik sürümler, başlıklar veya dosya imzaları (file signatures) aracılığıyla tamamen **ifşa ediliyor** *(Bilgi Düzeyi)*  

### İletişim yolunda ara cihazlar (WAF'lar, Yük Dengeleyiciler, Proxy'ler) tespit edilebiliyor mu?
- [ ] Hayır — hiçbir ara cihaz tespit edilmedi veya cihazlar tamamen şeffaf  
- [ ] Evet — zamanlama veya davranış yoluyla varlıklarından şüpheleniliyor ancak kimlikleri **doğrulanmadı**  
- [ ] Evet — spesifik ürünler (örneğin Cloudflare, Nginx, F5), benzersiz başlıklar veya davranışlar aracılığıyla **tanımlanıyor**  

### Uygulama, dahili dizin yapısı veya ağ topolojisi hakkında bilgi sızdırıyor mu?
- [ ] Hayır — dahili yollar ve IP'ler **ifşa edilmiyor**  
- [ ] Evet — dahili yollar hata mesajlarında veya yığın izlerinde (stack traces) sızdırılıyor ancak dahili IP'ler **sızdırılmıyor**  
- [ ] Evet — hem dahili dizin yapıları hem de özel IP adresleri **ifşa ediliyor**  

### Arka uç veritabanı türü ve sürümü uygulama davranışından çıkarılabiliyor mu?
- [ ] Hayır — veritabanı ayrıntıları **belirlenemiyor**  
- [ ] Evet — veritabanı türü (örneğin PostgreSQL, MSSQL), hata mesajları veya davranış yoluyla **tahmin ediliyor**  
- [ ] Evet — veritabanı türü ve sürümü, yanıt gövdelerinde veya başlıklarında açıkça **ifşa ediliyor**  

### Uygulama, tanımlanabilir bulut hizmet sağlayıcılarında veya belirli üçüncü taraf CDN'lerde mi barındırılıyor?
- [ ] Hayır — barındırma altyapısı **tanımlanamıyor**  
- [ ] Evet — bulut sağlayıcısı (AWS, Azure, GCP) tanımlanıyor ancak spesifik hizmetler **belirlenemiyor**  
- [ ] Evet — spesifik bulut hizmetleri (örneğin S3 bucket'ları, Lambda, Azure Functions) **haritalanmış ve erişilebilir durumda**  

---