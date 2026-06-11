## WSTG-INFO-07 — Uygulama Boyunca Yürütme Yollarının Haritalanması

Yürütme yollarının haritalanması (mapping execution paths), bir web uygulaması içindeki tüm erişilebilir uç noktaların (endpoints), fonksiyonel akışların ve karar verme dallarının sistematik olarak tanımlanmasını içerir. Bu süreç; eski kodlar (legacy code), hata ayıklama arayüzleri (debug interfaces) veya belgelenmemiş API rotaları gibi genellikle modern güvenlik kontrollerinden yoksun olan "karanlık" işlevlerin güvenlik incelemesinden gizli kalmamasını sağlamak için hayati önem taşır. Saldırganlar, uygulamanın yapısını görselleştirmek için otomatik örümcek tarama (spidering) ve manuel keşif yöntemlerinin bir kombinasyonunu kullanır; bu süreçte genellikle yetkisiz yönetici erişimi veya iş mantığı hataları (business logic flaws) gibi zafiyetleri keşfederler. Test uzmanları, girdileri belirli sunucu tarafı yanıtlarıyla ilişkilendirerek, kapsamlı bir test sağlamak amacıyla hedeflenmiş derinlemesine inceleme gerektiren hassas işleme mantıklarını tam olarak belirleyebilirler.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Şiddet** | Bilgilendirme |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### Uygulama, otomatik ve manuel emekleme (crawling) kullanılarak tam olarak dizine eklendi mi?
- [ ] Evet — tüm görünür ve bağlantılı kaynakların kapsamlı haritası **tamamlandı**  
- [ ] Evet — otomatik emekleme (crawling) **tamamlandı** ancak manuel keşif bekleniyor  
- [ ] Hayır — uygulama dizine **eklenmedi**  

### JS analizi veya dizin kaba kuvvet saldırısı (directory brute-forcing) yoluyla gizli veya belgelenmemiş uç noktalar (endpoints) tespit edildi mi?
- [ ] Hayır — yan kanal analizi (side-channel analysis) yoluyla hiçbir belgelenmemiş yol keşfedilmedi  
- [ ] Evet — gizli yollar keşfedildi ancak yetkilendirme olmadan bunlara **erişilemiyor**  
- [ ] Evet — belgelenmemiş uç noktalar **erişilebilir** durumdadır ve hassas işlevsellik sağlamaktadır *(Kritik / Yüksek)*  

### Yürütme yolları, standart dışı parametreler veya başlıklar (headers) aracılığıyla değiştirilebiliyor mu?
- [ ] Hayır — `debug`, `test` veya `admin` gibi parametreler yürütme akışını **etkilemiyor**  
- [ ] Evet — parametreler çıktıyı değiştiriyor ancak güvenlik kontrollerini **atlatmıyor**  
- [ ] Evet — yol değiştiren parametreler **uygulanıyor** ve hedeflenen mantığın atlatılmasına izin veriyor  

### Çok adımlı iş mantığı (örneğin; çok faktörlü kimlik doğrulama, ödeme) akışı tam olarak haritalandı mı?
- [ ] Evet — çok adımlı süreçlerdeki tüm durumlar ve geçişler **tanımlandı**  
- [ ] Hayır — karmaşık durum makinesi (state machine) geçişleri tam olarak haritalanamıyor  

### API dokümantasyon dosyaları (Swagger/OpenAPI) veya istemci tarafı haritaları (Source Maps) ifşa edilmiş mi?
- [ ] Hayır — hiçbir mimari harita veya dokümantasyon dosyası herkese açık şekilde ifşa edilmedi  
- [ ] Evet — dokümantasyon mevcut ancak kimlik doğrulaması ile **korunuyor**  
- [ ] Evet — Swagger UI, OpenAPI JSON veya JS Source Maps **etkinleştirilmiş** ve herkese açık olarak erişilebilir durumda

---