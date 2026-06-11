## WSTG-INPV-02 — Depolanmış Cross Site Scripting (XSS)

Kalıcı XSS (Persistent XSS) olarak da bilinen Depolanmış Cross Site Scripting (XSS), bir uygulamanın bir kullanıcıdan veri alması ve bu veriyi yeterli doğrulama (validation) veya kodlama (encoding) yapmadan kalıcı bir veritabanında, dosya sisteminde veya diğer depolama ortamlarında saklaması durumunda oluşur. Bir kurban, daha sonra bu arındırılmamış veriyi geri çağıran ve görüntüleyen bir sayfaya gittiğinde, kötü niyetli betik (script), kurbanın tarayıcı bağlamında uygulamanın kaynağı (origin) altında çalışır. Bu güvenlik zafiyeti, kurbanın özel olarak hazırlanmış bir bağlantıya tıklamasını gerektirmediği için özellikle tehlikelidir; etkilenen sayfayı görüntüleyen herhangi bir kullanıcı hedef haline gelir ve bu durum oturum ele geçirme (session hijacking), yetkisiz işlemler veya kimlik bilgisi toplama (credential harvesting) gibi sonuçlara yol açabilir. Saldırganlar genellikle girdinin diğer kullanıcılar veya yöneticiler için sürekli olarak işlendiği yorum bölümlerini, kullanıcı profillerini, mesaj panolarını ve yönetim günlüklerini hedef alırlar.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Araçlar:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Kullanıcı girdisini daha sonra diğer kullanıcılara göstermek üzere depolayan giriş noktaları var mı?
- [ ] Hayır — uygulama, daha sonra işlenmek üzere kullanıcı tarafından kontrol edilebilen bir girdi depolamamaktadır
- [ ] Evet — girdi depolanıyor (örneğin profiller, yorumlar) ancak bir HTML bağlamında **işlenmiyor**
- [ ] Evet — girdi depolanıyor ve diğer kullanıcılara veya yöneticilere sunuluyor

### Depolanan veriler işlendiğinde çıktı kodlaması (output encoding) veya arındırma (sanitization) uygulanıyor mu?
- [ ] Evet — bağlam duyarlı (context-aware) çıktı kodlaması **uygulanıyor** ve bypass **mümkün değil** *(En güvenli)*
- [ ] Evet — arındırma (örneğin `DOMPurify`) **uygulanıyor** ancak yapılandırma hataları nedeniyle bypass **mümkün**
- [ ] Hayır — veriler, kodlama veya arındırma yapılmadan doğrudan DOM içerisine **aktarılıyor** *(Yüksek)*

### Girdi doğrulaması veya arındırma, alternatif payload'lar kullanılarak bypass edilebilir mi?
- [ ] Hayır — filtreler; çeşitli kodlamaları, standart dışı etiketleri ve olay yöneticilerini (event handlers) güvenli bir şekilde ele alıyor
- [ ] Evet — HTML varlık kodlaması (HTML entity encoding) veya belirli etiketler (örneğin `<svg>`, `<img>`) kullanılarak bypass **mümkün**
- [ ] Evet — poliglot payload'lar veya mutasyon tabanlı XSS (mXSS) ile bypass **mümkün**

### İçerik Güvenliği Politikası (CSP), ikincil bir savunma katmanı sağlıyor mu?
- [ ] Evet — sıkı bir CSP **etkinleştirilmiştir** ve satır içi betiklerin (inline scripts) ve yetkisiz kaynakların çalıştırılmasını engeller
- [ ] Evet — bir CSP **etkinleştirilmiştir** ancak hatalı yapılandırılmıştır (örneğin `unsafe-inline` veya geniş kapsamlı `script-src`)
- [ ] Hayır — uygulama için herhangi bir CSP **etkinleştirilmemiştir**

### "Depolanmış XSS'ten RCE'ye" veya diğer yüksek etkili zincirleme saldırıları (Kör XSS / Blind XSS) gerçekleştirmek mümkün mü?
- [ ] Hayır — etki, istemci tarafındaki tarayıcı bağlamı ile sınırlıdır
- [ ] Evet — depolanmış XSS, dahili panellerdeki yöneticileri hedeflemek (Blind XSS) için **kullanılabilir**
- [ ] Evet — depolanmış XSS, diğer zafiyetlerle (örneğin CSRF, hassas veri sızdırma) **zincirlenebilir**

---