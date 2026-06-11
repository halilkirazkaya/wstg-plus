## WSTG-BUSL-04 — İşlem Zamanlama Testi (Test for Process Timing)

İşlem zamanlama saldırıları (Process Timing), dahili durumlar veya verilerin varlığı hakkında hassas bilgiler elde etmek için bir uygulamanın belirli istekleri işleme süresinin ölçülmesini içerir. Genellikle erken çıkışlı (early-exit) koşullu mantık, veritabanı aramaları veya sabit zamanlı olmayan kriptografik karşılaştırmaların neden olduğu milisaniye düzeyindeki yanıt süresi farklarını analiz ederek, saldırganlar geçerli kullanıcı adlarını numaralandırmak (enumerate), parola parçalarını doğrulamak veya belirli kayıtların varlığını teyit etmek için yan kanal analizi (side-channel analysis) gerçekleştirebilir. Bu güvenlik açıkları tipik olarak kimlik doğrulama uç noktalarında, parola sıfırlama formlarında ve arka uç mantığının girdi geçerliliğine göre dallandığı kaynak yoğunluklu API filtrelerinde ortaya çıkar. Bir saldırganın perspektifinden, bu zamanlama farkları, uygulama kullanıcıya özdeş ve genel hata mesajları döndürse bile arka uç verileri hakkındaki gerçekleri ortaya çıkaran bir "mantıksal kehanet" (boolean oracle) görevi görür.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta (Medium) |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Araçlar:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### Uygulama, geçerli ve geçersiz kaynak istekleri için farklı yanıt süreleri sergiliyor mu?
- [ ] Hayır — yanıt süreleri, girdi geçerliliğinden bağımsız olarak aynıdır  
- [ ] Evet — ihmal edilebilir zamanlama farkları mevcuttur ancak istatistiksel olarak anlamlı **değildir**  
- [ ] Evet — tutarlı zamanlama farkları **gözlemlenebilir** düzeydedir ve güvenilir bir kehanet (oracle) sağlar  

### Yanıt sürelerini normalize etmek için sabit zamanlı (constant-time) karşılaştırma fonksiyonları veya yapay gecikmeler uygulandı mı?
- [ ] Evet — tüm hassas karşılaştırma işlemlerine sabit zamanlı (constant-time) algoritmalar **uygulanmıştır** *(En güvenli)*  
- [ ] Evet — yapay gecikmeler veya rastgele gecikme varyasyonları (jitter) **etkindir**, ancak temel mantık sabit zamanlı değildir  
- [ ] Hayır — herhangi bir zamanlama iyileştirmesi **uygulanmamıştır**, bu da mantıksal dallanmaların doğrudan ölçülmesine olanak tanır  

### Bir saldırgan, zamanlama sinyalini ağ gürültüsünden ayırmak için istatistiksel analiz kullanabilir mi?
- [ ] Hayır — ağdaki gecikme dalgalanmaları (jitter) ve uygulama gürültüsü zamanlama analizini **mümkün kılmaz**  
- [ ] Evet — yeterli örneklem büyüklüğü ile zamanlama sinyali gürültüden **ayrıştırılabilir**  
- [ ] Evet — zamanlama farkı (delta) istatistiksel normalleştirme **gerektirmeyecek** kadar büyüktür  

### Oturum açma veya parola sıfırlama işlevleri aracılığıyla kullanıcı numaralandırma (account enumeration) mümkün mü?
- [ ] Hayır — hesap varlığı zamanlama yoluyla tespit edilemez  
- [ ] Evet — zamanlama farklılıkları, uzak bir saldırganın geçerli kullanıcı adlarını veya e-postaları **numaralandırmasına** (enumerate) olanak tanır  

### Kaynak yoğunluklu işlemler (örneğin; dosya işleme, karmaşık aramalar) zamanlama yoluyla veri varlığı sızıntısına neden oluyor mu?
- [ ] Hayır — kaynak tüketimi, bir kaydın bulunup bulunmadığından bağımsız olarak sabittir  
- [ ] Evet — hassas kayıtların varlığı, arka uç işlem süresinin ölçülmesiyle **anlaşılabilir**  

---