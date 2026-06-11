## WSTG-INPV-04 — HTTP Parametre Kirliliği (HTTP Parameter Pollution) Testi

HTTP Parametre Kirliliği (HTTP Parameter Pollution - HPP), bir uygulamanın aynı isme sahip birden fazla HTTP parametresi alması ve bunları tutarsız veya güvensiz bir şekilde işlemesi durumunda ortaya çıkar. Saldırgan, yinelenen parametreler sağlayarak uygulamanın dahili mantığını manipüle edebilir; bu durum Web Uygulaması Güvenlik Duvarlarının (Web Application Firewall - WAF), girdi doğrulama filtrelerinin veya erişim kontrol mekanizmalarının atlatılmasına (bypass) yol açabilir. Bu güvenlik açığı, yinelenen parametrelerden ilkini, sonuncusunu veya birleştirilmiş (concatenated) bir versiyonunu seçebilen temel web sunucusuna veya uygulama çatısına (framework) büyük ölçüde bağımlıdır. İstismar girişimi tipik olarak parametreleri arka uç API'lerine (API), veritabanı sorgularına veya URL yönlendirme mekanizmalarına ileten uç noktaları (endpoints) hedef alır; bu da Cross-Site Scripting (XSS) ile yetkisiz veri sızdırma (data exfiltration) arasında değişen etkilere neden olabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Uygulama aynı isme sahip birden fazla HTTP parametresini nasıl işliyor?
- [ ] Hayır — yinelenen parametreler sunucu tarafından **reddediliyor** veya yok sayılıyor  
- [ ] Evet — sunucu tutarlı bir şekilde yalnızca **ilk** veya **son** parametreyi seçiyor  
- [ ] Evet — sunucu değerleri **birleştiriyor**, bu da potansiyel olarak bir enjeksiyona (injection) izin veriyor  

### HPP, WAF veya girdi filtresi gibi güvenlik kontrollerini atlatmak için kullanılabilir mi?
- [ ] Hayır — güvenlik kontrolleri bir parametrenin **tüm** örneklerini doğru şekilde denetliyor  
- [ ] Evet — bir WAF veya filtre yalnızca **ilk** örneği denetliyor, bu da kötü niyetli **ikinci** bir örneğin geçmesine izin veriyor  
- [ ] Evet — değerlerin birleştirilmesi, bir saldırganın tespitten kaçmak için bir Payload'u birden fazla parametreye **bölmesine** (split) olanak tanıyor  

### Uygulama, kirlenmiş parametreleri uygun bir arındırma işlemi yapmadan yanıta yansıtıyor mu?
- [ ] Hayır — parametreler kullanıcı arayüzünde (UI) yansıtılmadan önce arındırılıyor (sanitization) veya kodlanıyor (encoding)  
- [ ] Evet — kirlilik (pollution) yansıtılan içerikle sonuçlanıyor ancak arındırma işlemi **uygulanıyor**  
- [ ] Evet — kirlilik yansıtılan içerikle sonuçlanıyor ve arındırma işlemi **uygulanmıyor**; bu durum XSS veya mantıksal hatalara yol açıyor  

### Dahili API çağrılarına veya arka uç sistemlere iletilen parametreler kirliliğe karşı savunmasız mı?
- [ ] Hayır — arka uç istekleri güvenli, dinamik olmayan oluşturma yöntemleri kullanıyor  
- [ ] Evet — HPP, bir saldırganın arka uç istek yapısına parametre **enjekte etmesine** veya mevcut parametrelerin üzerine **yazmasına** (overwrite) olanak tanıyor  

### Uygulama, parametreler GET ve POST isteklerinde kirlendiğinde farklı bir davranış sergiliyor mu?
- [ ] Hayır — davranış tüm HTTP yöntemlerinde tutarlıdır  
- [ ] Evet — yalnızca GET parametreleri kirliliğe karşı savunmasızdır  
- [ ] Evet — yalnızca POST (body) parametreleri kirliliğe karşı savunmasızdır  

---