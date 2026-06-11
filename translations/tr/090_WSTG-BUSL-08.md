## WSTG-BUSL-08 — Beklenmeyen Dosya Türlerinin Yüklenmesinin Test Edilmesi

Beklenmeyen dosya türlerinin yüklenmesi, saldırganların uygulamanın işlemeyi amaçlamadığı dosyaları göndererek iş mantığı kısıtlamalarını atlamasına olanak tanır ve potansiyel olarak Remote Code Execution (RCE), Cross-Site Scripting (XSS) veya Denial of Service (DoS) saldırılarına yol açabilir. Saldırganlar, sunucu tarafındaki doğrulama mantığını kötü amaçlı içeriği kabul etmesi için kandırmak amacıyla dosya uzantılarını, MIME türlerini ve Magic Bytes (sihirli baytlar) değerlerini değiştirerek bu zafiyetleri yoklarlar. Bu durum tipik olarak, arka uçta (back-end) yetersiz doğrulamanın olduğu profil resmi yüklemelerinde, doküman yönetim sistemlerinde veya destek talebi eklerinde meydana gelir. Başarılı bir istismar (Exploit), yüklenen dosyanın çalıştırılması durumunda ana sunucuyu tehlikeye atabilir veya dosyanın yanlış bir Content-Type ile diğer kullanıcılara sunulması durumunda istemci tarafı (client-side) saldırılarına yol açabilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Uygulama, dosya yüklemelerini belirli bir uzantı kümesiyle sınırlandırıyor mu?
- [ ] Evet — katı bir uzantı izin listesi (whitelist) uygulanmaktadır ve atlatılması (bypass) **mümkün değildir**  
- [ ] Evet — uzantı izin listesi mevcuttur ancak çift uzantı veya null byte'lar ile atlatılması **mümkündür**  
- [ ] Hayır — herhangi bir dosya uzantısı **yüklenebilir**  

### Dosya içeriği, dosya uzantısının ötesinde doğrulanıyor mu?
- [ ] Evet — sunucu tarafı mantığı Magic Bytes doğrulaması yapar ve derin içerik incelemesi gerçekleştirir  
- [ ] Evet — sunucu tarafı mantığı MIME türünü yalnızca `Content-Type` başlığı üzerinden doğrular  
- [ ] Hayır — uzantı kontrolünden sonra herhangi bir içerik doğrulaması **uygulanmamaktadır**  

### Bir saldırgan, web shell veya betik yükleyerek kod çalıştırabilir mi?
- [ ] Hayır — yüklenen dosyalar yürütülemez (non-executable) bir dizinde veya bir depolama biriminde (S3/Azure Blob) saklanmaktadır  
- [ ] Evet — dosyalar web üzerinden erişilebilir bir dizinde saklanmaktadır ancak sunucu yapılandırması yoluyla çalıştırma **devre dışı bırakılmıştır**  
- [ ] Evet — yüklenen kötü amaçlı betikler, öngörülebilir bir URL yolu üzerinden doğrudan **çalıştırılabilir** *(Kritik)*  

### Yüklenen dosya türleri üzerinden XSS gibi istemci tarafı saldırıları mümkün müdür?
- [ ] Hayır — dosyalar `Content-Disposition: attachment` ve `X-Content-Type-Options: nosniff` başlıkları ile sunulmaktadır  
- [ ] Evet — SVG veya HTML dosyaları **yüklenebilir** ve tarayıcıda işlenerek XSS'e yol açabilir  
- [ ] Evet — görüntü meta verileri (EXIF) **işlenmekte** ve temizleme (sanitization) yapılmadan kullanıcı arayüzünde yansıtılmaktadır  

---