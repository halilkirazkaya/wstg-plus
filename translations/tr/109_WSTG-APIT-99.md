## WSTG-APIT-99 — GraphQL Testi

GraphQL, istemcilerin belirli veri yapılarını talep etmesine olanak tanıyan API'lar için bir sorgu dilidir; ancak hatalı yapılandırmalar genellikle bilgi ifşası (information disclosure) ve kaynak tükenmesi (resource exhaustion) dahil olmak üzere önemli güvenlik risklerine yol açar. Zafiyetler genellikle etkinleştirilmiş introspection, sorgu derinliği/karmaşıklığı (query depth/complexity) sınırlarının eksikliği ve çözümleyici fonksiyonlar (resolver functions) içindeki yetersiz nesne seviyesinde yetkilendirmeden (Broken Object-Level Authorization - BOLA) kaynaklanır. Saldırganlar, tüm şemayı haritalamak için introspection özelliğini kullanarak, Hizmet Dışı Bırakma (Denial of Service - DoS) saldırılarını tetiklemek için dairesel parçalar (circular fragments) kullanarak veya iç içe geçmiş sorgular (nested queries) aracılığıyla yetkisiz alanlara erişerek bu uç noktaları (endpoints) istismar ederler. Testler, uygulamanın katı erişim kontrolleri ve kaynak yönetimi uyguladığından emin olmak için `/graphql`, `/v1/graphql` veya `/graphiql` uç noktalarına odaklanır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik* |

> *Yetkisiz nesne seviyesinde yetkilendirme (BOLA), PII (Kişisel Olarak Tanımlanabilir Bilgiler) veya idari mutasyonlara (administrative mutations) yetkisiz erişime izin veriyorsa önem derecesi Kritik seviyesine çıkar.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Araçlar:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### GraphQL introspection sistemi etkin mi?
- [ ] Hayır — introspection **devre dışıdır** ve şema haritalanamaz  
- [ ] Evet — introspection **etkindir** ancak idari kimlik doğrulaması gerektirir  
- [ ] Evet — introspection **etkindir** ve herkese açıktır *(Yüksek)*  

### Sorgularda kaynak sınırları (derinlik ve karmaşıklık) uygulanıyor mu?
- [ ] Evet — katı sorgu derinliği ve karmaşıklığı sınırları **uygulanmaktadır** ve zorunlu kılınmıştır  
- [ ] Evet — sınırlar **uygulanmaktadır** ancak takma adlar (aliases) veya parçalar (fragments) kullanılarak atlatılabilir  
- [ ] Hayır — hiçbir sınır **uygulanmamaktadır**; bu durum özyinelemeli (recursive) sorgulara ve DoS saldırılarına izin verir  

### Çözümleyicilerde (resolvers) Nesne Seviyesinde Yetkilendirme Hatası (BOLA) var mı?
- [ ] Hayır — her sorgu için alan (field) ve nesne seviyesinde yetkilendirme **uygulanmaktadır**  
- [ ] Evet — bazı alanlara yetkilendirme **uygulanmaktadır** ancak hassas nesneler açığa çıkmaktadır  
- [ ] Evet — yetkilendirme **uygulanmamaktadır**; ID manipülasyonu yoluyla herhangi bir nesneye erişime izin verilmektedir  

### GraphQL mutasyonları (mutations) yetkisiz kullanıma karşı uygun şekilde korunuyor mu?
- [ ] Hayır — şemada mutasyonlar **bulunmamaktadır**  
- [ ] Evet — mutasyonlar geçerli kimlik doğrulaması ve katı yetkilendirme kontrolleri gerektirir  
- [ ] Evet — kimliği doğrulanmamış kullanıcılar için mutasyonlar **mümkündür** veya yetkilendirme eksiktir  

### Hata mesajları hassas uygulama veya şema ayrıntılarını sızdırıyor mu?
- [ ] Hayır — hata mesajları geneldir ve dahili mantığı **ifşa etmez**  
- [ ] Evet — hata mesajları temel veritabanı türlerini veya "Bunu mu demek istediniz...?" (Did you mean...?) aracılığıyla önerileri ifşa eder  
- [ ] Evet — yanıtlarda tam yığın izleme (full stack traces) ve hassas yapılandırma verileri **ifşa edilmektedir**  

---