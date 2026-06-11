## WSTG-INPV-18 — Sunucu Taraflı Şablon Enjeksiyonu (Server-Side Template Injection) Testi

Sunucu Taraflı Şablon Enjeksiyonu (Server-Side Template Injection - SSTI), bir uygulamanın kullanıcı kontrollü girdiyi yeterli temizleme (sanitization) veya kaçış (escaping) işlemi yapmadan sunucu taraflı bir şablon motoruna (template engine) yerleştirmesiyle oluşur. Bu durum, bir saldırganın sunucuda yürütülen kötü niyetli şablon direktifleri enjekte etmesine olanak tanır ve Uzaktan Kod Çalıştırma (Remote Code Execution - RCE) yoluyla sistemin tamamen ele geçirilmesine yol açabilir. SSTI genellikle Jinja2, Twig veya FreeMarker gibi motorları kullanan dinamik e-posta oluşturma, profil sayfaları ve bildirim sistemleri gibi özelliklerde görülür. Bir saldırgan açısından bu zafiyet, şablon motorunun yerel nesne ve metotlarından yararlanarak hassas ortam değişkenlerini (environment variables) sızdırmak, dahili dosyaları okumak veya sistem komutlarını çalıştırmak için sıklıkla istismar edilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Araçlar:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### Uygulama, kullanıcı girdisini işlemek için sunucu taraflı bir şablon motoru kullanıyor mu?
- [ ] Hayır — uygulama dinamik içerik için sunucu taraflı şablonlar **kullanmıyor**  
- [ ] Evet — şablonlar kullanılıyor ancak kullanıcı girdisi şablon direktiflerine **yerleştirilmiyor**  
- [ ] Evet — kullanıcı girdisi, uygun temizleme işlemi **yapılmadan** şablonlara yerleştiriliyor  

### Girdi alanlarına enjekte edildiğinde temel aritmetik ifadeler değerlendiriliyor mu?
- [ ] Hayır — `{{7*7}}` veya `${7*7}` gibi payload'lar düz metin dizeleri (literal strings) olarak yansıtılıyor  
- [ ] Evet — ifadeler değerlendiriliyor (örneğin `49` sonucu dönüyor), bu da şablon motorunun **zafiyet barındırdığını** doğruluyor  

### Şablon motorunun yerleşik nesnelerine veya metotlarına erişilebiliyor mu?
- [ ] Hayır — tehlikeli nesnelere, metotlara veya yapılandırmalara erişim **kısıtlanmış** veya sandbox (kum havuzu) içine alınmış  
- [ ] Evet — hassas verileri sızdırmak için yerleşik nesnelere (örneğin `self`, `config`, `__class__`) **erişilebiliyor**  
- [ ] Evet — sistem çağrıları veya işletim sistemi kütüphaneleri aracılığıyla Uzaktan Kod Çalıştırma (RCE) **mümkündür**  

### Tehlikeli direktiflerin yürütülmesini engelleyen bir sandbox veya izin verilenler listesi (allowlist) mekanizması var mı?
- [ ] Evet — sıkı bir izin verilenler listesi veya güvenli sandbox mevcut ve atlatılması (bypass) **mümkün değil**  
- [ ] Evet — sandbox mevcut ancak motora özgü belirli payload'lar ile atlatılması (bypass) **mümkündür**  
- [ ] No — şablon motoruna herhangi bir sandbox veya izin verilenler listesi **uygulanmıyor**  

---