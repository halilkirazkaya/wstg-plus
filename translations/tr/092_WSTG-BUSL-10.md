## WSTG-BUSL-10 — Ödeme Fonksiyonelliğinin Test Edilmesi

Ödeme fonksiyonelliğinin test edilmesi, bir saldırganın ödeme geçitlerini (Payment Gateway) atlatmasına, sipariş toplamlarını manipüle etmesine veya başarılı işlem sinyallerini taklit etmesine olanak tanıyan iş mantığı (Business Logic) hatalarının tespit edilmesini içerir. Bu zafiyetler genellikle uygulama, istemci tarayıcısı ve Stripe, PayPal gibi üçüncü taraf ödeme işlemcileri veya dahili muhasebe sistemleri arasındaki iletişimde bulunur. Bir saldırgan, iletim sırasındaki fiyat parametrelerini değiştirmeyi, toplam maliyeti düşürmek için negatif miktarlar göndermeyi veya gerçek bir ödeme yapmadan siparişleri tamamlamak için başarılı işlem geri çağırmalarını (Callback) yeniden oynatmayı (Replay) deneyebilir. Başarılı bir istismar (Exploit), doğrudan finansal kayba, envanter tutarsızlığına ve uygulamanın e-ticaret bütünlüğünün potansiyel olarak bozulmasına yol açar.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### İşlem tutarı işlenmeden önce sunucu tarafında doğrulanıyor mu?
- [ ] Evet — tutar, sunucu tarafındaki ürün veritabanına göre sıkı bir şekilde doğrulanmaktadır *(En güvenli)*  
- [ ] Evet — tutar doğrulanmaktadır ancak Parameter Pollution veya para birimi değiştirme yoluyla mantık atlatma (Logic Bypass) **mümkündür**  
- [ ] Hayır — işlem tutarı istemci tarafındaki istekte **değiştirilebilir** ve arka uç tarafından kabul edilir  

### Ürün miktarları, sıfır veya negatif bir toplama neden olacak şekilde manipüle edilebilir mi?
- [ ] Hayır — negatif veya sıfır miktarlar uygulama doğrulama mantığı tarafından reddedilir  
- [ ] Evet — negatif miktarlar sepette kabul edilir ancak toplam tutar ödeme (Checkout) sırasında düzeltilir  
- [ ] Evet — negatif miktarlar toplam sipariş tutarını düşürmek veya kredi elde etmek için **kullanılabilir** *(Kritik)*  

### Uygulama, ödeme geçidi geri çağırmalarını (Callback) veya Webhook'ları güvenli bir şekilde işliyor mu?
- [ ] Evet — geri çağırmalar, taklit edilemeyen (Spoof) geçerli bir kriptografik imza gerektirir  
- [ ] Evet — imzalar kontrol edilir ancak gizli anahtar (Secret) zayıftır veya doğrulama **atlatılabilir**  
- [ ] Hayır — uygulama, ödeme durumunu onaylamak için kimliği doğrulanmamış veya doğrulanmamış geri çağırmalara dayanır  

### Uygulama, ödeme veya çıkış aşamasında yarış durumlarına (Race Condition) karşı savunmasız mı?
- [ ] Hayır — işlemsel bütünlük ve veritabanı kilitleme mekanizmaları **etkindir**  
- [ ] Evet — Race Condition **mümkündür** ancak nihai fiyatı veya sipariş karşılamayı etkilemez  
- [ ] Evet — eşzamanlı istekler, tek bir ödeme için birden fazla sipariş karşılamaya veya hediye kartlarının "mükerrer harcanmasına" (Double-spending) yol **açabilir**  

### İndirim kodları veya sadakat puanları manipülasyona veya yeniden kullanıma tabi mi?
- [ ] Hayır — kodlar tek kullanımlık olarak doğrulanır ve mantıksal kısıtlamalar **uygulanır**  
- [ ] Evet — kodlar yeniden kullanılabilir veya uygun olmayan öğelere uygulanabilir, ancak etki sınırlıdır  
- [ ] Evet — saldırganlar birden fazla çelişen kod **uygulayabilir** veya son kullanma tarihi ve kullanım sınırlarını atlatabilir  

---