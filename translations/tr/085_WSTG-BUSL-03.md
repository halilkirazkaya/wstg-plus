## WSTG-BUSL-03 — Bütünlük Kontrollerinin Test Edilmesi

Bütünlük kontrolü testi (Integrity check testing), verilerin çeşitli iş süreçlerinden geçerken veya istemci ile sunucu arasında aktarılırken değiştirilmeden kalmasını ve güvenilirliğinin korunmasını doğrulamaya odaklanır. Saldırganlar; HTTP istekleri, gizli alanlar (hidden fields) veya çerezler (cookies) içerisindeki ürün fiyatları, miktarlar, kullanıcı yetkileri veya işlem durumları gibi kritik parametreleri manipüle ederek (tampering) bu mantığı hedef alırlar. Uygulama; sunucu taraflı doğrulama (server-side verification) veya Karma İşlemi (Hashing) ya da HMAC gibi güçlü bütünlük kontrollerinden yoksunsa, bir saldırgan bu değerleri finansal dolandırıcılık yapmak, kendi yetkilerini yükseltmek veya hedeflenen iş kısıtlamalarını atlatmak (bypass) için manipüle edebilir. Bu test; finansal işlemler, hassas durum geçişleri veya bir aşamanın çıktısının bir sonraki aşama için güvenilir girdi işlevi gördüğü çok adımlı iş akışlarını (workflows) yöneten tüm uygulamalar için hayati önem taşır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Bütünlük hatasının finansal hırsızlığa, yetkisiz yetki yükseltmeye (privilege escalation) veya kalıcı kayıtların değiştirilmesine olanak tanıması durumunda önem derecesi Yüksek (High) hale gelir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Hassas iş parametreleri istemci taraflı manipülasyona (tampering) açık mı?
- [ ] Hayır — hassas değerler yalnızca sunucu tarafında yönetilmektedir  
- [ ] Evet — fiyat, miktar veya durum bayrakları (status flags) gibi değerler dışa aktarılmaktadır ancak şifrelenmiştir  
- [ ] Evet — değerler; gizli alanlar, URL parametreleri veya çerezler içerisinde düz metin (plain text) olarak dışa aktarılmaktadır  

### İstemci kontrollü parametrelere bir bütünlük mekanizması (örneğin; HMAC, Sağlama Toplamı (Checksum)) uygulanıyor mu?
- [ ] Evet — her istekte güçlü bir bütünlük kontrolü uygulanmakta ve doğrulanmaktadır  
- [ ] Evet — bir bütünlük kontrolü uygulanmaktadır ancak zayıf/tahmin edilebilir bir algoritma veya sabit kodlanmış (hardcoded) bir gizli anahtar (secret) kullanılmaktadır  
- [ ] Hayır — hassas parametrelere herhangi bir bütünlük kontrolü uygulanmamaktadır  

### Bütünlük kontrolü bir saldırgan tarafından atlatılabilir (bypass) veya yeniden hesaplanabilir mi?
- [ ] Hayır — imza doğrulaması (signature verification) sıkı bir şekilde uygulanmaktadır ve atlatılması mümkün değildir  
- [ ] Evet — imza parametresi (signature parameter) atlanarak imza doğrulaması devre dışı bırakılabilir  
- [ ] Evet — gizli anahtar veya mantık açığa çıktığı/zayıf olduğu için imza yeniden hesaplanabilir  

### Uygulama, verilerin güvenilir bir kaynağa karşı arka uçta (backend) yeniden doğrulamasını gerçekleştiriyor mu?
- [ ] Evet — sunucu, tüm değerleri veri tabanına karşı yeniden doğrular ve atlatılması mümkün değildir  
- [ ] Evet — sunucu kısmi doğrulama yapar ancak belirli uç durumlar (edge cases) üzerinden atlatılması mümkündür  
- [ ] Hayır — sunucu, arka uç doğrulaması yapmadan istemci tarafından sağlanan verilere güvenir *(Kritik)*  

### Çok adımlı bir sürecin durumunu (state) manipüle etmek mümkün mü?
- [ ] Hayır — oturum durumu (session state) sunucu tarafında yönetilmektedir ve manipüle edilemez  
- [ ] Evet — durum bilgisi istemci üzerinden iletilmektedir ve adımları atlamak veya işlemleri tekrarlamak için manipülasyon mümkündür  

---