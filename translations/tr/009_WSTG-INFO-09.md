## WSTG-INFO-09 — Web Uygulaması Parmak İzi Belirleme (Fingerprinting)

Bir web uygulamasının parmak izini belirlemek (Fingerprinting), belirli framework, İçerik Yönetim Sistemi (CMS) veya temel teknoloji yığını ile bunlarla ilişkili sürümlerin tespit edilmesi sürecidir. Bu keşif (Reconnaissance) aşaması, tespit edilen bileşenlerin bilinen zafiyetler (CVE) ve halka açık exploit'ler (Exploit) ile eşleştirilmesine olanak tanıyarak saldırı yüzeyini önemli ölçüde daralttığı için hayati önem taşır. Bilgiler tipik olarak HTTP yanıt başlıklarından (Response Headers), benzersiz dosya yollarından, çerez (Cookie) adlarından, HTML kaynak kodu meta verilerinden ve statik varlıkların kriptografik özetlerinden (Hash) elde edilir. Teknoloji yığını doğru bir şekilde belirlenerek, bir saldırgan genel otomatik taramalardan sürüm odaklı zayıflıkların hedeflendiği istismar aşamasına geçebilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Bilgisel |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### HTTP yanıt başlıkları teknoloji yığınını veya sürüm numaralarını ifşa ediyor mu?
- [ ] Hayır — hassas başlıklar kaldırılmış veya genel değerler içeriyor  
- [ ] Evet — `X-Powered-By`, `Server` veya `X-AspNet-Version` gibi varsayılan başlıklar mevcut  
- [ ] Evet — başlıklar web sunucusunun veya uygulama framework'ünün belirli, güncel olmayan sürümlerini ifşa ediyor  

### Uygulama framework'ü veya CMS, benzersiz dosya yolları veya adlandırma kuralları aracılığıyla tanımlanabiliyor mu?
- [ ] Hayır — dosya yolları gizlenmiş (Obfuscated) ve temel teknolojiyi açığa çıkarmıyor  
- [ ] Evet — yaygın dizin yapıları (örneğin `/wp-content/`, `/_next/`, `/node_modules/`) görünür durumda  
- [ ] Evet — `robots.txt`, `humans.txt` veya `sitemap.xml` gibi benzersiz dosyalar teknolojiye özgü meta veriler içeriyor  

### HTML kaynak kodu veya statik varlıklar içinde belirli sürüm numaraları ifşa ediliyor mu?
- [ ] Hayır — yorum satırlarında veya varlık parametrelerinde (Asset parameters) sürüm bilgisi bulunamadı  
- [ ] Evet — HTML yorumları veya meta etiketleri (örneğin `<meta name="generator" content="...">`) mevcut  
- [ ] Evet — sürüm dizeleri CSS/JS dosya adlarına eklenmiş (örneğin `jquery.js?v=1.12.4`) ve devre dışı bırakılamıyor  

### Varsayılan hata sayfaları veya yönetim arayüzleri yazılım ortamını açığa çıkarıyor mu?
- [ ] Hayır — uygulama, tüm durum kodları için özel ve genel hata sayfaları kullanıyor  
- [ ] Evet — web sunucusu veya framework için varsayılan hata sayfaları etkin ve sürüm verisi sızdırıyor  
- [ ] Evet — yönetim paneli giriş portalları (örneğin `/wp-admin`, `/phpmyadmin`) erişilebilir ve tanımlanabilir durumda  

### Çerezler oturum yönetimi framework'ünü ifşa ediyor mu?
- [ ] Hayır — oturum çerezleri (Session cookies) genel veya özel adlar kullanıyor  
- [ ] Evet — teknolojiye özgü çerez adları (örneğin `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) kullanılıyor  

---