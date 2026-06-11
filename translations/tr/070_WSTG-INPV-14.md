## WSTG-INPV-14 — İnkübe Edilmiş (Incubated) Zafiyetlerin Test Edilmesi

İnkübe edilmiş zafiyetler (Incubated vulnerabilities), kalıcı (persistent) veya ikinci derece (second-order) zafiyetler olarak da adlandırılır; kötü niyetli bir Payload'un uygulama tarafından "soğuk" (cold) bir durumda saklanması ve daha sonra farklı bir bağlamda çalıştırılması veya işlenmesi durumunda ortaya çıkar. Bu saldırı deseni tipik olarak, verileri yeterli yeniden doğrulama veya çıktı kodlaması (output encoding) yapmadan paylaşılan bir veritabanından veya dosya sisteminden alan arka uç (back-end) sistemlerini, yönetici konsollarını veya otomatik raporlama araçlarını hedef alır. Pentester'lar, kullanıcı girişinin yaşam döngüsünü giriş noktasından ("inkübatör") bu verinin nihayetinde diğer bileşenler veya ikincil uygulamalar tarafından işlendiği veya kullanıldığı her konuma kadar izlemelidir. Başarılı bir sömürü (exploitation), genellikle saklı XSS (Stored XSS) yoluyla yönetici oturumu ele geçirme veya dahili raporlama modüllerinde ikinci derece SQL Injection aracılığıyla veri sızdırma (data exfiltration) gibi yüksek etkili sonuçlara yol açar.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Araçlar:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Kullanıcı kontrollü girişlerin, diğer kullanıcılar veya süreçler tarafından daha sonra alınmak üzere saklandığı uç noktalar (endpoints) var mı?
- [ ] Hayır — uygulama kullanıcı girişini daha sonra kullanmak üzere **saklamıyor**  
- [ ] Evet — giriş; veritabanı alanlarında, günlük (log) dosyalarında veya yapılandırma ayarlarında **saklanıyor**  

### Veriler kalıcı depolama katmanına yazılmadan önce girdi doğrulaması (input validation) veya temizleme (sanitization) uygulanıyor mu?
- [ ] Evet — giriş noktasında katı izin listesi (allow-list) doğrulaması **uygulanıyor**  
- [ ] Evet — genel temizleme **uygulanıyor** ancak kodlama veya standart dışı karakterler aracılığıyla atlatma (bypass) **mümkün**  
- [ ] Hayır — giriş ham, doğrulanmamış formunda saklanıyor  

### Saklanan veriler yönetici veya ikincil arayüzlerde işlendiğinde (render) uygun şekilde kodlanıyor veya temizleniyor mu?
- [ ] Evet — verinin işlendiği her yerde bağlam duyarlı çıktı kodlaması (context-aware output encoding) **uygulanıyor**  
- [ ] Evet — bazı konumlarda kodlama **uygulanıyor** ancak diğerlerinde (örneğin dahili yönetici paneli) **eksik**  
- [ ] Hayır — veri herhangi bir kodlama veya temizleme yapılmadan işleniyor ve payload yürütülmesine izin veriyor  

### Veritabanında saklanan payload'lar arka uç toplu iş (batch) süreçlerini veya bant dışı (out-of-band) izleme sistemlerini etkileyebilir mi?
- [ ] Hayır — arka uç süreçleri güvenli ayrıştırma (parsing) yöntemleri veya parametreli sorgular (parameterized queries) kullanıyor  
- [ ] Evet — saklanan payload'lar arka uç mantığında etkileşimleri **tetikleyebilir** (örneğin CSV Injection, günlüklerde Command Injection)  
- [ ] Evet — saklanan payload'lar saldırgan kontrollü sunuculara bant dışı (OOB) istekleri **tetikleyebilir**  

---