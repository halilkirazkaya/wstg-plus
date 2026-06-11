## WSTG-APIT-01 — API Keşfi (API Reconnaissance)

API keşfi (API Reconnaissance), uç noktaları (endpoints), desteklenen yöntemleri ve altta yatan veri yapılarını ortaya çıkarmak amacıyla API saldırı yüzeyinin sistematik olarak tanımlanması ve haritalanması sürecidir. Saldırganlar, belgelenmemiş (gölge - shadow) API'ları, eski güvenlik açıklarına sahip artık kullanılmayan sürümleri ve dahili iş mantığını ortaya çıkaran Swagger veya OpenAPI tanımları gibi açıkta kalan dokümantasyon dosyalarını keşfetmek için bu aşamayı gerçekleştirirler. Öngörülebilir URL yapılarını, istemci tarafı JavaScript dosyalarını ve dizin kaba kuvvet (brute-force) sonuçlarını analiz ederek, bir saldırgan Kırık Nesne Seviyesinde Yetkilendirme (Broken Object Level Authorization - BOLA) veya Toplu Atama (Mass Assignment) saldırıları için hedef belirlemek üzere API'nın kapsamlı bir haritasını oluşturabilir. Bu keşif çalışması, uygulama arka ucunun işlevselliğine dair bir yol haritası elde etmek için genellikle `/api/v1/`, `/swagger.json` veya `/graphql` gibi yaygın yolları hedefler.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgilendirme / Düşük |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Araçlar:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### API dokümantasyon dosyaları (Swagger, OpenAPI, WSDL) herkese açık olarak erişilebilir mi?
- [ ] Hayır — API dokümantasyonu erişilebilir **değildir** veya kimlik doğrulaması ile sınırlandırılmıştır  
- [ ] Evet — Dokümantasyon erişilebilirdir ancak geçerli kimlik bilgileri gerektirir  
- [ ] Evet — API dokümantasyonu (örneğin `/swagger-ui.html`), kimlik doğrulaması olmaksızın **herkese açık olarak erişilebilirdir**  

### Belgelenmemiş veya "gölge" (shadow) API uç noktaları fuzzing yoluyla keşfedilebiliyor mu?
- [ ] Hayır — Dizin ve uç nokta fuzzing işlemi sonucunda belgelenmemiş bir yol bulunamamıştır  
- [ ] Evet — Belgelenmemiş uç noktalar bulunmuştur ancak bunlara yetkilendirme olmaksızın **erişilememektedir**  
- [ ] Evet — Belgelenmemiş uç noktalar bulunmuştur ve geçerli bir yetkilendirme olmaksızın **erişilebilmektedir**  

### Uygulama birden fazla API sürümünü (örneğin /v1/, /v2/, /beta/) ifşa ediyor mu?
- [ ] Hayır — Yalnızca güncel ve sıkılaştırılmış API sürümü erişilebilirdir  
- [ ] Evet — Eski sürümler mevcuttur ancak güncel sürümle **aynı** güvenlik kontrollerini uygulamaktadır  
- [ ] Evet — Eski sürümler (örneğin `/v1/`) erişilebilirdir ve daha yeni sürümlerdeki güvenlik kontrollerinden **yoksundur**  

### İstemci tarafı kaynakları (JavaScript/Mobil Uygulamalar), API uç nokta yapılarını veya anahtarlarını sızdırıyor mu?
- [ ] Hayır — İstemci tarafı kodu, kodun içine gömülmüş (hardcoded) API yolları veya hassas anahtarlar **içermemektedir**  
- [ ] Evet — İstemci tarafı kodu API uç nokta haritalarını içerir ancak hassas anahtar **içermez**  
- [ ] Evet — API anahtarları, gizli bilgiler (secrets) veya yalnızca dahili kullanıma özel uç nokta URL'leri, istemci tarafı kaynaklarında **kodun içine gömülmüş (hardcoded)** haldedir  

### Bilinen uç noktalarda gizli parametreler veya üstbilgiler (headers) keşfedilebiliyor mu?
- [ ] Hayır — Parametre fuzzing işlemi (örneğin `Arjun` ile), gizli girdiler **ortaya çıkarmamıştır**  
- [ ] Evet — Gizli parametreler (örneğin `debug=true`, `admin=1`) keşfedilmiştir ancak **işlevsel değildir**  
- [ ] Evet — Gizli parametreler keşfedilmiştir ve uygulama davranışını değiştirmek için **kullanılabilmektedir**  

---