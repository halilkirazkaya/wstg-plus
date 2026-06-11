## WSTG-CLNT-05 — CSS Enjeksiyonu Testi (CSS Injection)

CSS Enjeksiyonu (CSS Injection), bir uygulamanın kullanıcı tarafından sağlanan girdilerin, uygun sanitizasyon veya kaçış (escaping) işlemleri yapılmadan bir web sayfasının Basamaklı Stil Şablonlarını (Cascading Style Sheets - CSS) etkilemesine izin vermesiyle oluşur. Genellikle kozmetik bir sorun olarak algılansa da saldırganlar, CSS enjeksiyonunu; öznitelik seçicileri (attribute selectors) ve `background-image` özelliklerini kullanarak harici istekleri tetiklemek suretiyle CSRF token'ları veya oturum ID'leri (session IDs) gibi hassas verileri sızdırmak (exfiltrate) için kullanabilirler. Bu güvenlik zafiyeti genellikle kullanıcı tercihlerinin (örneğin; özel temalar, yazı tipi renkleri) `<style>` blokları veya satır içi (inline) `style` öznitelikleri içinde yansıtıldığı uç noktalarda (endpoints) meydana gelir. Bir saldırganın perspektifinden bakıldığında, başarılı bir istismar; kullanıcı arayüzü manipülasyonuna (UI redressing), yetkisiz düzen modifikasyonu yoluyla oltalama (phishing) saldırılarına veya katı İçerik Güvenlik Politikalarının (Content Security Policy - CSP) JavaScript tabanlı saldırıları engelleyebileceği ortamlarda gizli veri sızıntısına yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Enjeksiyon noktası CSRF token'ları gibi hassas özniteliklerin sızdırılmasına izin veriyorsa veya hassas iş akışlarında UI redressing için kullanılabiliyorsa önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Uygulama, kullanıcı kontrollü girdileri CSS bağlamları içinde yansıtıyor mu?
- [ ] Hayır — girdi stil etiketlerinde, özniteliklerde veya CSS dosyalarında yansıtılmıyor  
- [ ] Evet — girdi yansıtılıyor ancak kesinlikle güvenli alfanümerik değerlerle sınırlı  
- [ ] Evet — girdi bir `style` özniteliği veya `<style>` bloğu içinde yansıtılıyor  

### CSS'e özgü meta karakterler ve fonksiyonlar uygun şekilde sanitize ediliyor mu?
- [ ] Evet — `{`, `}`, `:` gibi karakterler ve `url()` gibi fonksiyonlar uygun şekilde kaçırılıyor (properly escaped)  
- [ ] Evet — sanitizasyon mevcut ancak kodlama (encoding) veya CSS yorumları (comments) yoluyla atlatılması (bypass) mümkün  
- [ ] Hayır — CSS meta karakterlerine hiçbir sanitizasyon uygulanmıyor  

### CSS seçicileri (selectors) kullanılarak veri sızıntısı mümkün mü?
- [ ] Hayır — seçiciler harici istekleri tetikleyemez veya veri sızdıramaz  
- [ ] Evet — öznitelik seçicileri ve `background-image` aracılığıyla kısmi veri sızıntısı mümkün  
- [ ] Evet — otomatik Brute Force teknikleri kullanılarak hassas token'ların tam sızıntısı mümkün  

### Bir İçerik Güvenlik Politikası (CSP), CSS enjeksiyonu riskini azaltıyor mu?
- [ ] Evet — `style-src` 'self' ile sınırlandırılmış ve `img-src` veya `connect-src` kısıtlanmış  
- [ ] Evet — CSP mevcut ancak 'unsafe-inline' kullanıyor veya joker karakterli (wildcard) harici alan adlarına izin veriyor  
- [ ] Hayır — CSS aracılığıyla harici kaynak yüklenmesini önlemek için herhangi bir CSP uygulanmamış  

### Güvenlik zafiyeti UI redressing veya phishing gerçekleştirmek için kullanılabilir mi?
- [ ] Hayır — enjeksiyon kapsamı düzeni (layout) önemli ölçüde değiştirmek için çok sınırlı  
- [ ] Evet — kullanıcı arayüzü modifikasyonu mümkün, bu da kötü niyetli öğelerin üst üste bindirilmesine (overlay) olanak tanıyor  
- [ ] Evet — tam sayfa düzeni kontrolü mümkün, bu da oldukça ikna edici oltalama (phishing) saldırılarını kolaylaştırıyor  

---