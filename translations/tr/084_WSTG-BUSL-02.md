## WSTG-BUSL-02 — İstek Sahteciliği Kabiliyetinin Test Edilmesi

İş mantığı (Business Logic) bağlamında isteklerin sahte olarak oluşturulması (Forging requests), hedeflenen iş akışının veya yetkilendirme seviyesinin dışındaki eylemleri gerçekleştirmek amacıyla uygulama tarafından tanımlanan parametrelerin manipüle edilmesini içerir. Saldırganlar; ödeme sistemleri veya hesap kaydı gibi, durumun (state) istemci tarafındaki parametreler aracılığıyla korunduğu ve sunucunun bu parametreleri güvenilir bir arka uç kaynağına karşı yeniden doğrulamadığı çok adımlı süreçleri hedeflerler. Gizli alanların (hidden fields), JSON değerlerinin veya fiyatlar, miktarlar ya da dahili durum bayrakları (internal status flags) gibi REST parametrelerinin değiştirilmesi yoluyla bir saldırgan; ödeme gereksinimlerini atlatabilir, yetki yükseltebilir (escalate privileges) veya istenmeyen durum değişikliklerini tetikleyebilir. Bu zafiyet genellikle, sunucunun bir işlem sırasında iş mantığı durumuna ilişkin istemci beyanına örtülü olarak güvendiği durum bilgisi koruyan (stateful) web formlarında veya API'lerde ortaya çıkar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik* |

> *Sahte oluşturulan istek; finansal kayba, yetkisiz yönetici eylemlerine veya tam hesap ele geçirmeye (account takeover) olanak tanıyorsa önem derecesi "Kritik" olarak değerlendirilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Uygulama, işlem parametrelerini sunucu tarafındaki bir "doğruluk kaynağına" (source of truth) karşı doğruluyor mu?
- [ ] Evet — tüm hassas parametreler (fiyat, rol, ID) veritabanına karşı doğrulanır ve atlatılması (bypass) **mümkün değildir**  
- [ ] Evet — doğrulama uygulanır ancak Parametre Kirliliği (Parameter Pollution) veya alternatif kodlama (encoding) yoluyla atlatılabilir  
- [ ] Hayır — uygulama, bir işlemin maliyetini veya sonucunu belirlemek için istemci tarafındaki değerlere güvenir *(Kritik)*  

### İş açısından kritik değerler (örneğin miktarlar, tutarlar) yetkisiz değerlerle değiştirilebilir mi?
- [ ] Hayır — negatif değerler, sıfır değerleri veya aşırı yüksek değerler sunucu tarafından reddedilir  
- [ ] Evet — sunucu değiştirilmiş değerleri kabul eder ancak sadece sınırlı bir aralık dahilinde  
- [ ] Evet — negatif veya sıfır değerler gönderilebilir, bu da finansal kayıplara veya mantık atlatmalarına yol açar  

### Çok adımlı süreçlerde durumu korumak için gizli alanlar veya API parametreleri kullanılıyor mu?
- [ ] Hayır — durum (state), sunucu tarafındaki oturumlar (sessions) veya imzalı belirteçler (tokens) aracılığıyla güvenli bir şekilde korunur  
- [ ] Evet — durum istemci üzerinden aktarılır ancak bütünlük kontrolleri (HMAC/İmzalar) **etkindir** ve güçlüdür  
- [ ] Evet — durum istemci üzerinden aktarılır ve bütünlük kontrolleri **devre dışıdır** veya kaldırılabilir  

### Bir iş akışındaki zorunlu ara adımları atlayan istekler oluşturmak mümkün mü?
- [ ] Hayır — sunucu, her işlem için katı bir operasyon sırası dayatır  
- [ ] Evet — adımlar atlanabilir ancak nihai eylem hala önceki adımlardan gelen geçerli verileri gerektirir  
- [ ] Evet — yetkilendirme veya ödeme adımlarını atlamak için nihai "gönder" (submit) veya "tamamla" (complete) isteği doğrudan sahte olarak oluşturulabilir  

---