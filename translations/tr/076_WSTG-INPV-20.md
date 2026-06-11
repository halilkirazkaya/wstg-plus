## WSTG-INPV-20 — Mass Assignment

Mass Assignment (Toplu Atama), Overposting (Aşırı Gönderim) veya parametrelerin Güvensiz Deserialization (Insecure Deserialization) işlemi olarak da bilinir; bir web uygulamasının, gelen HTTP istek parametrelerini uygun öznitelik filtrelemesi yapmadan dahili nesne özelliklerine otomatik olarak bağlaması durumunda ortaya çıkar. Bu güvenlik zafiyeti, JSON, XML veya form-kodlu verilerin doğrudan uygulama tarafındaki veri modellerine deserialize edildiği modern MVC framework'lerinde ve RESTful API'lerde yaygındır. Saldırganlar, geliştiricilerin kullanıcı kontrolüne açmayı amaçlamadığı hassas alanları değiştirmek için bir isteğe —`isAdmin`, `role` veya `account_balance` gibi— ek ve listede olmayan parametreler enjekte ederek bu davranışı suistimal ederler. Başarılı bir sömürü, genellikle yetki yükseltme (privilege escalation), yetkisiz veri değişikliği veya kritik iş mantığı (business logic) iş akışlarının atlatılması ile sonuçlanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Orta* |

> *Yönetimsel öznitelikler, izin seviyeleri veya faturalandırma alanları değiştirilebiliyorsa önem derecesi Yüksek olarak kabul edilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### Uygulama, gelen istek parametreleri için otomatik bağlama (automatic binding) kullanıyor mu?
- [ ] Hayır — parametreler manuel olarak belirli değişkenlere veya güvenli nesnelere eşlenmektedir  
- [ ] Evet — otomatik bağlama kullanılmaktadır ancak katı **beyaz listeler (white-lists)** veya DTO'lar (Data Transfer Objects) zorunlu tutulmaktadır  
- [ ] Evet — otomatik bağlama kullanılmaktadır ve hassas dahili öznitelikler **değiştirilebilmektedir**  

### Yönetimsel veya izinle ilgili öznitelikler başarıyla enjekte edilebiliyor mu?
- [ ] Hayır — enjekte edilen parametreler yok sayılır, temizlenir veya bir doğrulama (validation) hatasına neden olur  
- [ ] Evet — ek parametreler kabul edilir ancak arka uç (backend) nesne durumunu **etkilemez**  
- [ ] Evet — `is_admin`, `role` veya `permissions` gibi parametrelerin enjekte edilmesi yetkileri **başarıyla** yükseltir *(Kritik)*  

### Overposting yoluyla hassas iş mantığı veya finansal alanları değiştirmek mümkün mü?
- [ ] Hayır — `price`, `balance` veya `status` gibi alanlar toplu atamaya (mass assignment) karşı korunmaktadır  
- [ ] Evet — hassas iş öznitelikleri, JSON veya form istek gövdesine (request body) eklenerek **değiştirilebilmektedir**  

### Uygulama, parametre tahminini kolaylaştıran dahili nesne yapılarını ifşa ediyor mu?
- [ ] Hayır — yanıtlar yalnızca amaçlanan genel (public) alanları içerir ve dokümantasyon kısıtlıdır  
- [ ] Evet — dahili nesne alanları GET yanıtlarında görünür durumdadır ve bu durum parametre keşfini **mümkün** kılmaktadır  
- [ ] Evet — API dokümantasyonu (Swagger/OpenAPI) atanabilen dahili özellikleri açıkça listelemektedir  

---