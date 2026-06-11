## WSTG-CONF-02 — Uygulama Platformu Yapılandırmasının Test Edilmesi

Uygulama platformu yapılandırma testi, yaygın Exploit girişimlerine karşı sıkılaştırıldığından emin olmak için temel web sunucusu, uygulama sunucusu ve framework ayarlarının denetlenmesini içerir. Saldırganlar, yetkisiz erişim elde etmek veya kod çalıştırmak için aktif olarak varsayılan kimlik bilgilerini, yamalanmamış platform zafiyetlerini ve açıkta kalan yönetim arayüzlerini ararlar. Yanlış yapılandırmalar genellikle hassas ortam değişkenlerinin, dahili sistem yollarının ifşa edilmesine veya saldırı yüzeyini genişleten gereksiz örnek uygulamaların varlığına neden olur. Profesyonel bir perspektiften bakıldığında, kötü yapılandırılmış bir platform, hedef ortama ilk erişimi (initial access) sağlamak için yüksek olasılıklı bir giriş noktası görevi görür.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Önem derecesi, yönetim arayüzlerine varsayılan kimlik bilgileriyle erişilebiliyorsa veya örnek betikler Remote Code Execution imkanı tanıyorsa Yüksek olarak kabul edilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Platform üzerinde varsayılan kimlik bilgileri veya hesaplar aktif mi?
- [ ] Hayır — tüm varsayılan hesaplar **devre dışı bırakılmış** veya parolaları değiştirilmiştir  
- [ ] Evet — varsayılan hesaplar mevcuttur ancak dış ağdan **erişilebilir değildir**  
- [ ] Evet — varsayılan hesaplar **aktiftir** ve internet üzerinden erişilebilirdir *(Kritik)*  

### Platform, header'lar veya hata sayfaları aracılığıyla hassas sürüm bilgilerini ifşa ediyor mu?
- [ ] Hayır — sürüm dizinleri **gizlenmiştir** veya geneldir  
- [ ] Evet — detaylı sürüm bilgileri HTTP header'larında (örneğin `Server`, `X-Powered-By`) **ifşa edilmektedir**  
- [ ] Evet — ayrıntılı hata mesajlarında tam stack trace'ler ve dahili sistem yolları **açığa çıkmaktadır**  

### Yönetim veya idari arayüzler uygun şekilde kısıtlanmış mı?
- [ ] Hayır — yönetim arayüzleri **açıkta değildir**  
- [ ] Evet — arayüzler mevcuttur ancak IP allowlisting veya Multi-Factor Authentication ile **kısıtlanmıştır**  
- [ ] Evet — arayüzler (örneğin `/admin`, `/manager`, `/console`) tek faktörlü kimlik doğrulama ile **kamuya açıktır**  

### Production sunucusunda örnek dosyalar, betikler veya dokümantasyon bulunuyor mu?
- [ ] Hayır — gerekli olmayan tüm dosyalar **kaldırılmıştır**  
- [ ] Evet — örnek dosyalar veya dokümantasyon **mevcuttur** ancak hassas bilgi sızdırmamaktadır  
- [ ] Evet — örnek betikler (örneğin `info.php`, `examples/`) **mevcuttur** ve doğrudan bir saldırı vektörü sağlamaktadır  

### Sunucu, güvenli olmayan HTTP metotlarını destekleyecek şekilde mi yapılandırılmış?
- [ ] Hayır — sadece `GET` ve `POST` metotları **etkindir**  
- [ ] Evet — `PUT`, `DELETE` veya `TRACE` gibi potansiyel olarak riskli metotlar **etkindir** ancak kısıtlanmıştır  
- [ ] Evet — riskli metotlar **etkindir** ve yetkisiz dosya değişikliğine veya kimlik bilgisi hırsızlığına izin vermektedir  

---