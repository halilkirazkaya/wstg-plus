## WSTG-BUSL-09 — Zararlı Dosya Yükleme Testi (Test Upload of Malicious Files)

Dosya yükleme fonksiyonu, kullanıcıların sunucuya veri göndermesine olanak tanıyarak uygulama ortamına zararlı içerik aktarılması için doğrudan bir vektör sağlar. Saldırganlar; Web Shell, kötü amaçlı yazılım (Malware) veya SVG ya da HTML gibi özel olarak yapılandırılmış dosyaları yüklemek için zayıf doğrulama mantıklarını istismar ederek Uzaktan Kod Çalıştırma (Remote Code Execution - RCE) veya Siteler Arası Betik Çalıştırma (Cross-Site Scripting - XSS) elde edebilirler. Bu zafiyet genellikle profil ayarları, doküman yönetim sistemleri ve sunucunun dosya türlerini, içeriğini ve çalıştırma izinlerini düzgün bir şekilde doğrulamadığı ek dosyası işleyicileri gibi özelliklerde bulunur. Başarılı bir istismar genellikle tam sistem ele geçirme, veri sızıntısı (Data Exfiltration) veya sunucunun iç ağ saldırıları için bir atlama noktası (Pivot Point) olarak kullanılmasıyla sonuçlanır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### Uygulama dosya yükleme fonksiyonu sağlıyor mu?
- [ ] Hayır — herhangi bir dosya yükleme fonksiyonu bulunmamaktadır  
- [ ] Evet — fonksiyon, kimliği doğrulanmış kullanıcılar için mevcuttur  
- [ ] Evet — fonksiyon, **kimliği doğrulanmamış** kullanıcılar için mevcuttur *(Kritik)*  

### Sunucu tarafında dosya uzantısı doğrulaması zorunlu tutuluyor mu?
- [ ] Evet — katı bir uzantı izin listesi (Allowlist) **uygulanmaktadır** ve bypass **mümkün değildir**  
- [ ] Evet — bir izin listesi kullanılmaktadır ancak büyük/küçük harf duyarlılığı veya çift uzantı yoluyla bypass **mümkündür**  
- [ ] Evet — bir engelleme listesi (Blocklist) kullanılmaktadır ve bu liste alternatif uzantılarla (örneğin `.phtml`, `.asa`) bypass **edilebilir**  
- [ ] Hayır — sunucu tarafında herhangi bir uzantı doğrulaması **uygulanmamaktadır**  

### Dosya içeriği, uzantı veya MIME türünün ötesinde doğrulanıyor mu?
- [ ] Evet — uygulama sihirli baytları (Magic Bytes) doğrular ve derin içerik incelemesi gerçekleştirir  
- [ ] Evet — uygulama yalnızca kolayca taklit edilebilen (Spoofed) `Content-Type` başlığını kontrol eder  
- [ ] Hayır — uygulama doğrulama için tamamen dosya uzantısına güvenmektedir  

### Yüklenen dosyalar çalıştırma izinlerine sahip bir dizinde mi saklanıyor?
- [ ] Hayır — dosyalar özel bir depolama servisinde (örneğin S3) veya çalıştırılamaz bir dizinde saklanmaktadır  
- [ ] Evet — dosyalar web sunucusunda saklanmaktadır ancak yapılandırma yoluyla çalıştırma **devre dışı bırakılmıştır**  
- [ ] Evet — dosyalar web üzerinden erişilebilir bir dizinde saklanmaktadır ve çalıştırma **etkindir** *(Kritik)*  

### Güvenlik filtreleri dosya adı manipülasyonu ile bypass edilebilir mi?
- [ ] Hayır — dosya adları sunucu tarafından sanitize edilir veya rastgele yeniden oluşturulur  
- [ ] Evet — filtreler null byte'lar (örneğin `shell.php%00.jpg`) kullanılarak bypass **edilebilir**  
- [ ] Evet — dizin atlatma (Path Traversal) karakterleri (örneğin `../../`), dosyaları rastgele dizinlere yüklemek için **kullanılabilir**  

---